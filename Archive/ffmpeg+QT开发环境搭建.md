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
    - `INCLUDEPATH`、`LIBS`、是 QT 内部自己定义的环境变量。用来存放头文件路径、库文件路径
    - `FFMPEG_HOME` ：我们自己定义的变量，用来标记我们的主目录
```cpp
win32: {
    FFMPEG_HOME=D:\SF\ffmpeg\MJ\ffmpeg-4.3.2-2021-02-27-full_build-shared
    #设置 ffmpeg 的头文件
    INCLUDEPATH += $$FFMPEG_HOME/include

    #设置导入库的目录一边程序可以找到导入库
    # -L ：指定导入库的目录
    # -l ：指定要导入的 库名称
    LIBS +=  -L$$FFMPEG_HOME/lib \
             -lavcodec \
             -lavdevice \
             -lavfilter \
            -lavformat \
            -lavutil \
            -lpostproc \
            -lswresample \
            -lswscale
}
```
- `$$FFMPEG_HOME` 表示对环境变量值的引用，相当于原地展开
- `-L`：设置导入库的目录，以便编译器能够找到导入库（exe 程序编译的时候只需要包含 xxx. dll. a 静态导入库，运行的时候才需要 xxx. dll 文件）
- `-l`：设置需要链接的导入库名称, 导入库名称需要去掉文件名前面的 lib，比如 libavcodec. dll. a 就写成 avcodec
---
#### Source
- [1、ffmpeg+QT开发环境搭建\_ffmpeg qt\_想文艺一点的程序员的博客-CSDN博客](https://blog.csdn.net/vincent3678/article/details/120870939)