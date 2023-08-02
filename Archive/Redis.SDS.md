---
aliases: [SDS]
area: redis
project: 
date: 2023-08-02 16:30
tags: []
---
---
#### Content
Redis 是用 C 语言实现的，但是它没有直接使用 C 语言的 char* 字符数组来实现字符串，而是自己封装了一个名为简单动态字符串（simple dynamic string，SDS） 的数据结构来表示字符串，也就是 Redis 的 String 数据类型的底层数据结构是 SDS

- SDS 结构设计
![[Pasted image 20230802163504.png]]

结构中的每个成员变量分别介绍下：
- **len**，记录了字符串长度。这样获取字符串长度的时候，只需要返回这个成员变量值就行，时间复杂度只需要 O（1）。
- **alloc**，分配给字符数组的空间长度。这样在修改字符串的时候，可以通过 alloc - len 计算出剩余的空间大小，可以用来判断空间是否满足修改需求，如果不满足的话，就会自动将 SDS 的空间扩展至执行修改所需的大小，然后才执行实际的修改操作，所以使用 SDS 既不需要手动修改 SDS 的空间大小，也不会出现前面所说的缓冲区溢出的问题。
- **flags**，用来表示不同类型的 SDS。一共设计了 5 种类型，分别是 sdshdr5、sdshdr8、sdshdr16、sdshdr32 和 sdshdr64，后面在说明区别之处。
- **buf[ ]**，字符数组，用来保存实际数据。不仅可以保存字符串，也可以保存二进制数据

**flags**
Redis 一共设计了 5 种类型，分别是 sdshdr5、sdshdr8、sdshdr16、sdshdr32 和 sdshdr64
这 5 种类型的主要区别就在于，它们数据结构中的 **len** 和 **alloc** 成员变量的数据类型不同
```cpp
// 关闭
struct __attribute__ ((__packed__)) sdshdr16 {
    uint16_t len;
    uint16_t alloc; 
    unsigned char flags; 
    char buf[];
};


struct __attribute__ ((__packed__)) sdshdr32 {
    uint32_t len;
    uint32_t alloc; 
    unsigned char flags;
    char buf[];
};
```

#### 优点
1. O（1）复杂度获取字符串长度
2. 二进制安全
    SDS 不需要用 `“\0” ` 字符来标识字符串结尾了，而是有个专门的 len 成员变量来记录长度，所以可存储包含 `“\0”` 的数据
3. 不会发生缓冲区溢出
    当判断出缓冲区大小不够用时，Redis 会自动将扩大 SDS 的空间大小，以满足修改所需的大小
4. 节省内存空间
     Redis 一共设计了 5 种类型，分别是 sdshdr5、sdshdr8、sdshdr16、sdshdr32 和 sdshdr64
     SDS 设计不同类型的结构体，是为了能灵活保存不同大小的字符串，从而有效节省内存空间
5. 兼容部分 C 字符串函数

#### 空间预分配
- 如果对 SDS 进行修改之后，SDS 的长度（也即是 `len` 属性的值）将小于 `1MB`，那么程序分配和 `len` 属性同样大小的未使用空间，这时 SDS `len` 属性的值将和 `alloc - len` 属性的值相同
- 如果对 SDS 进行修改之后，SDS 的长度将大于等于 `1MB`，那么程序会分配 1MB 的未使用空间, 即 SDS 的 `alloc - len` 为 `1MB`

#### 惰性空间释放
- 当 SDS 的 API 需要缩短 SDS 保存的字符串时，程序并不立即使用内存重分配来回收缩短后多出来的字节，而是使用 `alloc` 属性将这些字节的数量记录起来，并等待将来使用

#### API
1.  `sdsnew`: 创建一个新的 SDS 对象
    描述：该函数用于在堆上分配内存并创建一个空的 SDS 对象
1. `sdsempty`: 创建一个不包含任何内容的空 SDS
1. `sdsdup`: 创建一个给定的 SDS 的副本
1.  `sdsfree`: 释放 SDS 对象所占用的内存
    描述：用于释放一个不再需要的 SDS 对象，以避免内存泄漏
3.  `sdscpy`: 复制一个 SDS 对象
    描述：将一个 SDS 对象的内容复制到另一个 SDS 对象中
4.  `sdscat`: 将内容追加到 SDS 对象的末尾
    描述：用于将一个字符串或另一个 SDS 对象的内容追加到目标 SDS 对象的末尾
1. `sdscatstd`: 将一个 SDS 追加到 SDS 的末尾
1.  `sdscatprintf`: 格式化字符串并将其追加到 SDS 对象的末尾
    描述：类似于 C 语言中的 sprintf 函数，但它将格式化的字符串追加到 SDS 对象中
6.  `sdscmp`: 比较两个 SDS 对象的内容
    描述：用于比较两个 SDS 对象的内容是否相同，类似于 C 语言中的 `strcmp` 函数
1.  `sdslen`: 获取 SDS 对象中字符串的长度
    描述：返回 SDS 对象中保存的字符串的长度，不包括空终止符
1.  `sdsavail`: 获取 SDS 对象中未使用的空间
    描述：返回 SDS 对象中未使用的空间大小
1.  `sdstrim`: 从 SDS 对象的开头和结尾修剪指定的字符
    描述：用于删除 SDS 对象开头和结尾的指定字符，类似于 C 语言中的 strtrim 函数
1.  `sdssplitlen`: 将 SDS 对象按照指定的分隔符拆分成多个子 SDS 对象
    描述：该函数用于将一个 SDS 对象按照指定的分隔符拆分成多个子 SDS 对象，并返回一个子 SDS 对象的数组
1.  `sdsrange`: 获取 SDS 对象中指定范围的子 SDS 对象
    描述：用于获取 SDS 对象中指定起始位置和长度的子 SDS 对象
1.  `sdsmapchars`: 对 SDS 对象中的字符进行替换操作
    描述：该函数可以用于对 SDS 对象中的字符进行映射操作，类似于 C 语言中的 strmap 函数
1.  `sdsjoin`: 将多个 SDS 对象连接成一个新的 SDS 对象
    描述：用于将多个 SDS 对象连接成一个新的 SDS 对象，类似于 C 语言中的 strcat 函数
1.  `sdssplitargs`: 将一个 C 字符串解析为多个参数的 SDS 对象数组
    描述：用于将一个 C 字符串解析为多个参数的 SDS 对象数组，常用于解析 Redis 的命令输入
1.  `sdsupdatelen`: 更新 SDS 对象保存的字符串长度
    描述：该函数用于更新 SDS 对象的字符串长度，用于手动修改 SDS 对象的长度

---
