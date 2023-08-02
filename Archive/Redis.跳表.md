---
aliases: [跳表, skiplist]
area: redis
project: 
date: 2023-08-02 12:19
tags: [TODO]
---
---
#### Content
跳表的优势是能支持平均 O (logN) 复杂度的节点查找
zset 结构体里有两个数据结构：一个是跳表，一个是哈希表。这样的好处是既能进行高效的范围查询，也能进行高效单点查询
```cpp
typedef struct zset {
    dict *dict;
    zskiplist *zsl;
} zset;
```
Zset 对象在执行数据插入或是数据更新的过程中，会依次在跳表和哈希表中插入或更新相应的数据，从而保证了跳表和哈希表中记录的信息一致
Zset 对象能支持范围查询（如 **ZRANGEBYSCORE** 操作），这是因为它的数据结构设计采用了跳表，而又能以常数复杂度获取元素权重（如 **ZSCORE** 操作），这是因为它同时采用了哈希表进行索引

**跳表设计**


---
