---
area: C++
tags: []
date: 2023-08-24 16:56
---
1. **修饰变量**，说明该变量不可以被改变；
2. **修饰指针**，分为指向常量的指针和指针常量；
3. **常量引用**，经常用于形参类型，即避免了拷贝，又避免了函数对值的修改；
4. **修饰成员函数**，说明该成员函数内不能修改成员变量。因为 `const` 函数中的`*this`是常量，同样只能访问 `const` 函数


---
