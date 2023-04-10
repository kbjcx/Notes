---
area: 操作系统
project: 
tags: []
date: 2023-04-10 15-34
---
---
CPU 写数据时会将数据写入到 CPU 缓存中, 这种情况下 Cache 和内存中的数据就会不一致, 一般采用[[写回]]或[[写直达]]的方法对 CPU Cache 和内存中的数据进行同步

除了内存和 CPU Cache 之间会存在不同步的问题外, CPU 不同核心之间的 Cache 也会存在不同步, 这就是 <font color="#0593A2">CPU 的缓存不一致问题</font>. 其<font color="#0593A2">根本原因</font>就在于不同 CPU 核心之间的 L1, L2 缓存是独立的, 当多个核心对同一个数据进行操作时, 不同核心的缓存就会不一致, 导致执行结果错误. 

- 要同步不同 CPU 核心的缓存数据需要做到以下两点: 
    1. <font color="#0593A2">写传播</font>: 当某个 CPU 核心的 Cache 数据更新时, 必须将这个更新广播到其他核心
    2. <font color="#0593A2">事务串行化</font>: 在每个核心看来, 数据的操作顺序是一样的

- 写传播最常见的实现方式就是总线嗅探
    ![[总线嗅探]]

- MESI 协议基于总线嗅探机制实现了事务的串行化, 并利用状态机降低了总线带宽压力
    ![[MESI协议|MESI]]
---
# Source
- [2.4 CPU 缓存一致性 | 小林coding](https://xiaolincoding.com/os/1_hardware/cpu_mesi.html)