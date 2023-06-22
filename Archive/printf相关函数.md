---
aliases: [printflike]
area: C++
project: 
date: 2023-06-22 19:11
tags: []
---
---
#### Description
##### `printf`
```cpp
/**
* @return 输出字符的长度, 包含'\0'
**/
int printf(const char *format, ...)
```
- 将内容格式化输出到标准输出设备
##### `vprintf`
```cpp
int vprintf(const char *format, va_list arg)
```
##### `dprintf`
```cpp
/**
* @param fd 文件描述符
* @return 输出字符的长度, 包含'\0'
**/
int dprintf(int fd, const char *format, ...);
```
- 将内容输出到文件描述符指定的文件中
##### `fprintf`
```cpp
/**
* @param stream 指向 FILE 对象的指针，该 FILE 对象标识了流
* @return 输出字符的长度, 包含'\0'
**/
int fprintf(FILE *stream, const char *format, ...)
```
- `fprintf` 是将数据以字符的形式写入流中的，即使我们写的是 int 类型的数据，也是转化成了字符进行写入的
- `fwrite` 是以**二进制**的形式进行写入的, 与 `fprintf` 做区分
##### `vfprintf`
```cpp
int vfprintf(FILE *stream, const char *format, va_list arg)
```
- 直接传入可变参数列表, 在于实现自定义的格式化输出函数
##### `sprintf`
```cpp
int sprintf(char *str, const char *format, ...)
```
- 需要注意 `str` 指向的内存空间的地址大小要保证能够存放的下 format 指定的数据内容，否则会出现缓冲区溢出问题，所以这个函数使用起来不安全，有缓冲区溢出的风险
##### `asprintf`
```cpp
int asprintf(char **strp, const char *fmt, ...);
```
- 创建一个在堆上的缓冲区, 

---
#### Source
