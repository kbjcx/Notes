---
aliases: [栈]
area: C++
project: 
date: 2023-06-12 21:24
tags: []
---
---
#### Description
> [!note] 容器适配器
##### 特点
- stack 的特点是后进先出（一端进出），不允许遍历；任何时候外界只能访问 stack 顶部的元素；只有在移除 stack 顶部的元素后，才能访问下方的元素
- 在标准模板库容器中，vector、deque 和 list 满足上述要求，当然用户也可以自定义一个满足上述要求的容器。通过模板参数可以看出，默认情况下，stack 使用 deque 作为底层容器

##### 成员函数
- `push()`, `pop()`, `top()`, `empty()`, `size()`

---
#### Source
