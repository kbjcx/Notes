---
aliases: []
area: 音视频开发
project: QT推流软件
date: 2023-04-25 21:26
---
---
#### Description
# 开发库选择
## 音视频开发库
每个主流平台都有单独的开发库用以处理音视频数据
- IOS: AVFoundation, AudioUnit 等
- Android: MediaPlayer, MediaCodec 等
- Windows: DirectShow 等
-  跨平台库: ffmpeg
## GUI 界面开发库
- 跨平台的 GUI 界面开发库: QT

# 下载安装环境
## 安装 ffmpeg
- 下载库文件, 将源码编译成 x86, arm 等平台的动态库
- [Download FFmpeg](https://ffmpeg.org/download.html)
- ![[Pasted image 20230425214543.png]]
- 选择 full-shared 的文件, 带有动态库文件
- ![[Pasted image 20230425214631.png]]
- 解压到目录 ffmpeg
- 将 `ffmpeg/bin` 加入环境变量 `path`
## 安装 QT
- 下载 QT 安装文件, 5.15 及以上需要收费使用, 因此下载 5.12.12
- [Index of /archive/qt/5.12/5.12.12](https://download.qt.io/archive/qt/5.12/5.12.12/)
- 安装时选择 MinGW 组件
## 将 ffmpeg 继承到 QT 中
- .pro 文件（ exe 编译的时候）
    - 指导程序的链接过程，是否能找到对应的动态库
    - INCLUDEPATH、LIBS、是 QT 内部自己定义的环境变量。用来存放头文件路径、库文件路径。
    - FFMPEG_HOME ：我们自己定义的变量，用来标记我们的主目录
---
#### Source
1