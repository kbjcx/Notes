---
aliases: []
area: [操作系统, C/C++]
project: 
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

> [!important] 
> `malloc()` 分配的虚拟内存会比实际需要的内存多 16 个字节, 这 16 个字节可以在调用 `free()` 时确定需要释放的内存大小. 

- 为什么不全部使用 `mmap` 来分配内存?
    因为向系统申请内存, 是需要通过系统调用的, 执行系统调用就需要进入内核态, 然后再回到用户态, 这样频繁调用 `mmap` 会进行频繁的运行态切换, 导致较大的 CPU 消耗. 并且还会发生缺页中断
    
    由于调用 `brk()` 直接从内存池中取出对应的内存块就行, 而且这个虚拟内存地址与物理地址的映射可能已经存在, 这样不仅减少了系统调用, 也减少了缺页中断的次数. 

- 那为什么不全用 `brk()` 来分配内存呢?
    - 使用 `brk()` 分配内存时很可能产生越来越多不可用的小块碎片
---
#### Source
- [4.2 malloc 是如何分配内存的？ | 小林coding](https://xiaolincoding.com/os/3_memory/malloc.html#linux-%E8%BF%9B%E7%A8%8B%E7%9A%84%E5%86%85%E5%AD%98%E5%88%86%E5%B8%83%E9%95%BF%E4%BB%80%E4%B9%88%E6%A0%B7)