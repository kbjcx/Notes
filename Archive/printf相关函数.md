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
##### ``



---
#### Source
