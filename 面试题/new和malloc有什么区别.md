---
area: C++
tags: []
date: 2023-08-24 14:33
---
- **申请的内存所在位置**: new 操作符从*自由存储区（free store*）上为对象动态分配内存空间，而 malloc 函数从堆上动态分配内存
> [!info] 自由存储区
> 只要是使用 malloc 开辟的内存都会在堆上, 而 new 在不重载 operator new ()的前提下, new 底层会默认调用 malloc 来分配内存, 但是当重载 operator new ()之后, 可以自定义内存分配的来源, 也就是自由存储区

- **返回类型安全性**: `new` 操作符内存分配成功时，返回的是对象类型的指针，类型严格与对象匹配，无须进行类型转换，故 `new` 是符合类型安全性的操作符。而 `malloc` 内存分配成功则是返回 `void *` ，需要通过强制类型转换将 `void*`指针转换成我们需要的类型
- **内存分配失败时的返回值**: `new` 内存分配失败时，会抛出 `bad_alloc` 异常，它不会返回 `NULL`；`malloc` 分配内存失败时返回 `NULL`
- `new` 是关键字, 而 `malloc` 是函数调用, `new` 与 `delete` 搭配使用, 而, `malloc` 与 `free` 搭配使用
- **构造函数调用**: `new` 在分配内存时会调用对象的构造函数, 保证了对象会被正确初始化. `malloc` 不会调用构造函数，分配的内存区域中的内容是未定义的，需要手动初始化. 同时 `delete` 会调用对象的析构函数
- **内存大小**: 在使用 `new` 时，分配的内存大小由编译器根据对象类型计算。在使用 `malloc` 时，需要显式指定要分配的字节数
- **对数组的处理**: C++提供了 `new[]`与 `delete[]`来专门处理数组类型, `malloc` 需要自己在返回的地址上处理
- **是否可以被重载**: `opeartor new /operator delete` 可以被重载。`malloc/free` 并不允许重载（函数不允许重载，但是可以通过 hook 技术来进行覆盖）

---
