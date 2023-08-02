---
aliases: [Zset]
area: redis
project: 
date: 2023-08-02 12:06
tags: []
---
---
#### Content
Zset 类型（有序集合类型）相比于 Set 类型多了一个排序属性 score（分值），对于有序集合 ZSet 来说，每个存储元素相当于有两个值组成的，一个是有序集合的元素值，一个是排序值
Zset 类型的底层数据结构是由[[Redis压缩列表|压缩列表]]或[[Redis.跳表|跳表]]实现的: 
- 如果有序集合的元素个数小于 128 个，并且每个元素的值小于 64 字节时，Redis 会使用[[Redis压缩列表|压缩列表]]作为 Zset 类型的底层数据结构
- 如果有序集合的元素不满足上面的条件，Redis 会使用跳表作为 Zset 类型的底层数据结构

> [!attention] 在 Redis 7.0 中，压缩列表数据结构已经废弃了，交由 listpack 数据结构来实现

#### 应用场景
Zset 类型（Sorted Set，有序集合） 可以根据元素的权重来排序，我们可以自己来决定每个元素的权重值。比如说，我们可以根据元素插入 Sorted Set 的时间确定权重值，先插入的元素权重小，后插入的元素权重大。

在面对需要展示最新列表、排行榜等场景时，如果数据更新频繁或者需要分页显示，可以优先考虑使用 Sorted Set
##### 排行榜
有序集合比较典型的使用场景就是排行榜。例如学生成绩的排名榜、游戏积分排行榜、视频播放排名、电商系统中商品的销量排名等。

我们以博文点赞排名为例，小林发表了五篇博文，分别获得赞为 200、40、100、50、150
```py
# arcticle:1 文章获得了200个赞
> ZADD user:xiaolin:ranking 200 arcticle:1
(integer) 1
# arcticle:2 文章获得了40个赞
> ZADD user:xiaolin:ranking 40 arcticle:2
(integer) 1
# arcticle:3 文章获得了100个赞
> ZADD user:xiaolin:ranking 100 arcticle:3
(integer) 1
# arcticle:4 文章获得了50个赞
> ZADD user:xiaolin:ranking 50 arcticle:4
(integer) 1
# arcticle:5 文章获得了150个赞
> ZADD user:xiaolin:ranking 150 arcticle:5
(integer) 1
```
文章 `arcticle: 4` 新增一个赞，可以使用 **ZINCRBY** 命令（为有序集合 **key** 中元素 **member** 的分值加上 **increment**）
```py
> ZINCRBY user:xiaolin:ranking 1 arcticle:4
"51"
```
查看某篇文章的赞数，可以使用 **ZSCORE** 命令（返回有序集合 **key** 中元素个数）
```py
> ZSCORE user:xiaolin:ranking arcticle:4
"50"
```
获取小林文章赞数最多的 3 篇文章，可以使用 **ZREVRANGE** 命令（倒序获取有序集合 **key** 从 start 下标到 stop 下标的元素）
```py
# WITHSCORES 表示把 score 也显示出来
> ZREVRANGE user:xiaolin:ranking 0 2 WITHSCORES
1) "arcticle:1"
2) "200"
3) "arcticle:5"
4) "150"
5) "arcticle:3"
6) "100"
```
获取小林 100 赞到 200 赞的文章，可以使用 **ZRANGEBYSCORE** 命令（返回有序集合中指定分数区间内的成员，分数由低到高排序）
```py
> ZRANGEBYSCORE user:xiaolin:ranking 100 200 WITHSCORES
1) "arcticle:3"
2) "100"
3) "arcticle:5"
4) "150"
5) "arcticle:1"
6) "200"
```

##### 电话、姓名排序
使用有序集合的 **ZRANGEBYLEX** 或 **ZREVRANGEBYLEX** 可以帮助我们实现电话号码或姓名的排序，我们以 **ZRANGEBYLEX** （返回指定成员区间内的成员，按 **key** 正序排列，分数必须相同）为例

> [!attention] 不要在分数不一致的 SortSet 集合中去使用 `ZRANGEBYLEX` 和 `ZREVRANGEBYLEX` 指令，因为获取的结果会不准确

***电话排序***
```py
> ZADD phone 0 13100111100 0 13110114300 0 13132110901 
(integer) 3
> ZADD phone 0 13200111100 0 13210414300 0 13252110901 
(integer) 3
> ZADD phone 0 13300111100 0 13310414300 0 13352110901 
(integer) 3
```
获取所有号码
```py
> ZRANGEBYLEX phone - +
```
获取 132 号段的号码
```py
> ZRANGEBYLEX phone [132 (133
1) "13200111100"
2) "13210414300"
3) "13252110901"
```
***姓名排序***
```py
> zadd names 0 Toumas 0 Jake 0 Bluetuo 0 Gaodeng 0 Aimini 0 Aidehua 
(integer) 6
```
获取所有人的名字
```py
> ZRANGEBYLEX names - +
```
获取名字中大写字母 A 开头的所有人
```py
> ZRANGEBYLEX names [A (B
1) "Aidehua"
2) "Aimini"
```
获取名字中大写字母 C 到 Z 的所有人
```py
> ZRANGEBYLEX names [C [Z
1) "Gaodeng"
2) "Jake"
3) "Toumas"
```

#### 常用命令
```py
# 往有序集合key中加入带分值元素
ZADD key score member [[score member]...]   
# 往有序集合key中删除元素
ZREM key member [member...]                 
# 返回有序集合key中元素member的分值
ZSCORE key member
# 返回有序集合key中元素个数
ZCARD key 

# 为有序集合key中元素member的分值加上increment
ZINCRBY key increment member 

# 正序获取有序集合key从start下标到stop下标的元素
ZRANGE key start stop [WITHSCORES]
# 倒序获取有序集合key从start下标到stop下标的元素
ZREVRANGE key start stop [WITHSCORES]

# 返回有序集合中指定分数区间内的成员，分数由低到高排序。
ZRANGEBYSCORE key min max [WITHSCORES] [LIMIT offset count]

# 返回指定成员区间内的成员，按字典正序排列, 分数必须相同。
ZRANGEBYLEX key min max [LIMIT offset count]
# 返回指定成员区间内的成员，按字典倒序排列, 分数必须相同
ZREVRANGEBYLEX key max min [LIMIT offset count]
```
Zset 运算操作（相比于 Set 类型，ZSet 类型没有支持差集运算）
```py
# 并集计算(相同元素分值相加)，numberkeys一共多少个key，WEIGHTS每个key对应的分值乘积
ZUNIONSTORE destkey numberkeys key [key...] 
# 交集计算(相同元素分值相加)，numberkeys一共多少个key，WEIGHTS每个key对应的分值乘积
ZINTERSTORE destkey numberkeys key [key...]
```

---
