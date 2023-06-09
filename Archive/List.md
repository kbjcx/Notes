---
aliases: [双向链表]
area: C++
project: 
date: 2023-06-11 21:05
tags: []
---
---
#### Description
##### 特点
- 非连续存储：与向量式容器（如 vector 和 deque）不同，list 使用链表实现，其中每个元素存储在独立的节点中。这意味着元素在内存中不是连续存储的，每个节点包含了指向前一个节点和后一个节点的指针
- 动态调整大小
- 高效的插入和删除：两端效率高，中间效率低
- 不支持随机访问
- 双向迭代器：list 提供了双向迭代器，支持向前和向后遍历元素。通过迭代器，可以访问、修改和删除 list 中的元素
- 插入和删除操作不会使迭代器失效：在 list 进行插入和删除操作后，已经获取的迭代器仍然保持有效，不会失效。这是因为链表结构的特性，只需更新相邻节点的指针，而不会影响其他迭代器的有效性
- 没有固定的容量限制

##### 成员函数
- `front()`, `back()`
- `push_back()`, `push_front()`
- `insert(pos, elem)/insert(pos, n, elem)/insert(pos, begin, end)`
- `pop_back()`, `pop_front()`
- `erase(begin, end)/erase(elem)`
- `resize()`, `size()`
- `remove()`: 移除链表中与指定值相等的元素
- `unique()`: 移除链表中相邻重复的元素
- `sort()`: 对链表进行排序、
- `reverse()`: 翻转链表
- `splice()`: 将另一个链表插入到指定位置
- `merge()`: 将另一个已排序的链表合并到当前链表
- `swap()` : 交换两个链表

##### forward_list (单向链表)
- 取消了 `size()` 接口来提高效率

---
#### Source
