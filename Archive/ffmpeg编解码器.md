---
aliases: [编解码器]
area: 音视频开发
project: 
date: 2023-05-05 20:41
---
---
#### Description
##### 解码
###### 相关的结构体
1. `AVCodecContext`
    这个结构体可以是**编码器**的上下文, 也可以是**解码器**的上下文
2. `AVCodec`
    编解码信息
3. `AVCodecParameters`
    编解码参数
4. `AVPacket`
    数据包 (已编码压缩), 这里面的数据通常是一帧视频或音频的数据 (它本身没有编码数据, 只是管理编码数据)
5. `AVFrame`
    解码之后的 YUV 数据, `AVFrame` 跟 `AVPacket` 类似, 都只是管理数据的结构体

###### 相关的 API 函数
1. `avcodec_alloc_context3`
    通过传递 `AVCodec` 编解码信息来初始化上下文
2. `avcodec_parameters_to_context`
    把流的 `AVCodecParameters` 里面的编解码参数复制到 `AVCodecContext`
3. `AVCodec_open2`
    打开一个编码器或者解码器
4. `avcodec_is_open`
    判断一个编码器或者解码器是否打开
5. `avcodec_send_packet`
    往 `AVCodecContext` 解码器发送一个 `AVPacket`
6. `avcodec_receive_frame`
    从 `AVCodecContext` 解码器读取一个 `AVFrame` 

###### 视频解码流程
发送一个 `AVPacket`, 死循环读取解码器, 直到返回 `EAGAIN`, 循环是因为可能有多个 `AVFrame` 需要读取.

如果返回了 `EAGAIN`, 则说明解码器需要更多的 `AVPacket` 才能够解码出 `AVFRame`

如果已经读到文件末尾，没有 `AVPacket` 能从容器里面读出来了, 这时候就需要往解码器发一个 `size` 跟 `data` 都是 0 的 `AVPacket` ，这样解码器就会把它内部剩余的帧，全部都刷出来.

当解码器完全没有帧可以输出时, 就会返回 `AVERROR_EOF`, 表示解码器结束.

---
#### Source
- [如何使用FFmpeg的解码器 · FFmpeg原理](https://ffmpeg.xianwaizhiyin.net/api-ffmpeg/decode.html)