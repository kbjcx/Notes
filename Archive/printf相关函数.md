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
- 创建一个在堆上的缓冲区, 不会有缓存溢出的风险
- 返回的指针需要主动释放
##### `vsprintf`
```cpp
int vsprintf(char *str, const char *format, va_list arg)
```
- 使用参数列表格式化输出到字符串
- 有缓冲区溢出的风险
##### `snprintf`
```cpp
 int snprintf(char *str, int n, char * format, ...);
```
- 指定写入长度, 避免缓冲区溢出的风险
##### `vsnprintf`
```cpp
int vsnprintf(char *str, size_t size, const char *format, va_list arg)
```
- 防止缓冲区溢出
##### `vsaprintf`
```cpp
int vasprintf(char **strp, const char *format, va_list ap);
```
- 可以自动为输出的字符串分配足够的内存空间，在无需提前计算字符串长度或手动分配内存的情况下，将格式化后的字符串输出到字符数组中
- 需要主动释放内存

---
#### Source
