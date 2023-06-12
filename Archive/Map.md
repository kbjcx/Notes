---
aliases: []
area: C++
project: 
date: 2023-06-12 20:50
tags: []
---
---
#### Description
##### 特点
- map 为单重映射、multimap 为多重映射
- 主要区别是 map 存储的是无重复键值的元素对，而 multimap 允许相同的键值重复出现，既一个键值可以对应多个值
- map 和 multimap 内部自建了一颗红黑二叉树，可以对数据进行自动排序，插入、删除效率高，所以 map、multimap 里的数据都是有序的，这也是我们通过 map 简化代码的原因

##### 成员函数
- `at()`, `max_size()`, `size()`
- `count()`
- `insert(elem)/insert(pos, elem)/insert(begin, end)`
- `erase(pos)/erase(begin, end)/erase(key)`

##### 底层实现
- map/multimap 都使用红黑树实现
- unordered_map/unordered_multimap 使用哈希表实现
[[哈希表和红黑树比较]]
---
#### Source
