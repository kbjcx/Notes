---
aliases: []
area: C++
project: 
date: 2023-06-11 20:24
tags: []
---
---
#### Description
特点：
- 动态大小： 当数组空间内存不足时，都会执行: 分配新空间-复制元素-释放原空间
- 尾部插入和删除高效： 当删除元素时，不会释放限制的空间，所以向量容器的容量 (capacity)大于向量容器的大小 (size)
- 对于删除或插入操作，执行效率不高，越靠后插入或删除执行效率越高
- 快速随机访问
- 连续存储： 底层内存空间连续
- 当旧的存储空间不够时，扩充一个新的空间，空间大小为原来大小的两倍，复制元素到新的空间，并释放旧空间

成员函数：
- `insert(pos, elem)/insert(pos, n, elem)/insert(pos, begin, end)`: 在指定位置插入元素
- `erase(pos)/erase(begin, end)`: 移除元素
- `reverse(pos1, pos2)`: 将 pos1 到 pos2 的元素逆序
- `capacity()`, `size()`, `at()`, `push_back()`, `pop_back()`, `front()`, `back()`
---
#### Source
