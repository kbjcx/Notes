---
aliases: []
area: 音视频开发
project: 
date: 2023-05-04 19:56
---
---
#### Description
- `avformat_open_input` 
打开输入文件，这个函数会生成 `AVFormatContext`
```cpp
int avformat_open_input(AVFormatContext **ps, const char *filename,
                        ff_const59 AVInputFormat *fmt, AVDictionary            
                        **options)
```
有两种打开输入文件的方式: 
1. `char* filename`
    根据文件名的后缀来猜测以什么样的封装格式打开文件，例如 mp4，m4a 后缀的文件都用 mov 封装格式来打开
2. `AVInputFormat* fmt`
    指定一种格式
可以通过 `AVDictionary **options` 来[[设置解复用器参数]].
> [!tip] 
> FFmpeg 的函数经常会有 options 这个参数，因为每种封装格式都支持很多定制参数的，例如 mp4 封装格式支持 movflags: faststart，这样可以在录制完成之后把 moov 移动到文件头部，这样在网络里面播放 mp4 的时候，就会更快一些

- `avformat_find_stream_info`
探测函数，读取输入文件的一部分信息来分析出各个流的情况，通常用于一些没有头信息的封装格式，例如 MPEG
```cpp
int avformat_find_stream_info(AVFormatContext *ic, AVDictionary **options);
```
与打开输入文件相关的结构体: 
- `AVFormatContext`
    封装格式上下文, 可以理解为 MP4 或者 FLV
- `AVStream`
    容器里面的流

---
#### Source
