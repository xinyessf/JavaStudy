什么是kafka

[kafka原理总结](https://www.jianshu.com/p/734cf729d77b)

>一种分布式的、基于发布/订阅的消息系统（Scala编写的)
>
>Kafka是最初由Linkedin公司开发，是一个分布式、支持分区的（partition）、多副本的（replica），基于zookeeper协调的分布式消息系统，它的最大的特性就是可以实时的处理大量数据以满足各种需求场景：比如基于hadoop的批处理系统、低延迟的实时系统、storm/Spark流式处理引擎，web/nginx日志、访问日志，消息服务等等，用scala语言编写，Linkedin于2010年贡献给了Apache基金会并成为顶级开源 项目。

*特性*

>高吞吐量、低延迟：kafka每秒可以处理几十万条消息，它的延迟最低只有几毫秒，每个topic可以分多个partition, consumer group 对partition进行consume操作。
>
>可扩展性：kafka集群支持热扩展
>
>持久性、可靠性：消息被持久化到本地磁盘，并且支持数据备份防止数据丢失
>
>容错性：允许集群中节点失败（若副本数量为n,则允许n-1个节点失败）
>
>高并发：支持数千个客户端同时读写

**使用场景**

>日志收集：一个公司可以用Kafka可以收集各种服务的log，通过kafka以统一接口服务的方式开放给各种consumer，例如hadoop、Hbase、Solr等。
>
>消息系统：解耦和生产者和消费者、缓存消息等。
>
>用户活动跟踪：Kafka经常被用来记录web用户或者app用户的各种活动，如浏览网页、搜索、点击等活动，这些活动信息被各个服务器发布到kafka的topic中，然后订阅者通过订阅这些topic来做实时的监控分析，或者装载到hadoop、数据仓库中做离线分析和挖掘。
>
>运营指标：Kafka也经常用来记录运营监控数据。包括收集各种分布式应用的数据，生产各种操作的集中反馈，比如报警和报告。
>
>流式处理：比如spark streaming和storm
>
>事件源

### kafka设计思想

