---
aliases: [哈希表]
area: redis
project: 
date: 2023-08-02 10:16
tags: [TODO]
---
---
#### Content
**哈希表结构设计**:
```cpp
typedef struct dictht {
    //哈希表数组
    dictEntry **table;
    //哈希表大小
    unsigned long size;  
    //哈希表大小掩码，用于计算索引值
    unsigned long sizemask;
    //该哈希表已有的节点数量
    unsigned long used;
} dictht;
```
可以看到，哈希表是一个数组（`dictEntry **table`），数组的每个元素是一个指向「哈希表节点（`dictEntry`）」的指针



---
