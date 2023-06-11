---
aliases: []
area: C++
project: 
date: 2023-06-11 20:33
tags: []
---
---
#### Description
特点：
- 固定大小：array 的大小在编译时确定，无法在运行时动态改变。它的大小是类型和指定的元素个数的组合
- 连续存储
- 快速随机访问
- 对于固定大小的数组需求，比自动扩容的 vector 效率更高

成员函数：
- `at()`: 返回指定索引的值，支持边界检查，超过边界会报 out_of_range
- `data()`: 返回指向第一个元素的指针
- `fill()`: 将所有元素设置为指定的值
- `front()`, `back()`, `max_size()`, `swap()`, `size()`

---
#### Source
