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
    把流的 ``
---
#### Source
- [如何使用FFmpeg的解码器 · FFmpeg原理](https://ffmpeg.xianwaizhiyin.net/api-ffmpeg/decode.html)