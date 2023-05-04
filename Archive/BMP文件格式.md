---
aliases: [BMP]
area: 音视频开发
project: 
date: 2023-05-04 10:10
---
---
#### Description
比较常见的使用 RGB 模式的文件格式就是 BMP，BMP 全称 Bitmap-File，是微软出的图像文件格式.
- **BMP 格式由以下部分组成**:
    1. BMP 文件头 (14 bytes)
        文件相关信息
    2. BMP 信息头 (通常是 40 bytes)
        图像信息头, 存放图像相关的信息, 例如宽高之类的数据
    3. 调色板 (根据颜色索引大小决定)
        例如 RGB24 为真彩色, 调色板大小为 0 字节
    4. 位图数据
        对于 RGB24 而言, 一个像素大小为 3 字节

```cpp
//位图文件头
typedef __packed struct
{
    uint16_t  bfType ;     //文件标志.固定为'BM',用来识别BMP位图类型
    uint32_t  bfSize ;	   //文件大小,占四个字节, 小端存储
    uint16_t  bfReserved1 ;//保留，总为0
    uint16_t  bfReserved2 ;//保留，总为0
    uint32_t  bfOffBits ;  //从文件开始到位图数据(bitmap data)开始之间的的偏移量
}BITMAPFILEHEADER ;

//位图信息头
typedef __packed struct
{
    uint32_t biSize ;		   	//BITMAPINFOHEADER结构所需要的字数。
    long  biWidth ;		   	    //图象的宽度，以象素为单位
    long  biHeight ;	   	    //图象的高度，以象素为单位
    uint16_t  biPlanes ;	    //为目标设备说明位面数，其值将总是被设为1 
    uint16_t  biBitCount ;	   	//比特数/象素，其值为1、4、8、16、24、或32
    uint32_t biCompression ;  	//图象数据压缩的类型。其值可以是下述值之一：
	                              //BI_RGB：没有压缩；
	                              //BI_RLE8：每个象素8比特的RLE压缩编码，压缩格式由    
	                              //2字节组成(重复象素计数和颜色索引)；  
                                  //BI_RLE4：每个象素4比特的RLE压缩编码，压缩格式由
                                  //2字节组成
  	                              //BI_BITFIELDS：每个象素的比特由指定的掩码决定。
    uint32_t biSizeImage ;		//图象的大小，以字节为单位。当用BI_RGB格式时，可设置
                                //为0  
    long  biXPelsPerMeter ;	    //水平分辨率，用象素/米表示
    long  biYPelsPerMeter ;	    //垂直分辨率，用象素/米表示
    uint32_t biClrUsed ;	  	//位图实际使用的彩色表中的颜色索引数
    uint32_t biClrImportant ; 	//对图象显示有重要影响的颜色索引的数目，如果是0，表示
                                //都重要。 
}BITMAPINFOHEADER ;

typedef __packed struct 
{
    uint8_t rgbBlue ;    //指定蓝色强度
    uint8_t rgbGreen ;	 //指定绿色强度 
    uint8_t rgbRed ;	 //指定红色强度 
    uint8_t rgbReserved ;//保留，设置为0 
}RGBQUAD ;

typedef __packed struct
{ 
	BITMAPFILEHEADER bmfHeader;     //位图文件头
	BITMAPINFOHEADER bmiHeader;     //位图信息头 
	uint32_t RGB_MASK[3];			//调色板用于存放RGB掩码.
	//RGBQUAD bmiColors[256];  
}BITMAPINFO; 


```
---
#### Source
