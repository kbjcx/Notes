---
aliases: [SDS]
area: redis
project: 
date: 2023-08-02 16:30
tags: []
---
---
#### Content
Redis 是用 C 语言实现的，但是它没有直接使用 C 语言的 char* 字符数组来实现字符串，而是自己封装了一个名为简单动态字符串（simple dynamic string，SDS） 的数据结构来表示字符串，也就是 Redis 的 String 数据类型的底层数据结构是 SDS


---