![img](https://img-blog.csdn.net/20140709200548420?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3VpZmVuZzMwNTE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### 集群模式

>
>

```

```

### kafka集群安装说明

```shell
kafka集群安装
	1.下载Kafka安装包
	2.上传安装包
	3.解压
	4.修改配置文件 config/server.properties
		broker.id=0
		host.name=node-4
		log.dirs=/data/kafka
		zookeeper.connect=node-1:2181,node-2:2181,node-3:2181
	5.将配置好的kafka拷贝到其他机器上
	6.修改broker.id和host.name
	7.启动kafka
		/bigdata/kafka_2.11-0.8.2.2/bin/kafka-server-start.sh -daemon /bigdata/kafka_2.11-0.8.2.2/config/server.properties 


	#查看topic信息
	/bigdata/kafka_2.11-0.8.2.2/bin/kafka-topics.sh --list --zookeeper node-1:2181,node-2:2181

	#创建topic
	/bigdata/kafka_2.11-0.8.2.2/bin/kafka-topics.sh --create --zookeeper node-1:2181,node-2:2181 --replication-factor 3 --partitions 3 --topic xiaoniu

	#往Kafka的topic中写入数据(命令行的生成者)
	/bigdata/kafka_2.11-0.8.2.2/bin/kafka-console-producer.sh --broker-list node-4:9092,node-5:9092,node-5:9092 --topic xiaoniu

	#启动消费者
	/bigdata/kafka_2.11-0.8.2.2/bin/kafka-console-consumer.sh --zookeeper node-1:2181,node-2:2181 --topic xiaoniu --from-beginning

```

####安装

```shell
## 需要安装zookeeper 集群请看说明

/usr/local/zookeeper/bin/zkServer.sh start
/usr/local/zookeeper/bin/zkServer.sh restart
/usr/local/zookeeper/bin/zkServer.sh stop
/usr/local/zookeeper/bin/zkServer.sh status
###
cd /usr/local/java/
rz kafka_2.11-0.8.2.2.tgz
tar -zxvf kafka_2.11-0.8.2.2.tgz
mv kafka_2.11-0.8.2.2.tgz kafka

## 修改配置
cd conf
vi server.properties
####
broker.id=0
host.name=192.168.73.128
log.dirs=/data/kafka
zookeeper.connect=192.168.73.128:2181
#### 2,3台
scp -r /usr/local/java/kafka hdp-02:/usr/local/java/kafka
scp -r /usr/local/java/kafka hdp-03:/usr/local/java/kafka
## 修改broker.id  1,2 和host.name为机器
## 分别启动,下边的是一个命令
/usr/local/java/kafka/bin/kafka-server-start.sh -daemon /usr/local/java/kafka/config/server.properties 


```

#### 后台常用命令

```shell
#查看topic信息
/usr/local/java/kafka/bin/kafka-topics.sh --list --zookeeper 192.168.73.128:2181

#创建topic
/usr/local/java/kafka/bin/kafka-topics.sh --create --zookeeper 192.168.73.128:2181 --replication-factor 2 --partitions 2 --topic huanyu

#往Kafka的topic中写入数据(命令行的生成者)
/usr/local/java/kafka/bin/kafka-console-producer.sh --broker-list 192.168.73.128:9092 --topic cmccSmsQueue

#启动消费者
/usr/local/java/kafka/bin/kafka-console-consumer.sh --zookeeper 192.168.73.128:2181 --topic huanyu --from-beginning
/usr/local/java/kafka/bin/kafka-console-consumer.sh --zookeeper 192.168.73.128:2181 --topic cmccSmsQueue --from-beginning
/usr/local/java/kafka/bin/kafka-console-consumer.sh --zookeeper 192.168.73.128:2181 --topic cuccSmsQueue --from-beginning
/usr/local/java/kafka/bin/kafka-console-consumer.sh --zookeeper 192.168.73.128:2181 --topic ctccSmsQueue --from-beginning

-- 
/usr/local/java/kafka/bin/kafka-console-consumer.sh   --abc --bootstrap-server  node02:9092  --topic  cmccSmsQueue

# 消费者连接到borker的地址
##/usr/local/java/kafka/bin/kafka-console-consumer.sh --bootstrap-server node-###1.xiaoniu.com:9092,node-2.xiaoniu.com:9092,node-3.xiaoniu.com:9092 --topic huanyu --from-beginning 
## 查看分区
/usr/local/java/kafka/bin/kafka-topics.sh --describe --zookeeper 192.168.73.128:2181 --topic cmccSmsQueue 

## 根据group id

## 单机生产者
/usr/local/java/kafka/bin/kafka-console-producer.sh --broker-list 192.168.73.128:2181 --topic topic_demo

## 集群生产者
/usr/local/java/kafka/bin/kafka-console-consumer.sh --zookeeper 192.168.73.128:2181 --topic cmccSmsQueue --from-beginning

## 单机消费者
/usr/local/java/kafka/bin/kafka-console-consumer.sh --zookeeper 192.168.73.128:2181 --topic topic_demo --consumer-property group.id=group1

/usr/local/java/kafka/bin/kafka-console-consumer.sh --zookeeper 192.168.73.128:2181 --topic cmccSmsQueue spring.kafka.consumer.group-id=test-group

## 列表
/usr/local/java/kafka/bin/kafka-console-consumer.sh --zookeeper 192.168.73.128:2181 --topic huanyu --from-beginning
```



### kafka代码实现

[kafka producer  API实现](https://blog.csdn.net/bornzhu/article/details/78678253)

```
 metadata.broker.list：定义一个或者多个消息中介（broker），Produder通过broker决定主题leader的位置。这里无需   配置所有的broker，但建议配置多于一个。
  serializer.class:定义准备传递数据给broker时使用哪个序列化器。
  partitioner.class：这个是可选项，该类将决定消息将发送到哪个主题分区上。
  request.required.acks：该值设置为1后，broker收到消息后将发送一个确认信息给producer。
```

### kafka文件存储机制

> Kafka中发布订阅的对象是topic。我们可以为每类数据创建一个topic，把向topic发布消息的客户端称作producer，从topic订阅消息的客户端称作consumer。Producers和consumers可以同时从多个topic读写数据。一个kafka集群由一个或多个broker服务器组成，它负责持久化和备份具体的kafka消息。

####名词了解

```shell
## Broker：
Kafka节点，一个Kafka节点就是一个broker，多个broker可以组成一个Kafka集群。
## Topic：
一类消息，消息存放的目录即主题，例如page view日志、click日志等都可以以topic的形式存在，Kafka集群能够同时负责多个topic的分发。
## Partition：
topic物理上的分组，一个topic可以分为多个partition，每个partition是一个有序的队列
## Segment：
partition物理上由多个segment组成，每个Segment存着message信息
## Producer : 
生产message发送到topic
##Consumer : 
订阅topic消费message, consumer作为一个线程来消费
##Consumer Group：
一个Consumer Group包含多个consumer, 这个是预先在配置文件中配置好的。各个consumer（consumer 线程）可以组成一个组（Consumer group ），partition中的每个message只能被组（Consumer group ） 中的一个consumer（consumer 线程 ）消费，如果一个message可以被多个consumer（consumer 线程 ） 消费的话，那么这些consumer必须在不同的组。Kafka不支持一个partition中的message由两个或两个以上的consumer thread来处理，即便是来自不同的consumer group的也不行。它不能像AMQ那样可以多个BET作为consumer去处理message，这是因为多个BET去消费一个Queue中的数据的时候，由于要保证不能多个线程拿同一条message，所以就需要行级别悲观锁（for update）,这就导致了consume的性能下降，吞吐量不够。而kafka为了保证吞吐量，只允许一个consumer线程去访问一个partition。如果觉得效率不高的时候，可以加partition的数量来横向扩展，那么再加新的consumer thread去消费。这样没有锁竞争，充分发挥了横向的扩展性，吞吐量极高。这也就形成了分布式消费的概念。
##consumer group：
各个consumer可以组成一个组，每个消息只能被组中的一个consumer消费，如果一个消息可以被多个consumer消费的话，那么这些consumer必须在不同的组。
##消息状态：
在Kafka中，消息的状态被保存在consumer中，broker不会关心哪个消息被消费了被谁消费了，只记录一个offset值（指向partition中下一个要被消费的消息位置），这就意味着如果consumer处理不好的话，broker上的一个消息可能会被消费多次。
##消息持久化：
Kafka中会把消息持久化到本地文件系统中，并且保持极高的效率。
##消息有效期：
Kafka会长久保留其中的消息，以便consumer可以多次消费，当然其中很多细节是可配置的。
##批量发送：
Kafka支持以消息集合为单位进行批量发送，以提高push效率。
##push-and-pull:
Kafka中的Producer和consumer采用的是push-and-pull模式，即Producer只管向broker push消息，consumer只管从broker pull消息，两者对消息的生产和消费是异步的。
##Kafka集群中broker之间的关系：
不是主从关系，各个broker在集群中地位一样，我们可以随意的增加或删除任何一个broker节点。
##负载均衡方面：
Kafka提供了一个 metadata API来管理broker之间的负载（对Kafka0.8.x而言，对于0.7.x主要靠zookeeper来实现负载均衡）。
##同步异步：
Producer采用异步push方式，极大提高Kafka系统的吞吐率（可以通过参数控制是采用同步还是异步方式）。
##分区机制partition：
Kafka的broker端支持消息分区，Producer可以决定把消息发到哪个分区，在一个分区中消息的顺序就是Producer发送消息的顺序，一个主题中可以有多个分区，具体分区的数量是可配置的。分区的意义很重大，后面的内容会逐渐体现。
##离线数据装载：
Kafka由于对可拓展的数据持久化的支持，它也非常适合向Hadoop或者数据仓库中进行数据装载。
##插件支持：
现在不少活跃的社区已经开发出不少插件来拓展Kafka的功能，如用来配合Storm、Hadoop、flume相关的插件。
```

#### 备份机制

>备份机制是Kafka0.8版本的新特性，备份机制的出现大大提高了Kafka集群的可靠性、稳定性。有了备份机制后，Kafka允许集群中的节点挂掉后而不影响整个集群工作。一个备份数量为n的集群允许n-1个节点失败。在所有备份节点中，有一个节点作为lead节点，这个节点保存了其它备份节点列表，并维持各个备份间的状体同步。下面这幅图解释了Kafka的备份机制：
>

![img](https://img-blog.csdn.net/20140709200710601?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3VpZmVuZzMwNTE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### 代码

####细化配置

**生产者**

```shell
 #kafka-producer配置，官网=>https://kafka.apache.org/documentation/#producerconfigs
#集群地址
spring.kafka.producer.bootstrap-servers=192.168.199.128:9092,192.168.199.128:9093,192.168.199.128:9094
#指定创建信息nio-buffer缓冲区大小约1M
spring.kafka.producer.buffer-memory=1024000
#累计约1M条就发发送，必须小于缓冲区大小，否则报错无法分配内存（减少IO次数，过大则延时高，瞬间IO大）
spring.kafka.producer.batch-size=1024000
#默认0ms立即发送，不修改则上两条规则相当于无效（这个属性时个map列表，producer的其它配置也配置在这里，详细↑官网，这些配置会注入给KafkaProperties这个配置bean中，供#spring自动配置kafkaTemplate这个对象时使用）
spring.kafka.producer.properties.linger.ms=1000
#kafkaTempalte可以发送对象类型的消息，系列化为json，一般使用默认的string序列化器（对象则手动手动转为json）
spring.kafka.producer.value-serializer=org.springframework.kafka.support.serializer.JsonSerializer
#发送确认机制:acks=all或-1：leader会等待所有ISR中的follower同步完成的ack才commit(保证ISR副本都有数据leader才commit，吞吐率降低),acks=0：partition leader不会等待任何ISR中副本的commit（可能会有数据丢失，吞吐高），acks=1 kafka会把这条消息写到本地日志文件中
spring.kafka.producer.acks=1
#发送失败重试次数
spring.kafka.producer.retries=3
```

#### 消费者

```shell
#kafka-consumer配置，官网=>https://kafka.apache.org/documentation/#producerconfigs
spring.kafka.consumer.bootstrap-servers=192.168.199.128:9092,192.168.199.128:9093,192.168.199.128:9094
#消费组ID
spring.kafka.consumer.group-id=test-group
#当未初始化消费组偏移量或没找到是怎么办？自动偏移量重置到最早的earlist（这个值会导致新加入的消费者去消费较久以前最开始的大量信息）,最新的latest（新消费者消费最新消息）,还是none或其它值报错
spring.kafka.consumer.auto-offset-reset=latest
#一次最大拉取多少条消息，太多处理消息压力大，太少则IO过于频繁；这个配置需要额外培养一个批量工厂bean，并在@KafkaListener注解指定这个批量工厂{@Link https://www.jianshu.com/p/5370fff55cff}
spring.kafka.consumer.max-poll-records=10
#自动提交consumer已消费的消息offset周期,周期过大，重启后可能重复消费较多已消费但offset未提交的消息,kafka scala程序有个定时任务来提交offset
spring.kafka.consumer.auto-commit-interval=1000ms
#仅在不开启上述周期性自动确认，配置其他的ack-mode才有效，如下累计10次消费才进行一次commit以修改消息消费者在该partition的offset
spring.kafka.consumer.enable-auto-commit=false
#计数模式，还是计时模式，手动ack模式等，配合ack-count一起配置
spring.kafka.listener.ack-mode=count_time
spring.kafka.listener.ack-count=10
#消费者的其它属性，类似producer，也是一个map
#spring.kafka.consumer.properties.XXX=YYY
```

####路径

[编码实战](https://blog.csdn.net/wangzhanzheng/article/details/80801059)

[api](https://www.cnblogs.com/hd3013779515/p/6939013.html)

[api](https://blog.51cto.com/luchunli/1714857)

[kafka client写法](https://www.jianshu.com/p/daf1b68cf2a6)

```
项目
big-data-soa 下 big-data-kafka
```

### 如何删除topic

[删除topic数据](https://blog.csdn.net/magic_kid_2010/article/details/104009847?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-21.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-21.nonecase)

```shell
##修改 kafka/conf/server.properties，增加 delete.topic.enable=true，然后重启 kafka，通过命令行删除 kafkfa 即可
./kafka-topics.sh --delete --zookeeper localhost:2181  --topic test
```

### 生产者

#### 发送消息的方式

```
1.发送并忘记 2.同步发送 3.异步发送+回调函数
```

#### 如何保证可靠性

* 举个栗子

  比如关系型数据库的可靠性保证是 ACID，也就是原子性（Atomicity）、一致性（Consistency）、隔离性（Isolation）和持久性（Durability）。

	>- 对于一个分区来说，它的消息是有序的。如果一个生产者向一个分区先写入消息 A，然后写入消息 B，那么消费者会先读取消息 A 再读取消息 B。
	>- 当消息写入所有 in-sync 状态的副本后，消息才会认为已提交（committed）。 这里的写入有可能只是写入到文件系统的缓存，不一定刷新到磁盘。生产者可以等待不同时机的确认，比如等待分区主副本写入即返回，后者等待所有 in-sync 状态副本写入才返回。
	>- 一旦消息已提交，那么只要有一个副本存活，数据不会丢失。
	>- 消费者只能读取到已提交的消息。
	>
	>使用这些基础保证，我们构建一个可靠的系统，这时候需要考虑一个问题：究竟我们的应用需要多大程度的可靠性？
	>
	>可靠性不是无偿的，它与系统可用性、吞吐量、延迟和硬件价格息息相关，得此失彼。因此，我们往往需要做权衡，一味的追求可靠性并不实际。

####消息发送过程中会出现的问题

>1. 一个消息发送失败
>2. 一个消息被发送多次
>3. 最理想的情况：exactly-once ,一个消息发送成功且仅发送了一次     

```shell
有许多系统声称它们实现了exactly-once，但是它们其实忽略了生产者或消费者在生产和消费过程中有可能失败的情况。比如虽然一个Producer成功发送一个消息，但是消息在发送途中丢失，或者成功发送到broker，也被consumer成功取走，但是这个consumer在处理取过来的消息时失败了。
##从Producer端看：
Kafka是这么处理的，当一个消息被发送后，Producer会等待broker成功接收到消息的反馈（可通过参数控制等待时间），如果消息在途中丢失或是其中一个broker挂掉，Producer会重新发送（我们知道Kafka有备份机制，可以通过参数控制是否等待所有备份节点都收到消息）。
##从Consumer端看：
前面讲到过partition，broker端记录了partition中的一个offset值，这个值指向Consumer下一个即将消费message。当Consumer收到了消息，但却在处理过程中挂掉，此时Consumer可以通过这个offset值重新找到上一个消息再进行处理。Consumer还有权限控制这个offset值，对持久化到broker端的消息做任意处理。
```

### 消费者

####如何消息不从头消费

[怎么避免不从头消费](https://www.cnblogs.com/jun1019/p/6700923.html)

