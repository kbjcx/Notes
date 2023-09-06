---
aliases: [String]
area: redis
project: 
date: 2023-08-01 14:56
tags: []
---
---
#### Content
> [!info] String 类型底层数据结构实现主要是 int 和 [[简单动态字符串|SDS]]

##### 内部编码
1. ***int***
    如果一个字符串对象保存的是整数值，并且这个整数值可以用 `long` 类型来表示，那么字符串对象会将整数值保存在字符串对象结构的 `ptr` 属性里面（将 `void*` 转换成 `long`），并将字符串对象的编码设置为 `int`
1. ***embstr***
    如果字符串对象保存的是一个字符串，并且这个字符申的长度小于等于 32 字节（redis 2.+版本），那么字符串对象将使用一个简单动态字符串（SDS）来保存这个字符串，并将对象的编码设置为 `embstr`， `embstr` 编码是专门用于保存==短字符串==的一种优化编码方式
1. ***raw***
    如果字符串对象保存的是一个字符串，并且这个字符串的长度大于 32 字节（redis 2.+版本），那么字符串对象将使用一个简单动态字符串（SDS）来保存这个字符串，并将对象的编码设置为 `raw`

> [!attention] embstr 编码和 raw 编码的边界在 redis 不同版本中是不一样的：
> - redis 2.+ 是 32 字节
> - redis 3.0-4.0 是 39 字节
> - redis 5.0 是 44 字节

> [!question] embstr 和 raw 编码之间有什么区别？
> embstr 会通过一次内存分配函数来分配一块连续的内存空间来保存 `redisObject` 和 `SDS`，而 raw 编码会通过调用两次内存分配函数来分别分配两块空间来保存 `redisObject` 和 `SDS`
> embstr 的优点：
> -  embstr 编码将创建字符串对象所需的内存分配次数从 raw 编码的两次降低为一次
> - 释放 embstr 编码的字符串对象同样只需要调用一次内存释放函数
> - 因为 embstr 编码的字符串对象的所有数据都保存在一块连续的内存里面可以更好的利用 CPU 缓存提升性能
> 
> embstr 的缺点：
> - 如果字符串的长度增加需要重新分配内存时，整个 `redisObject` 和 `SDS` 都需要重新分配空间，所以**embstr 编码的字符串对象实际上是只读的**，redis 没有为 embstr 编码的字符串对象编写任何相应的修改程序。当我们对 embstr 编码的字符串对象执行任何修改命令（例如 `append`）时，程序会先将对象的编码从 embstr 转换成 raw，然后再执行修改命令

#### 应用场景
##### 缓存对象
使用 String 来缓存对象有两种方式：
- 直接缓存整个对象的 JSON，命令例子： `SET user:1 '{"name":"xiaolin", "age":18}'`
- 采用将 key 进行分离为 user:ID: 属性，采用 MSET 存储，用 MGET 获取各属性值，命令例子： `MSET user:1:name xiaolin user:1:age 18 user:2:name xiaomei user:2:age 20`

##### 常规计数
因为 Redis 处理命令是单线程，所以执行命令的过程是原子的。因此 String 数据类型适合计数场景，比如计算访问次数、点赞、转发、库存数量等等

##### 分布式锁
SET 命令有个 NX 参数可以实现「key 不存在才插入」，可以用它来实现分布式锁：
- 如果 key 不存在，则显示插入成功，可以用来表示加锁成功
- 如果 key 存在，则会显示插入失败，可以用来表示加锁失败

```cpp
    SET lock_key unique_value NX PX 10000
```
- **lock_key** 就是 key 键；
- **unique_value** 是客户端生成的唯一的标识；
- **NX** 代表只在 lock_key 不存在时，才对 lock_key 进行设置操作；
- **PX 10000** 表示设置 lock_key 的过期时间为 10s，这是为了避免客户端发生异常而无法释放锁

解锁的过程就是将 lock_key 键删除，但不能乱删，要保证执行操作的客户端就是加锁的客户端。所以，解锁的时候，我们要先判断锁的 unique_value 是否为加锁客户端，是的话，才将 lock_key 键删除
> [!attention] 解锁分为两个操作，需要通过 lua 脚本来保证操作的原子性

```lua
// 释放锁时，先比较 unique_value 是否相等，避免锁的误释放
if redis.call("get",KEYS[1]) == ARGV[1] then
    // 如果此时key过期，那么其他线程就会获取到分布式锁，那么执行到下一步的时候就
    会将其他线程的锁删除，导致多个线程同时操作临界资源
    return redis.call("del",KEYS[1])
else
    return 0
end
```

##### 共享 Session 信息
通常我们在开发后台管理系统时，会使用 Session 来保存用户的会话 (登录)状态，这些 Session 信息会被保存在服务器端，但这只适用于单系统应用，如果是分布式系统此模式将不再适用
因此，我们需要借助 Redis 对这些 Session 信息进行统一的存储和管理，这样无论请求发送到那台服务器，服务器都会去同一个 Redis 获取相关的 Session 信息，这样就解决了分布式系统下 Session 存储的问题

#### 常用命令
```cpp
# 设置 key-value 类型的值
> SET name lin
OK
# 根据 key 获得对应的 value
> GET name
"lin"
# 判断某个 key 是否存在
> EXISTS name
(integer) 1
# 返回 key 所储存的字符串值的长度
> STRLEN name
(integer) 3
# 删除某个 key 对应的值
> DEL name
(integer) 1
``` 
**批量设置**
```cpp
# 批量设置 key-value 类型的值
> MSET key1 value1 key2 value2 
OK
# 批量获取多个 key 对应的 value
> MGET key1 key2 
1) "value1"
2) "value2"
```
**计数器**
```cpp
# 设置 key-value 类型的值
> SET number 0
OK
# 将 key 中储存的数字值增一
> INCR number
(integer) 1
# 将key中存储的数字值加 10
> INCRBY number 10
(integer) 11
# 将 key 中储存的数字值减一
> DECR number
(integer) 10
# 将key中存储的数字值减 10
> DECRBY number 10
(integer) 0
```
**过期**
```cpp
# 设置 key 在 60 秒后过期（该方法是针对已经存在的key设置过期时间）
> EXPIRE name  60 
(integer) 1
# 查看数据还有多久过期
> TTL name 
(integer) 51

#设置 key-value 类型的值，并设置该key的过期时间为 60 秒
> SET key  value EX 60
OK
> SETEX key  60 value
OK
```
**不存在就插入**
```cpp
# 不存在就插入（not exists）
>SETNX key value
(integer) 1
```


---
