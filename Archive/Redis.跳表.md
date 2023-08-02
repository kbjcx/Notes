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




---
