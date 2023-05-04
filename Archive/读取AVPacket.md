---
aliases: [AVPacket]
area: 音视频开发
project: 
date: 2023-05-04 21:50
---
---
#### Description
`AVPacket` 是一个管理压缩后的媒体数据的结构体, 但是其本身不包含压缩的媒体数据, 而是通过 `data` 指针来指向媒体数据.

这里面的**<font color="#0593A2">媒体数据</font>**通常是一帧视频的数据，或者一帧音频的数据. 但是也有一些特殊情况，这个 `AVPacket` 的 `data` 是空的，只有 `side data` 的数据.  `side data` 是一些附加信息.

##### 相关函数
1. `av_packet_alloc`
    初始化一个 `AVPacket`
2. `av_read_frame`
    从 `ACFormatContext` 容器中读取一个 `AVPacket`, 虽然函数名是 frame, 但是读取的确实 `AVPacket`
3. `av_packet_unref`
    减少 `AVPacket` 对编码数据的引用次数, 减到 0 会释放编码数据的内存.
4. `av_packet_free`
    释放 `AVPacket` 自身的内存, 里面会调用 `av_packet_unref`



---
#### Source
