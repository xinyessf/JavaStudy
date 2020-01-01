### 什么是Redis

```
简单来说 redis 就是一个数据库，不过与传统数据库不同的是 redis 的数据是存在内存中的，所以读写速度非常快，因此 redis 被广泛应用于缓存方向。另外，redis 也经常用来做分布式锁。redis 提供了多种数据类型来支持不同的业务场景。除此之外，redis 支持事务 、持久化、LUA脚本、LRU驱动事件、多种集群方案。 
```

### 常用命令

[推荐网站](http://doc.redisfans.com/)

```
redis-cli 打开
exists key 是否存在key
del key    删除key
ttl key 查看过期时间
expire  key 60  设置过期时间单位秒
keys * 获取所有的key
select 0 选择第一个库
move myString 1 将当前的数据库key移动到某个数据库,目标库有，则不能移动
flush db      清除指定库
randomkey     随机key
type key      类型
```

### String

```
set name zhangsan 
get name
mset key1 value1 key2 value2 批量设置
mget key1 key2 批量获取
setnx key value  不存在就插入（not exists）
setex key 1000 value   过期时间（expire）1000秒
setrange key index value  从index开始替换value
setrange key index value  从index开始替换value
incr age        递增
incrby age 10   递增10
decr age        递减
decrby age 10   递减
incrbyfloat     增减浮点数
append          追加
strlen          长度
getbit/setbit/bitcount/bitop    位操作
```

### hash

```
hmset user name "zhangsan" age 18  设置值
hget user name  获取user 为key的值
hgetall user	获取所有数据
hkeys user  	获取所有的key
hvals user   	所有value
hlen user 		获取长度
hsetnx key valueKey value  当hash中不存在的时候赋值
```

### list

```
lpush mylist a b c  左插入
rpush mylist x y z  右插入
lrange mylist 0 -1  数据集合
lpop mylist  弹出元素
rpop mylist  弹出元素
llen mylist  长度
lrem mylist count value  删除
lindex mylist 2          指定索引的值
lset mylist 2 n          索引设值
ltrim mylist 0 4         删除key
linsert mylist before a  插入
linsert mylist after a   插入
rpoplpush list list2     转移列表的数据
```

### set

```
sadd myset redis 
smembers myset       数据集合
srem myset set1         删除
sismember myset set1 判断元素是否在集合中
scard key_name       个数
sdiff | sinter | sunion 操作：集合间运算：差集 | 交集 | 并集
srandmember          随机获取集合中的元素
spop                 从集合中弹出一个元素
```

### zset

```
zadd zset 1 one
zadd zset 2 two
zadd zset 3 three
zincrby zset 1 one              增长分数
zscore zset two                 获取分数
zrange zset 0 -1 withscores     范围值
zrangebyscore zset 10 25 withscores 指定范围的值
zrangebyscore zset 10 25 withscores limit 1 2 分页
Zrevrangebyscore zset 10 25 withscores  指定范围的值
zcard zset  元素数量
Zcount zset 获得指定分数范围内的元素个数
Zrem zset one two        删除一个或多个元素
Zremrangebyrank zset 0 1  按照排名范围删除元素
Zremrangebyscore zset 0 1 按照分数范围删除元素
Zrank zset 0 -1    分数最小的元素排名为0
Zrevrank zset 0 -1  分数最大的元素排名为0
Zinterstore
zunionstore rank:last_week 7 rank:20150323 rank:20150324 rank:20150325  weights 1 1 1 1 1 1 1
```

### 排序：

```
sort mylist  排序
sort mylist alpha desc limit 0 2 字母排序
sort list by it:* desc           by命令
sort list by it:* desc get it:*  get参数
sort list by it:* desc get it:* store sorc:result  sort命令之store参数：表示把sort查询的结果集保存起来
```

### 发布订阅

```
subscribe redisChat   --订阅
publish redisChat "redis is great"  发消息
publish redisChat "redis is learn" 发消息
PSUBSCRIBE pattern [pattern ...] 订阅一个或多个符合给定模式的频道。
PUBSUB subcommand [argument [argument ...]] 查看订阅与发布系统状态。
PUBLISH channel message 将信息发送到指定的频道。
PUNSUBSCRIBE [pattern [pattern ...]] 退订所有给定模式的频道。
SUBSCRIBE channel [channel ...] 订阅给定的一个或多个频道的信息。
UNSUBSCRIBE [channel [channel ...]] 指退订给定的频道。
```

### 为什么要用Redis

**高行能**

假如用户第一次访问数据库中的某些数据。这个过程会比较慢，因为是从硬盘上读取的。将该用户访问的数据存在缓存中，这样下一次再访问这些数据的时候就可以直接从缓存中获取了。操作缓存就是直接操作内存，所以速度相当快。如果数据库中的对应数据改变的之后，同步改变缓存中相应的数据即可！

**高并发**

直接操作缓存能够承受的请求是远远大于直接访问数据库的，所以我们可以考虑把数据库中的部分数据转移到缓存中去，这样用户的一部分请求会直接到缓存这里而不用经过数据库。

### 为什么要用 redis 而不用 map/guava 做缓存?

> 下面的内容来自 segmentfault 一位网友的提问，地址：https://segmentfault.com/q/1010000009106416

缓存分为本地缓存和分布式缓存。以 Java 为例，使用自带的 map 或者 guava 实现的是本地缓存，最主要的特点是轻量以及快速，生命周期随着 jvm 的销毁而结束，并且在多实例的情况下，每个实例都需要各自保存一份缓存，缓存不具有一致性。

使用 redis 或 memcached 之类的称为分布式缓存，在多实例的情况下，各实例共用一份缓存数据，缓存具有一致性。缺点是需要保持 redis 或  memcached服务的高可用，整个程序架构上较为复杂。

### 如何解决 Redis 的并发竞争 Key 问题

所谓 Redis 的并发竞争 Key 的问题也就是多个系统同时对一个 key 进行操作，但是最后执行的顺序和我们期望的顺序不同，这样也就导致了结果的不同！

推荐一种方案：分布式锁（zookeeper 和 redis 都可以实现分布式锁）。（如果不存在 Redis 的并发竞争 Key 问题，不要使用分布式锁，这样会影响性能）

基于zookeeper临时有序节点可以实现的分布式锁。大致思想为：每个客户端对某个方法加锁时，在zookeeper上的与该方法对应的指定节点的目录下，生成一个唯一的瞬时有序节点。 判断是否获取锁的方式很简单，只需要判断有序节点中序号最小的一个。 当释放锁的时候，只需将这个瞬时节点删除即可。同时，其可以避免服务宕机导致的锁无法释放，而产生的死锁问题。完成业务流程后，删除对应的子节点释放锁。

在实践中，当然是从以可靠性为主。所以首推Zookeeper。

### 如何保证缓存与数据库双写时的数据一致性?

你只要用缓存，就可能会涉及到缓存与数据库双存储双写，你只要是双写，就一定会有数据一致性的问题，那么你如何解决一致性问题？

一般来说，就是如果你的系统不是严格要求缓存+数据库必须一致性的话，缓存可以稍微的跟数据库偶尔有不一致的情况，最好不要做这个方案，读请求和写请求串行化，串到一个内存队列里去，这样就可以保证一定不会出现不一致的情况

串行化之后，就会导致系统的吞吐量会大幅度的降低，用比正常情况下多几倍的机器去支撑线上的一个请求。

### 和其他缓存比较

```
MemCache
```

### 如何使用

```

```

### 分布式缓存使用

```

```

### 分布式锁

[锁的的方式](https://www.cnblogs.com/itplay/p/10163720.html)

[分布式锁](https://blog.csdn.net/tuesdayma/article/details/82751790)

```
//分布式锁使用

```

#### Redission分布式学习

[Redis常用命令对应到Redisson对象操作](https://blog.csdn.net/learner198461/article/details/87192834)

[基于redission的分布式锁](https://blog.csdn.net/liuxiao723846/article/details/88131065)

### 文章推荐



