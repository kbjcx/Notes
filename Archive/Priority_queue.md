---
aliases: [优先级队列]
area: C++
project: 
date: 2023-06-12 21:20
tags: []
---
---
#### Description
> [!note] 容器适配器
##### 特点
- 标准模板库中的 vector 和 deque 能够满足上述需求，默认情况下，priority_queue 使用 vector 作为底层容器
- 某种程度上来说，priority_queue 默认在 vector 上使用堆算法将 vector 中元素构造成大顶堆的结构，因此 priority_queue 就是堆，所有需要用到堆的位置，都可以考虑使用 priority_queue
- priority_queue 实现了最高优先级先出

---
#### Source
