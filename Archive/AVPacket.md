---
aliases: []
area: 音视频开发
project: 
date: 2023-05-08 22:38
tag: TODO
---
---
#### Description
- AVPacket 是存储压缩编码数据相关信息的结构体

##### 重要变量
1. `uint_8* data`
    - 压缩编码的数据
    - 对于 H264 而言, 一个 AVPacket 的 data 相当于一个 NAL (但不是一模一样)
2. `int size`
    - `data` 的大小
3. `int64_t pts`
    - 显示时间戳
4. `int64_t dts`
    - 解码时间戳
5. `int stream_index`
    - 标识该 AVPacket 所属的视频/音频流

---
#### Source
