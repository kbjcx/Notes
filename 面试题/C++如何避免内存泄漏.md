---
area: C++
tags: []
date: 2023-08-24 16:44
---
1. 使用 RAII (ResourceAcquisition Is Initialization, 资源获取即初始化)技法，以构造函数获取资源 (内存), 析构函数释放
1. 相比于使用原生指针，更建议使用智能指针
1. 注意 `new/delete` 和 `new[]/delete[]` 的搭配使用
2. 类的 **copy constructor** 函数，可能造成内存泄漏，当原始对象中有动态分配的成员变量时，默认 **copy constructor** 函数会采用浅拷贝，从而导致析构时，同一个资源被释放两次


---
