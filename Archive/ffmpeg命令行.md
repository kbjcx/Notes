---
aliases: []
area: 音视频开发
project: 
date: 2023-05-04 16:14
---
---
#### Description
ffmpeg 命令行分为 6 个部分: 
![[Pasted image 20230504161733.png]]

1. 可执行文件
2. 输入文件参数
    在 `-i` 前面的参数, 全部作用于当前输入文件
3. 输入文件名
    `-i` 指定输入文件名
4. 输出文件参数
    输出文件名前面的参数都为输出文件参数, 全部作用于输出文件
5. 输出文件名
    输出文件名前面没有 `-i`
6. 全局参数
    与输入输出文件无关的参数

> [!important] 
> **ffmpeg 的命令行参数是没有先后顺序的**
> `ffmpeg -c copy -t 10 -f mpegts juren.ts -ss 00:01:32 -f flv -i juren.flv -y -benchmark` 与上述命令作用相同
---
#### Source
