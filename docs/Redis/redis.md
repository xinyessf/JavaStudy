### 什么是Redis

```
简单来说 redis 就是一个数据库，不过与传统数据库不同的是 redis 的数据是存在内存中的，所以读写速度非常快，因此 redis 被广泛应用于缓存方向。另外，redis 也经常用来做分布式锁。redis 提供了多种数据类型来支持不同的业务场景。除此之外，redis 支持事务 、持久化、LUA脚本、LRU驱动事件、多种集群方案。 
```

**和mysql差别**:

mysql数据库是存在磁盘中的，操作是对于磁盘操作，这样访问量和并发很大时，运行速率就取决于磁盘的容量，带宽的大小和读取的方式，也就是 `sql` 语句，次数和效率也会影响读取效率。当访问量和并发很大的时候，`mysql` 就撑不住了，据统计，mysql的连接池并发数max为 *500-1000，*这时就可以使用redis缓存来帮助数据库缓解压力

### redis优点

**特点:**

- 内存数据库，速度快，也支持数据的持久化，可以将内存中的数据保存在磁盘中，重启的时候可以再次加载进行使用。
- Redis不仅仅支持简单的key-value类型的数据，同时还提供list，set，zset，hash等数据结构的存储。
- Redis支持数据的备份，即master-slave模式的数据备份。
- 支持事务

**优势**

- 性能极高 – Redis能读的速度是110000次/s,写的速度是81000次/s 。
- 丰富的数据类型 – Redis支持二进制案例的 Strings, Lists, Hashes, Sets 及 Ordered Sets 数据类型操作。
- 原子 – Redis的所有操作都是原子性的，同时Redis还支持对几个操作合并后的原子性执行。（事务）
- 丰富的特性 – Redis还支持 publish/subscribe, 通知, key 过期等等特性。

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
FLUSHALL      删除所有key
```

####String

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

####hash

```
hmset user name "zhangsan" age 18  设置值
hget user name  获取user 为key的值
hgetall user	获取所有数据
hkeys user  	获取所有的key
hvals user   	所有value
hlen user 		获取长度
hsetnx key valueKey value  当hash中不存在的时候赋值
```

####list

```bash
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

####set

```bash
sadd myset redis 
smembers myset       数据集合
srem myset set1         删除
sismember myset set1 判断元素是否在集合中
scard key_name       个数
sdiff | sinter | sunion 操作：集合间运算：差集 | 交集 | 并集
srandmember          随机获取集合中的元素
spop                 从集合中弹出一个元素

#-------操作一个set-------
# 添加元素
sadd mySet 1

# 查看全部元素
smembers mySet

# 判断是否包含某个值
sismember mySet 3

# 删除某个/些元素
srem mySet 1
srem mySet 2 4

# 查看元素个数
scard mySet

# 随机删除一个元素
spop mySet

#-------操作多个set-------
# 将一个set的元素移动到另外一个set
smove yourSet mySet 2

# 求两set的交集
sinter yourSet mySet

# 求两set的并集
sunion yourSet mySet

# 求在yourSet中而不在mySet中的元素
sdiff yourSet mySet
```

####zset

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

####排序

```
sort mylist  排序
sort mylist alpha desc limit 0 2 字母排序
sort list by it:* desc           by命令
sort list by it:* desc get it:*  get参数
sort list by it:* desc get it:* store sorc:result  sort命令之store参数：表示把sort查询的结果集保存起来
```

####发布订阅

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

### 为什么要用Redis?

**高性能**

假如用户第一次访问数据库中的某些数据。这个过程会比较慢，因为是从硬盘上读取的。将该用户访问的数据存在缓存中，这样下一次再访问这些数据的时候就可以直接从缓存中获取了。操作缓存就是直接操作内存，所以速度相当快。如果数据库中的对应数据改变的之后，同步改变缓存中相应的数据即可！

**高并发**

直接操作缓存能够承受的请求是远远大于直接访问数据库的，所以我们可以考虑把数据库中的部分数据转移到缓存中去，这样用户的一部分请求会直接到缓存这里而不用经过数据库。

> 缓存是走内存的，内存天然就支撑高并发。

**为什么要用 redis 而不用 map/guava 做缓存?**

缓存分为本地缓存和分布式缓存。以 Java 为例，使用自带的 map 或者 guava 实现的是本地缓存，最主要的特点是轻量以及快速，生命周期随着 jvm 的销毁而结束，并且在多实例的情况下，每个实例都需要各自保存一份缓存，缓存不具有一致性。

使用 redis 或 memcached 之类的称为分布式缓存，在多实例的情况下，各实例共用一份缓存数据，缓存具有一致性。缺点是需要保持 redis 或  memcached服务的高可用，整个程序架构上较为复杂。

