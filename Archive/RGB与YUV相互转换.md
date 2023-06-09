---
aliases: []
area: 音视频开发
project: 
date: 2023-05-04 14:14
---
---
#### Description
RGB 转 YUV 的过程实际上就是把 RGB 3分量里面的亮度信息提取出来，放到 Y 分量。再把 RGB 3分量里面的色调，色饱和度信息提取出来放到 U 跟 V 分量.

提取 Y 亮度信息的公式如下：
$$
Y = Kr*R + Kg*G+Kb*B
$$
上面公式中的 K 是一个权重因子，Kr 代表红色通道的权重，Kg 代表绿色通道的权重，Kb 代表蓝色通道的权重，这三个权重加起来等于 1.
> [!attention] 
> 由于得到 Y, Cr 和 Cb 就能够求得 Cg, 因此没有必要存储 Cg.

因为 K 权重因子是需要经过大量的实验才能得到，经过实验研究后发现，K 权重因子会影响压缩率，所以产生了以下标准。

1. BT. 601 标准[1]——标清数字电视（SDTV)
$$
Y = 0.299R + 0.587G + 0.114B
$$
1. BT. 709标准[2]——高清数字电视（HDTV)
$$
Y = 0.2126R + 0.7152G + 0.0722B
$$
1. BT. 2020标准[3]——超高清数字电视（UHDTV)
$$
Y = 0.2627R + 0.6780G + 0.0593B
$$

> [!important] 
> 因为某些原因，亮度这个信息，不能完完全全从 RGB 里面提取出来，总会残留一些亮度信息在 RGB 里面没提取到。所以 K 权重因子根据不同标准也是不同的.
> BT. 2020 标准生成的 YUV 数据在编码系统里面的压缩率是比 BT. 709 小的，虽然现在还没开始进行编码压缩，但是确确实实，K 权重因子会影响压缩效果.

- 以 BT. 601 标准[1]——标清数字电视（SDTV)为例
##### RGB 转换为 YUV
- Cr 的计算过程为: 
$$
\begin{aligned}
Cr &= R-Y\\
R-Y &= R - 0.299R-0.587G-0.114B\\
R-Y &= 0.701R-0.587G-0.114B
\end{aligned}
$$
$R-Y$ 能够取到的最大值为:
$$
(R-Y)_{max} = 0.701R_{max}
$$
$R-Y$ 能够取到的最小值为:
$$
(R-Y)_{min} = -0.587G_{max}-0.114B_{max}
$$
因为 $R_{max} = G_{max} = B_{max}$, 所以最小值可以表示为:
$$
(R-Y)_{min} = -0.701R_{max}
$$
但是由于 Cr 要跟 Y 一起传输, 而 Y 的取值范围为 $[0-R_{max}]$, 因此需要将 Cr 进行归一化, 将 Cr 的取值范围变为 $[-0.5R_{max}, 0.5R_{max}]$, 因此 Cr 表示为:
$$
\begin{aligned}
Cr &= (R-Y)/1.402\\
&=0.713\times(R-Y)
\end{aligned}
$$
- Cb 的计算过程类似, 得到 Cb 表达为:
$$
\begin{aligned}
Cb &= (B - Y)/1.772\\
&=0.564\times (B-Y)
\end{aligned}
$$
##### 将 YUV 转换为 RGB
根据 Cr 和 Cb 的转换公式, 易得:
$$\begin{aligned}
R &= 1.402\times Cr + Y\\
B &= 1.772\times Cb + Y
\end{aligned}$$
至此， R 跟 B 已经求出来了。把 R 跟 B 套进公式 Y:
$$\begin{aligned}
Y & = 0.299\times (1.402\times Cr + Y) + 0.587G + 0.114\times (1.772\times Cb + Y)\\
G &= Y-0.713Cr - 0.344Cb
\end{aligned}$$

---
#### Source
- [RGB与YUV相互转换 · FFmpeg原理](https://ffmpeg.xianwaizhiyin.net/base-knowledge/raw-yuv-to-rgb.html)