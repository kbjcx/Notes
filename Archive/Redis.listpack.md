---
aliases: [listpack]
area: redis
project: 
date: 2023-08-02 12:23
tags: [TODO]
---
---
#### Content
Redis 在 5.0 新设计一个数据结构叫 listpack，目的是替代压缩列表，它最大特点是 listpack 中每个节点不再包含前一个节点的长度了，压缩列表每个节点正因为需要保存前一个节点的长度字段，就会有连锁更新的隐患

#### listpack 结构设计
listpack 采用了压缩列表的很多优秀的设计，比如还是用一块连续的内存空间来紧凑地保存数据，并且为了节省内存的开销，listpack 节点会采用不同的编码方式保存不同大小的数据
![[Pasted image 20230802214545.png]]

listpack 头包含两个属性，分别记录了 listpack 总字节数和元素数量，然后 listpack 末尾也有个结尾标识，listpack entry 表示 listpack 的节点，节点结构如下：
![[Pasted image 20230802214641.png]]
- **encoding**，定义该元素的编码类型，会对不同长度的整数和字符串进行编码；
- **data**，实际存放的数据；
- **len**，`encoding+data` 的总长度

listpack 没有压缩列表中记录前一个节点长度的字段了，listpack 只记录当前节点的长度，当我们向 listpack 加入一个新元素的时候，不会影响其他节点的长度字段的变化，从而避免了压缩列表的连锁更新问题

> [!faq] listpack 与 ziplist 的区别？
> 1. listpack


---
