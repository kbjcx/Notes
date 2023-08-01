---
aliases: [Hash]
area: redis
project: 
date: 2023-08-01 19:46
tags: []
---
---
#### Content
Hash 是一个键值对（key - value）集合，其中 value 的形式如： `value=[{field1，value1}，...{fieldN，valueN}]`。Hash 特别适合用于存储对象
![[Pasted image 20230801200433.png]]

Hash 类型的底层数据结构是由压缩列表或哈希表实现的：
- 如果哈希类型元素个数小于 512 个（默认值，可由 hash-max-ziplist-entries 配置），所有值小于 64 字节（默认值，可由 hash-max-ziplist-value 配置）的话，Redis 会使用压缩列表作为 Hash 类型的底层数据结构
- 如果哈希类型元素不满足上面条件，Redis 会使用哈希表作为 Hash 类型的底层数据结构

> [!tip] 在 Redis 7.0 中，压缩列表数据结构已经废弃了，交由 listpack 数据结构来实现了

#### 应用场景
##### 缓存对象
Hash 类型的 （key，field， value） 的结构与对象的（对象 id，属性，值）的结构相似，也可以用来存储对象

> [!note] 一般对象用 String + Json 存储，对象中某些频繁变化的属性可以考虑抽出来用 Hash 类型存储

#### 常用命令
```c
# 存储一个哈希表key的键值
HSET key field value   
# 获取哈希表key对应的field键值
HGET key field

# 在一个哈希表key中存储多个键值对
HMSET key field value [field value...] 
# 批量获取哈希表key中多个field键值
HMGET key field [field ...]       
# 删除哈希表key中的field键值
HDEL key field [field ...]    

# 返回哈希表key中field的数量
HLEN key       
# 返回哈希表key中所有的键值
HGETALL key 

# 为哈希表key中field键的值加上增量n
HINCRBY key field n  
```

---
