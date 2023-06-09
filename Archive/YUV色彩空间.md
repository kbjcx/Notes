---
aliases: [YUV]
area: 音视频开发
project: 
date: 2023-05-04 12:05
---
---
#### Description
相比于 [[RGB色彩空间]], YUV 更适合用作图像的编码和储存. [[RGB色彩空间|RGB]] 适合图像的采集和显示. 因此, 在存储和编码前要将 RGB 图像转换为 YUV 图像, 而 YUV 图像在显示之前要转换为 RGB.
- 这里显示的时候 YUV 转成 RGB 通常是硬件或者软件内部做了，我们写代码开发的时候这个 YUV 转 RGB 显示到屏幕这个过程通常是透明的.

视觉心理学研究表明，人的视觉系统对光的感知程度可以用两个属性来描述：亮度（luminance）跟色度（chrominance），这里的色度也叫做饱和度或彩度，总之色度的叫法有很多，要注意上下文来区分语义。

然后色度感知包含两个维度：色调（Hue）和色饱和度（saturation）。色调是由光波的峰值定义的，描述的是光的颜色。色饱和度是由光波的谱宽定义的，描述的是光的纯度。

因此 HVS 对色彩的感知主要有 3 个属性：亮度（luminance），色调（Hue）和色饱和度（saturation）。也就是 YUV 色彩空间，Y 代表亮度，U 代表色调，V 代表色饱和度。

经过大量研究实验表明，视觉系统对色度的敏感度是远小于亮度的。所以可以对色度采用更小的采样率来压缩数据，对亮度采用正常的采样率即可，这样压缩数据不会对视觉体验产生太大的影响。简单来说就是用更少的数据/信息来表达色度（chroma），用更多的数据/信息来表达亮度（luminance）

##### **YUV 的三种分类**
1. YIQ: 适用于 NTSC 彩色电视制式
2. YUV: 适用于 PAL 和 SECAM 彩色电视制式
3. YCbCr: 适用于计算机用的显示器

一般在开发中提到的 YUV 都是指**YCbCr**.

YUV 里面的 U 就是 Cb, 即色调. V 就是 Cr, 即色饱和度.
$$
\begin{aligned}
Cb &= Blue - Y\\
Cr &= Red - Y\\
Cg &= Green - Y
\end{aligned}
$$
Cb 代表蓝色色度的分量，是通过 RGB 里面的 B 的值减去 Y 的值得到的.

Cr 代表红色色度的分量，是通过 RGB 里面的 R 的值减去 Y 的值得到.

Cg 代表绿色色色度的分量，是通过 RGB 里面的 G 的值减去 Y 的值得到的.

Cb 跟 Cr 的颜色空间如下图:
![[Pasted image 20230504141251.png|500]]

##### YUV 的三种采样模式
1. 4:4:4, 一个像素占 3 个字节
2. 4:2:2, 一个像素占 2 个字节
3. 4:2:0, 一个像素占 1.5 个字节

- 444 格式存储每个像素 YUV 值
    ![[Pasted image 20230504152659.png|400]]
- 422 格式在水平方向上进行了 2:1压缩, 存储相邻两个像素的平均 UV 值
    ![[Pasted image 20230504152859.png|400]]
- 420 格式在水平和垂直方向都进行了 2:1压缩, 存储相邻 4 个像素的 UV 值 (平均, 加权等方式)
    ![[Pasted image 20230504153035.png|400]]

- ffmpeg 使用的转换命令:
```shell
ffmpeg -i juren.jpg -s 1920*1080 -pix_fmt yuvj444p juren_yuv_444.yuv
ffmpeg -i juren.jpg -s 1920*1080 -pix_fmt yuvj422p juren_yuv_422.yuv
ffmpeg -i juren.jpg -s 1920*1080 -pix_fmt yuvj420p juren_yuv_420.yuv
```
> [!important] 
> YUV420 比 YUV444 少了一半的储存空间, 但是对视觉体验没有影响

> [!attention] 
> YUV 文件格式，没有头信息，没有宽高信息，所以打开 YUV 文件的时候，需要指定宽高，要指定采样格式是 4:4:4. 如果你有一个 YUV 图片，但是你忘记了它的宽高跟采样格式，那你就无法正常显示这个 YUV 图片.

- YUV 格式有三种存储方式: 
    1. **planner**
        平面格式，先连续存储所有像素点的 Y，紧接着存储所有像素点的 U，随后是所有像素点的 V
    1. **semi-planner**
        半平面的 YUV 格式，Y 分量单独存储，但是 UV 分量交叉存储
    1. **packed**
        每个像素点的 Y, U, V 是连续交错存储的


---
#### Source
- [YUV色彩空间 · FFmpeg原理](https://ffmpeg.xianwaizhiyin.net/base-knowledge/raw-yuv.html)