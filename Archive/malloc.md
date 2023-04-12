---
aliases: []
area: [操作系统, C/C++]
project: 
tag: TODO
date: 2023-04-12 11:48
---
---
#### Description
`malloc` 不是系统调用, 而是 C 库里的函数, 用于动态内存分配, `malloc` 申请内存的时候, 会有两种方式向系统申请堆内存
> [!important] 
> `malloc()` 分配的是虚拟内存地址
1. 通过 `brk()` 系统调用从堆分配内存, 将堆顶指针向高地址移动, 获取新的内存空间「<font color="#0593A2">用户分配的内存小于`128KB`</font>」
    ![[Pasted image 20230412115917.png|500]]
2. 通过 `mmap` 系统调用在文件映射区分配内存, 调用私有匿名映射的方式, 在文件映射区分配一块内存「<font color="#0593A2">用户分配的内存大于`128KB`</font>」
    ![[Pasted image 20230412120035.png|500]]

- 为什么不全部使用 `mmap` 来分配内存?
    

---
#### Source
- [4.2 malloc 是如何分配内存的？ | 小林coding](https://xiaolincoding.com/os/3_memory/malloc.html#linux-%E8%BF%9B%E7%A8%8B%E7%9A%84%E5%86%85%E5%AD%98%E5%88%86%E5%B8%83%E9%95%BF%E4%BB%80%E4%B9%88%E6%A0%B7)