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
- 使用可变参数列表


---
#### Source
