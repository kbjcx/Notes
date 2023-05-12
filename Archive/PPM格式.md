---
aliases: [PPM]
area: 音视频开发
project: 
date: 2023-05-12 19:47
---
---
#### Description
- PPM 通过 RGB 三种颜色来显示图像 (**Pixmaps**)
- 每个图像文件开头通过 2 个字节 (Magic Number) 来表明文件的格式以及编码方式
- Magic Number 有以下 6 个:
    1. P1: Bitmap ASCII
    2. P2: Graymap ASCII
    3. P3: Pixmap ASCII
    4. P4: Bitmap Binary
    5. P5: Graymap Binary
    6. P6: Pixmap Binary
ASCII 格式可用文本编辑器打开, 读取对应图像的数据, 比如 PPM 格式的 RGB 值, Binary 格式适合机器阅读, 按照二进制的形式顺序存储图像信息, 不用空格分隔, 图像处理起来效率更高, 占用的空间更小.
---
#### Source
