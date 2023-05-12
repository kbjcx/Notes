---
aliases: [AVFrame]
area: 音视频开发
project: 
date: 2023-05-06 21:08
tag: TODO
---
---
#### Description
- `AVFrame` 结构体一般用于存储原始数据 (即非压缩数据, 对于视频而言是 YUV 和 RGB, 对音频来说是 PCM)
##### 重要变量
1.  `uint8_t *data[AV_NUM_DATA_POINTERS]`
    - 解码后的原始数据 (对视频而言是 YUV, RGB, 对音频而言是 PCM)
    - 一般而言 `AV_NUM_DATA_POINTERS` 定义为 8, 说明可以有 8个通道的 data 数据, 对于 packed 格式的会存到 data[0], 对于 planner 注意把各通量分别存到 data[0], data[1], data[2]
    - 如果这八个通量不够用的话, #TODO 
2. `enum AVPictureType pict_type`
    - 帧类型 (I 帧, B 帧, P 帧), 包含以下类型: 
        ```cpp
        enum AVPictureType {
            AV_PICTURE_TYPE_NONE = 0, ///< Undefined
            AV_PICTURE_TYPE_I,     ///< Intra
            AV_PICTURE_TYPE_P,     ///< Predicted
            AV_PICTURE_TYPE_B,     ///< Bi-dir predicted
            AV_PICTURE_TYPE_S,     ///< S(GMC)-VOP MPEG4
            AV_PICTURE_TYPE_SI,    ///< Switching Intra
            AV_PICTURE_TYPE_SP,    ///< Switching Predicted
            AV_PICTURE_TYPE_BI,    ///< BI type
        }
        ```
3. `AVRational sample_aspect_ratio`
    - 宽高比, 用如下结构体表达分数: 
        ```cpp
        /**
         * rational number numerator/denominator
         */
        typedef struct AVRational{
            int num; ///< numerator
            int den; ///< denominator
        } AVRational;
        ```
4. `int8_t* qscale_table`
    - QP 表
    - 指向一块内存, 里面存储每个宏块的 QP 值, 宏块编号从左往右, 每个宏块对应有个 QP , 宏块的大小为 $16\times 16$, 因此每行的宏块数计算如下: 
       `int mb_stride = pCodecCtx->width/16 + 1`
    - 宏块总数: 
        `int mb_sum = ((pCodecCtx->height+15)>>4)*(pCodecCtx->width/16+1)`
5. `int linesize[AV_NUM_DATA_POINTERS]`
    linesize 是针对不同 data 流的每一行的长度, AVFrame 的 width 是分辨率大小, 但是有可能会因为芯片要求进行 64 位或 128 位对其, 当无法对齐时
---
#### Source
- [FFMPEG结构体分析：AVFrame\_sample\_aspect\_ratio\_雷霄骅的博客-CSDN博客](https://blog.csdn.net/leixiaohua1020/article/details/14214577)