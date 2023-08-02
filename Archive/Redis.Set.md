---
aliases: [Set]
area: redis
project: 
date: 2023-08-01 22:08
tags: []
---
---
#### Content
Set 类型是一个无序并唯一的键值集合，它的存储顺序不会按照插入的先后顺序进行存储
Set 类型除了支持集合内的增删改查，同时还支持多个集合取**交集、并集、差集**
Set 类型的底层数据结构是由[[Redis.哈希表|哈希表]] 或[[整数集合]]实现的:
- 如果集合中的元素都是整数且元素个数小于 512 （默认值，`set-maxintset-entries` 配置）个，Redis 会使用整数集合作为 Set 类型的底层数据结构
- 如果集合中的元素不满足上面条件，则 Redis 使用哈希表作为 Set 类型的底层数据结构

> [!question] Set 类型和 List 类型的区别？
> - List 可以存储重复元素，Set 只能存储非重复元素
> - List 是按照元素的先后顺序存储元素的，而 Set 则是无序方式存储元素的

#### 应用场景
> [!tip] Set 类型比较适合用来**数据去重**和**保障数据的唯一性**，还可以用来统计多个集合的交集、错集和并集等，当我们存储的数据是无序并且需要去重的情况下，比较适合使用集合类型进行存储

> [!attention] Set 的差集、并集和交集的计算复杂度较高，在数据量较大的情况下，如果直接执行这些计算，会导致 Redis 实例阻塞

```cpp
#include <cstring>
int main() {
    return 0;
}
```

##### 点赞
Set 类型可以保证一个用户只能点一个赞，这里举例子一个场景，key 是文章 id，value 是用户 id
`uid: 1 、uid: 2、uid: 3` 三个用户分别对 `article: 1` 文章点赞
```sh
> SADD article:1 uid:1 uid:2 uid:3
(integer) 3
```
`uid: 1` 取消了对 ` article: 1` 文章点赞
```sh
> SREM article:1 uid:1
(integer) 1
```
获取 article: 1 文章所有点赞用户
```sh
> SMEMBERS article:1
1) "uid:3"
2) "uid:2"
```
获取 article: 1 文章的点赞用户数量
```sh
> SCARD article:1
(integer) 2
```
判断用户 uid: 1 是否对文章 article: 1 点赞
```sh
> SISMEMBER article:1 uid:1
(integer) 0  # 返回0说明没点赞，返回1则说明点赞了
```

##### 共同关注
Set 类型支持交集运算，所以可以用来计算共同关注的好友、公众号等
`uid: 1` 用户关注公众号 id 为 5、6、7、8、9，`uid: 2` 用户关注公众号 id 为 7、8、9、10、11
```sh
# uid:1 用户关注公众号 id 为 5、6、7、8、9
> SADD uid:1 5 6 7 8 9
(integer) 5
# uid:2  用户关注公众号 id 为 7、8、9、10、11
> SADD uid:2 7 8 9 10 11
(integer) 5
```
`uid: 1` 和 `uid: 2` 共同关注的公众号
```sh
# 获取共同关注
> SINTER uid:1 uid:2
1) "7"
2) "8"
3) "9"
```
给 `uid: 2` 推荐 `uid: 1` 关注的公众号
```sh
> SDIFF uid:1 uid:2
1) "5"
2) "6"
```
验证某个公众号是否同时被 `uid: 1` 或 `uid: 2` 关注
```sh
> SISMEMBER uid:1 5
(integer) 1 # 返回0，说明关注了
> SISMEMBER uid:2 5
(integer) 0 # 返回0，说明没关注
```

##### 抽奖活动
存储某活动中中奖的用户名，Set 类型因为有去重功能，可以保证同一个用户不会中奖两次
**key** 为抽奖活动名，**value** 为员工名称，把所有员工名称放入抽奖箱 
```sh
> SADD lucky Tom Jerry John Sean Marry Lindy Sary Mark
(integer) 8
```
如果允许重复中奖，可以使用 **SRANDMEMBER** 命令
```sh
# 抽取 1 个一等奖：
> SRANDMEMBER lucky 1
1) "Tom"
# 抽取 2 个二等奖：
> SRANDMEMBER lucky 2
1) "Mark"
2) "Jerry"
# 抽取 3 个三等奖：
> SRANDMEMBER lucky 3
1) "Sary"
2) "Tom"
3) "Jerry"
```
如果不允许重复中奖，可以使用 **SPOP** 命令
```sh
# 抽取一等奖1个
> SPOP lucky 1
1) "Sary"
# 抽取二等奖2个
> SPOP lucky 2
1) "Jerry"
2) "Mark"
# 抽取三等奖3个
> SPOP lucky 3
1) "John"
2) "Sean"
3) "Lindy"
```

#### 常用命令
```sh
# 往集合key中存入元素，元素存在则忽略，若key不存在则新建
SADD key member [member ...]
# 从集合key中删除元素
SREM key member [member ...] 
# 获取集合key中所有元素
SMEMBERS key
# 获取集合key中的元素个数
SCARD key

# 判断member元素是否存在于集合key中
SISMEMBER key member

# 从集合key中随机选出count个元素，元素不从key中删除
SRANDMEMBER key [count]
# 从集合key中随机选出count个元素，元素从key中删除
SPOP key [count]
```
**Set 运算操作**
```sh
# 交集运算
SINTER key [key ...]
# 将交集结果存入新集合destination中
SINTERSTORE destination key [key ...]

# 并集运算
SUNION key [key ...]
# 将并集结果存入新集合destination中
SUNIONSTORE destination key [key ...]

# 差集运算
SDIFF key [key ...]
# 将差集结果存入新集合destination中
SDIFFSTORE destination key [key ...]
```


---
