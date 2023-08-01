---
aliases: [List]
area: redis
project: 
date: 2023-08-01 15:13
tags: []
---
---
#### Content
> [!info] List 类型的底层数据结构是由双向链表或压缩列表实现的
> 如果列表的元素个数小于 `512` 个（默认值，可由 `list-max-ziplist-entries` 配置），列表每个元素的值都小于 `64` 字节（默认值，可由 `list-max-ziplist-value` 配置），Redis 会使用**压缩列表**作为 List 类型的底层数据结构
> 如果列表的元素不满足上面的条件，Redis 会使用**双向链表**作为 List 类型的底层数据结构

> [!important] **在 Redis 3.2 版本之后，List 数据类型底层数据结构就只由 quicklist 实现了，替代了双向链表和压缩列表**

---
