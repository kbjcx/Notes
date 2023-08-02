---
aliases: [双向链表, List]
area: redis
project: 
date: 2023-08-01 15:15
tags: [TODO]
---
---
#### Content
**链表节点设计**
```cpp
typedef struct listNode {
    //前置节点
    struct listNode *prev;
    //后置节点
    struct listNode *next;
    //节点的值
    void *value;
} listNode;
```
Redis 在 listNode 结构体基础上又封装了 list 这个数据结构，这样操作起来会更方便，链表结构如下
```cpp
typedef struct list {
    //链表头节点
    listNode *head;
    //链表尾节点
    listNode *tail;
    //节点值复制函数
    void *(*dup)(void *ptr);
    //节点值释放函数
    void (*free)(void *ptr);
    //节点值比较函数
    int (*match)(void *ptr, void *key);
    //链表节点数量
    unsigned long len;
} list;
```
list 结构为链表提供了链表头指针 `head`、链表尾节点 `tail`、链表节点数量 `len`、以及可以自定义实现的 `dup、free、match` 函数

#### 优点
1. listNode 链表节点的结构里带有 `prev` 和 `next` 指针，获取某个节点的前置节点或后置节点的时间复杂度只需 O (1)，而且这两个指针都可以指向 NULL，所以链表是无环链表
2. list 结构因为提供了表头指针 `head` 和表尾节点 `tail`，所以获取链表的表头节点和表尾节点的时间复杂度只需 O (1)
3. list 结构因为提供了链表节点数量 `len`，所以获取链表中的节点数量的时间复杂度只需 O (1)
4. listNode 链表节使用 ` void*` 指针保存节点值，并且可以通过 list 结构的 `dup、free、match` 函数指针为节点设置该节点类型特定的函数，因此链表节点可以保存各种不同类型的值

#### 缺点


---
