---
aliases: [CPU高速缓存, CPU Cache]
area: 操作系统
project: 
date: 2023-04-10 14:31
---
---
- CPU 缓存内嵌在 CPU 中, 充当 CPU 与内存之间的缓存
- CPU 缓存分为三级: L1 Cache, L2 Cache 和 L3 Cache, 级别越低的缓存离 CPU 核心越近, 访问速度更快, 但是容量会更小
> [!important]
> 在 CPU 的多个核心中, 每个核心都拥有自身的 L1, L2 Cache, 而 L3 Cache 是所有核心共享使用的
- CPU Cache 的基本组成单位为 CPU Line, 它是从内存中读取数据的基本单位, 一个 CPU Line 由标识和数据块构成
- <font color="#0593A2">CPU 读取数据</font>时希望尽可能从 CPU Cache 中读取, 而非从内存中去取数据, 因此首先会在 L1 Cache 中寻找是否有所需数据, 然后依次去 L2, L3 Cache 中寻找所需数据, 都没找到才去内存中读取数据
- <font color="#0593A2">CPU 写数据</font>时, 将数据写入 CPU Cache, 此时 CPU Cache 中的数据就会与内存中的数据不一致, 就需要将 Cache 中的数据与内存进行同步, 可以采用[[写直达]]或者[[写回]]的方法进行数据写入
---
# Source
-  [2.4 CPU 缓存一致性 | 小林coding](https://xiaolincoding.com/os/1_hardware/cpu_mesi.html)