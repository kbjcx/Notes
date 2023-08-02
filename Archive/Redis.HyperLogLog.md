---
aliases: [HyperLogLog]
area: redis
project: 
date: 2023-08-02 13:58
tags: []
---
---
#### Content
Redis HyperLogLog 是 Redis 2.8.9 版本新增的数据类型，是一种用于「统计基数」的数据集合类型，基数统计就是指统计一个集合中不重复的元素个数

> [!attention] HyperLogLog 是统计规则是基于概率完成的，不是非常准确，标准误算率是 0.81%

> [!tip]  HyperLogLog 提供**不精确的去重计数**
> 在输入元素的数量或者体积非常非常大时，计算基数所需的内存空间总是固定的、并且是很小的

#### 应用场景
##### 百万级网页 UV 计数
Redis HyperLogLog 优势在于只需要花费 12 KB 内存，就可以计算接近 $2^{64}$ 个元素的基数，和元素越多就越耗费内存的 Set 和 Hash 类型相比，HyperLogLog 就非常节省空间
在统计 UV 时，你可以用 **PFADD** 命令（用于向 HyperLogLog 中添加新元素）把访问页面的每个用户都添加到 HyperLogLog 中
```py
PFADD page1:uv user1 user2 user3 user4 user5
```
接下来，就可以用 **PFCOUNT** 命令直接获得 page1 的 UV 值了，这个命令的作用就是返回 HyperLogLog 的统计结果
```py
PFCOUNT page1:uv
```

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