###Redis持久化

[Redis持久化入门](https://blog.csdn.net/qq_34190023/article/details/82715705)

[Redis实战总结-配置、持久化、复制](https://blog.csdn.net/guweiyu_thinker/article/details/78816071)

[持久化详解](https://www.cnblogs.com/kismetv/p/9137897.html)

#### 什么是持久化

```
持久化就是把内存的数据写到磁盘中去，防止服务宕机了内存数据丢失。
在于数据备份和故障恢复。持久化主要是做灾难恢复，数据恢复
部署一个redis作为cache缓存，或保存一些较为重要的数据
```

**会导致什么问题**

```
如redis不可用了，要做的事是让redis尽快变得可用，重启redis，尽快让它对外提供服务，

如果没做数据备份，这时redis启动了也不可用，因为数据都没了。

有可能大量的请求过来，缓存全部无法命中，在redis里找不到数据，这时缓存雪崩问题，所有请求，没有在redis命中，就会去mysql数据库这种数据源头中去找，一下子mysql承接高并发，然后就挂了。
```

#### RDB

```
rdb是Redis DataBase缩写
```

![img](https://img2018.cnblogs.com/blog/1481291/201809/1481291-20180925141429889-1694430603.png)

![](https://img-blog.csdn.net/20180915170612823?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MTkwMDIz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

#### AOF

```
Aof是Append-only file缩写
```

![img](https://img2018.cnblogs.com/blog/1481291/201809/1481291-20180925141527592-2105439510.png)

比较:

```
1、aof文件比rdb更新频率高，优先使用aof还原数据。
2、aof比rdb更安全也更大
3、rdb性能比aof好
4、如果两个都配了优先加载AOF
```

#### 如何选择

- 不要仅仅使用 RDB，因为那样会导致你丢失很多数据；
- 也不要仅仅使用 AOF，因为那样有两个问题：第一，你通过 AOF 做冷备，没有 RDB 做冷备来的恢复速度更快；第二，RDB 每次简单粗暴生成数据快照，更加健壮，可以避免 AOF 这种复杂的备份和恢复机制的 bug；
- redis 支持同时开启开启两种持久化方式，我们可以综合使用 AOF 和 RDB 两种持久化机制，用 AOF 来保证数据不丢失，作为数据恢复的第一选择; 用 RDB 来做不同程度的冷备，在 AOF 文件都丢失或损坏不可用的时候，还可以使用 RDB 来进行快速的数据恢复。

### Redis和MemCache区别

**优点**

```
1.redis 支持复杂的数据结构
2.可以集群
3.性能,redis使用单核,读写速度快
```

**缺点**

```
而在10以上的数据,memcached 性能要高于 redis。虽然 redis 最近也在存储大数据的性能上进行优化，但是比起 memcached，还是稍有逊色。
```

**为啥redis单线程模型效率这么高?**

- 纯内存操作。
- 核心是基于非阻塞的 IO 多路复用机制。
- C 语言实现，一般来说，C 语言实现的程序“距离”操作系统更近，执行速度相对会更快。
- 单线程反而避免了多线程的频繁上下文切换问题，预防了多线程可能产生的竞争问题。

流程如下

[redis通信过程](https://www.jianshu.com/p/d9d1a15779af)

![img](https://github.com/doocs/advanced-java/raw/master/images/redis-single-thread-model.png)

###  Redis 的并发竞争 Key 

所谓 Redis 的并发竞争 Key 的问题也就是多个系统同时对一个 key 进行操作，但是最后执行的顺序和我们期望的顺序不同，这样也就导致了结果的不同！

推荐一种方案：分布式锁（zookeeper 和 redis 都可以实现分布式锁）。（如果不存在 Redis 的并发竞争 Key 问题，不要使用分布式锁，这样会影响性能）

基于zookeeper临时有序节点可以实现的分布式锁。大致思想为：每个客户端对某个方法加锁时，在zookeeper上的与该方法对应的指定节点的目录下，生成一个唯一的瞬时有序节点。 判断是否获取锁的方式很简单，只需要判断有序节点中序号最小的一个。 当释放锁的时候，只需将这个瞬时节点删除即可。同时，其可以避免服务宕机导致的锁无法释放，而产生的死锁问题。完成业务流程后，删除对应的子节点释放锁。

在实践中，当然是从以可靠性为主。所以首推Zookeeper。

### 缓存常见的问题

[了解缓存雪崩,穿透,双写一致性](https://www.cnblogs.com/kyoner/p/11297488.html)

[redis并发问题](并发问题分析)

**缓存雪崩**

```
//什么是雪崩?
如果我们的缓存挂掉了，这意味着我们的全部请求都跑去数据库了。
//如何解决
事发前：实现Redis的高可用(主从架构+Sentinel 或者Redis Cluster)，尽量避免Redis挂掉这种情况发生。
事发中：万一Redis真的挂了，我们可以设置本地缓存(ehcache)+限流(hystrix)，尽量避免我们的数据库被干掉(起码能保证我们的服务还是能正常工作的)
事发后：redis持久化，重启后自动从磁盘上加载数据，快速恢复缓存数据。
```

**缓存穿透**

```
//什么是缓存穿透
缓存穿透是指查询一个一定不存在的数据。由于缓存不命中，并且出于容错考虑，如果从数据库查不到数据则不写入缓存，这将导致这个不存在的数据每次请求都要到数据库去查询，失去了缓存的意义。
//如何解决?
1.由于请求的参数是不合法的(每次都请求不存在的参数)，于是我们可以使用布隆过滤器(BloomFilter)或者压缩filter提前拦截，不合法就不让这个请求到数据库层！
2.当我们从数据库找不到的时候，我们也将这个空对象设置到缓存里边去。下次再请求的时候，就可以从缓存里边获取了。
```

**缓存与数据库双写数据一致性?**

[缓存不一致](http://dy.163.com/v2/article/detail/EA85R850054479O4.html)

**1.最初级的缓存不一致问题及解决方案**

　　**问题**：先修改数据库，再删除缓存。如果删除缓存失败了，那么会导致数据库中是新数据，缓存中是旧数据，数据就出现了不一致。解决思路:先删除缓存，再修改数据库。

**2.比较复杂的数据不一致问题分析**

​		**问题:**数据发生了变更，先删除了缓存，然后要去修改数据库，此时还没修改。一个请求过来，去读缓存，发现缓存空了，去查询数据库，查到了修改前的旧数据，放到了缓存中。随后数据变更的程序完成了数据库的修改。完了，数据库和缓存中的数据不一样了;

>1更新数据的时候，根据数据的唯一标识，将操作路由之后，发送到一个 jvm 内部队列中。读取数据的时候，如果发现数据不在缓存中，那么将重新读取数据+更新缓存的操作，根据唯一标识路由之后，也发送同一个 jvm 内部队列中。
>
>2一个队列对应一个工作线程，每个工作线程串行拿到对应的操作，然后一条一条的执行。这样的话，一个数据变更的操作，先删除缓存，然后再去更新数据库，但是还没完成更新。此时如果一个读请求过来，读到了空的缓存，那么可以先将缓存更新的请求发送到队列中，此时会在队列中积压，然后同步等待缓存更新完成。
>
>3这里有一个优化点，一个队列中，其实多个更新缓存请求串在一起是没意义的，因此可以做过滤，如果发现队列中已经有一个更新缓存的请求了，那么就不用再放个更新请求操作进去了，直接等待前面的更新操作请求完成即可。
>
>4 待那个队列对应的工作线程完成了上一个操作的数据库的修改之后，才会去执行下一个操作，也就是缓存更新的操作，此时会从数据库中读取最新的值，然后写入缓存中。
>
>5 如果请求还在等待时间范围内，不断轮询发现可以取到值了，那么就直接返回；如果请求等待的时间超过一定时长，那么这一次直接从数据库中读取当前的旧值。

### redis缓存策略

####缓存策略起因

```
1.往 redis 写入的数据怎么没了？
2.数据明明过期了，怎么还占用着内存？
答:1.啥叫缓存？用内存当缓存。内存是无限的吗，内存是很宝贵而且是有限的，磁盘是廉价而且是大量的。可能一台机器就几十个 G 的内存，但是可以有几个 T 的硬盘空间。redis 主要是基于内存来进行高性能、高并发的读写操作的。
redis 就只能用 10G，你要是往里面写了 20G 的数据，会咋办？当然会干掉 10G 的数据，
答:2.这是由 redis 的过期策略来决定。
```

#### 手写一个LRU算法

```
思路:
class LRUCache<K, V> extends LinkedHashMap<K, V> 
1.初始化一个LinkHashMap
2.获取当前长度,当长度满足指定的数据,就自动删除
```

####redis 过期策略

redis 过期策略是：**定期删除+惰性删除**。

假设 redis 里放了 10w 个 key，都设置了过期时间，你每隔几百毫秒，就检查 10w 个 key，那 redis 基本上就死了，cpu 负载会很高的，消耗在你的检查过期 key 上了。注意，这里可不是每隔 100ms 就遍历所有的设置过期时间的 key，那样就是一场性能上的**灾难**。实际上 redis 是每隔 100ms **随机抽取**一些 key 来检查和删除的。

但是问题是，定期删除可能会导致很多过期 key 到了时间并没有被删除掉，那咋整呢？所以就是惰性删除了。这就是说，在你获取某个 key 的时候，redis 会检查一下 ，这个 key 如果设置了过期时间那么是否过期了？如果过期了此时就会删除，不会给你返回任何东西。

> 获取 key 的时候，如果此时 key 已经过期，就删除，不会返回任何东西。

但是实际上这还是有问题的，如果定期删除漏掉了很多过期 key，然后你也没及时去查，也就没走惰性删除，此时会怎么样？如果大量过期 key 堆积在内存里，导致 redis 内存块耗尽了，咋整？

答案是：**走内存淘汰机制**。

####内存淘汰机制

redis 内存淘汰机制有以下几个：

- noeviction: 当内存不足以容纳新写入数据时，新写入操作会报错，这个一般没人用吧，实在是太恶心了。
- **allkeys-lru**：当内存不足以容纳新写入数据时，在**键空间**中，移除最近最少使用的 key（这个是**最常用**的）。
- allkeys-random：当内存不足以容纳新写入数据时，在**键空间**中，随机移除某个 key，这个一般没人用吧，为啥要随机，肯定是把最近最少使用的 key 给干掉啊。
- volatile-lru：当内存不足以容纳新写入数据时，在**设置了过期时间的键空间**中，移除最近最少使用的 key（这个一般不太合适）。
- volatile-random：当内存不足以容纳新写入数据时，在**设置了过期时间的键空间**中，**随机移除**某个 key。
- volatile-ttl：当内存不足以容纳新写入数据时，在**设置了过期时间的键空间**中，有**更早过期时间**的 key 优先移除。

### 如何保证Redis高并发高可用

如果你用 redis 缓存技术的话，肯定要考虑如何用 redis 来加多台机器，保证 redis 是高并发的，还有就是如何让 redis 保证自己不是挂掉以后就直接死掉了，即 redis 高可用。

由于此节内容较多，因此，会分为两个小节进行讲解。

- [redis 主从架构](redis 主从架构.md)
- [redis 基于哨兵实现高可用](redis 基于哨兵实现高可用.md)

redis 实现**高并发**主要依靠**主从架构**，一主多从，一般来说，很多项目其实就足够了，单主用来写入数据，单机几万 QPS，多从用来查询数据，多个从实例可以提供每秒 10w 的 QPS。

如果想要在实现高并发的同时，容纳大量的数据，那么就需要 redis 集群，使用 redis 集群之后，可以提供每秒几十万的读写并发。

redis 高可用，如果是做主从架构部署，那么加上哨兵就可以了，就可以实现，任何一个实例宕机，可以进行主备切换。

##分布式缓存使用

### 分布式锁

[锁的的方式](https://www.cnblogs.com/itplay/p/10163720.html)

[分布式锁](https://blog.csdn.net/tuesdayma/article/details/82751790)

```
分布式锁使用:

```

#### Redission分布式学习

[Redis常用命令对应到Redisson对象操作](https://blog.csdn.net/learner198461/article/details/87192834)

[基于redission的分布式锁](https://blog.csdn.net/liuxiao723846/article/details/88131065)

### redis分布式和本地缓存结合使用

是什么?

>本地缓存+redis结合使用，减少网络带宽的使用，还可以提升应用服务的性能,但不是什么数据都要放本地缓存里，根据自己服务器的内存和缓存业务数据综合考虑。

### 缓存使用

###面试

- [在项目中缓存是如何使用的？缓存如果使用不当会造成什么后果？]

- [Redis 和 Memcached 有什么区别？Redis 的线程模型是什么？为什么单线程的 Redis 比多线程的 Memcached 效率要高得多？]

  [Redis 都有哪些数据类型？分别在哪些场景下使用比较合适？]

- [Redis 的过期策略都有哪些？手写一下 LRU 代码实现？]

- [如何保证 Redis 高并发、高可用？Redis 的主从复制原理能介绍一下么？Redis 的哨兵原理能介绍一下么？]

- [Redis 的持久化有哪几种方式？不同的持久化机制都有什么优缺点？持久化机制具体底层是如何实现的？]

- [Redis 集群模式的工作原理能说一下么？在集群模式下，Redis 的 key 是如何寻址的？分布式寻址都有哪些算法？了解一致性 hash 算法吗？如何动态增加和删除一个节点？]

- [了解什么是 redis 的雪崩、穿透和击穿？Redis 崩溃之后会怎么样？系统该如何应对这种情况？如何处理 Redis 的穿透？]

- [如何保证缓存与数据库的双写一致性？]

- [Redis 的并发竞争问题是什么？如何解决这个问题？了解 Redis 事务的 CAS 方案吗？]

- [生产环境中的 Redis 是怎么部署的？]





