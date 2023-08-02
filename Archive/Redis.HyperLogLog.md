---
aliases: [HyperLogLog]
area: redis
project: 
date: 2023-08-02 13:58
tags: []
---
---
#### Content
Redis HyperLogLog 是 Redis 2.8.9 版本新增的数据类型，是一种用于「统计基数」的数据集合类型，基数统计就是指统计一个集合中不重复的元素个数。但要注意，HyperLogLog 是统计规则是基于概率完成的，不是非常准确，标准误算率是 0.81%

> [!tip]  HyperLogLog 提供**不精确的去重计数**
> 在输入元素的数量或者体积非常非常大时，计算基数所需的内存空间总是固定的、并且是很小的

#### 常见命令
```py
# 添加指定元素到 HyperLogLog 中
PFADD key element [element ...]

# 返回给定 HyperLogLog 的基数估算值。
PFCOUNT key [key ...]

# 将多个 HyperLogLog 合并为一个 HyperLogLog
PFMERGE destkey sourcekey [sourcekey ...]
```

---
