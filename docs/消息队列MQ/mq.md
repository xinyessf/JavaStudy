###MQ

####什么是消息队列

####为什么使用消息队列

[为什么使用消息队列](https://www.cnblogs.com/terry-love/p/11492397.html)

1.解耦

```
A向 B,C,D发信息,现在又加个E
```

![img](https://img2018.cnblogs.com/blog/1756639/201909/1756639-20190908223436765-607649977.jpg)

![img](https://img2018.cnblogs.com/blog/1756639/201909/1756639-20190908223437000-220196142.jpg)

2.异步

```
A写数据
B 耗时100ms
c 耗时900ms
D 耗时1000ms 
此时就可以将B,C,D放入到MQ 
```

![img](https://img2018.cnblogs.com/blog/1756639/201909/1756639-20190908223437240-841013337.jpg)



![img](https://img2018.cnblogs.com/blog/1756639/201909/1756639-20190908223437468-1330530320.jpg)

3.削峰

```
比如:每天中午13点-14点请求过大,数据吃不消
扛到每秒 2k 个请求就差不多了，如果每秒请求到 5k 的话，
```

![img](https://img2018.cnblogs.com/blog/1756639/201909/1756639-20190908223437702-1402821528.jpg)

![img](https://img2018.cnblogs.com/blog/1756639/201909/1756639-20190908223437928-344432831.jpg)



缺点:

```
1.系统可用性降低,
2.系统复杂度提高
3.一致性问题 A系统,处理完了返回成功, BCD三个系统哪里,BD系统成功,C失败
```

####Kafka,ActiveMQ,RabbitMQ,RocketMQ 区别,以及适合那些场景

[为什么使用消息队列](https://www.cnblogs.com/terry-love/p/11492397.html)

#### 消息丢失怎么办

[消息丢失](https://blog.csdn.net/Iperishing/article/details/86674488)

[消息丢失解决办法](https://blog.csdn.net/u014513171/article/details/92797793)

#### 如何保证消息队列的高可用

[RabbitMQ](https://www.cnblogs.com/mengchunchen/p/10001971.html)

```
单机模式，普通集群模式，镜像集群模式
```

#### 如何保证消息不被重复消费

[如何保证消息不被重复消费](https://www.cnblogs.com/mengchunchen/p/10007537.html)

```
1.比如kafka,会被消费的消息offset一下;offset失败
--
解决:
1.如果是redis,set下,天然幂等性;
2.如果是mysql,主键或者其他信息查询一下啊;
```

#### 如何保证消息的顺序性

[消费的一致性](https://www.cnblogs.com/mengchunchen/p/10020997.html)

#### 消息队列过期失效,爆满,有几百万消挤压，说说怎么解决？

[解决方法](https://blog.csdn.net/Iperishing/article/details/86676682)

[几百万数据挤压](https://blog.csdn.net/A_BlackMoon/article/details/85197943)

```
1.爆满,消费者服务器排查,mq加服务器,
2.mq失效,丢失;补数据;
3.mq快写满了,写程序消费;
```

#### 写一个消息队列，该如何进行架构设计啊？

[设计消息队列](https://blog.csdn.net/zhaoziyun21/article/details/88428821)

```
分布式
集群,主从
```





