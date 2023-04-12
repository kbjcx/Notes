---
aliases: []
area: C/C++
project: 
date: 2023-04-12 13:51
---
---
#### Description
- `malloc` 通过 `brk()`方式申请的内存，`free` 释放内存的时候，并不会把内存归还给操作系统，而是缓存在 `malloc` 的内存池中，待下次使用；
- `malloc` 通过 `mmap()` 方式申请的内存，`free` 释放内存的时候，会把内存归还给操作系统，内存得到真正的释放

> [!important] 
> `malloc` 申请内存时会多出 16 字节的内存, 因此靠这 16 个字节就可以确定需要释放的内存有多大
---
#### Source
