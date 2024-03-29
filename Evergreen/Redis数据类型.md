---
area: redis
project: 
tags: []
date: 2023-07-21 22:27
---
---
# Redis 键值对数据库实现
Redis 使用了一个 [[Redis.哈希表]] 保存所有键值对
哈希桶存放的是指向键值对数据的指针（`dictEntry*`），这样通过指针就能找到键值对数据，然后因为键值对的值可以保存字符串对象和集合数据类型的对象，所以键值对的数据结构中并不是直接保存值本身，而是保存了 `void * key` 和 ` void * value` 指针，分别指向了实际的键对象和值对象，这样一来，即使值是集合数据，也可以通过 `void * value` 指针找到

![[Pasted image 20230802162026.png]]

`dictEntry` 结构，表示哈希表节点的结构，结构里存放了 `void * key` 和 `void * value` 指针， **key 指向的是 String 对象，而 value 则可以指向 String 对象，也可以指向集合类型的对象，比如 List 对象、Hash 对象、Set 对象和 Zset 对象**
Redis 中的每个对象都由 `redisObject` 结构表示，如下图：

![[Pasted image 20230802162225.png]]

对象结构里包含的成员变量：
- **type**，标识该对象是什么类型的对象（String 对象、 List 对象、Hash 对象、Set 对象和 Zset 对象）
- **encoding**，标识该对象使用了哪种底层的数据结构
- **ptr**，指向底层数据结构的指针
# String
![[Redis.String|seamless]]

# List
![[Redis.List|seamless]]

# Hash
![[Redis.Hash|seamless]]

# Set
![[Redis.Set|seamless]]

# Zset
![[Redis.Zset|seamless]]

# BitMap
![[Redis.BitMap|seamless]]

# HyperLogLog
![[Redis.HyperLogLog|seamless]]

# GEO
![[Redis.GEO|seamless]]

# Stream
![[Redis.Stream|seamless]]


---
