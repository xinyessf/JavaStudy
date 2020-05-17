### 什么是kafka

>
>
>

```

```

### 集群模式

>
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

**安装**

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
## 分别启动
/usr/local/java/kafka/bin/kafka-server-start.sh -daemon /usr/local/java/kafka/config/server.properties 

#查看topic信息
/usr/local/java/kafka/bin/kafka-topics.sh --list --zookeeper 192.168.73.128:2181

#创建topic
/usr/local/java/kafka/bin/kafka-topics.sh --create --zookeeper 192.168.73.128:2181 --replication-factor 3 --partitions 3 --topic huanyu

#往Kafka的topic中写入数据(命令行的生成者)
/usr/local/java/kafka/bin/kafka-console-producer.sh --broker-list 192.168.73.128:9092 --topic huanyu

#启动消费者
/usr/local/java/kafka/bin/kafka-console-consumer.sh --zookeeper 192.168.73.128:2181 --topic huanyu --from-beginning


# 消费者连接到borker的地址
##/usr/local/java/kafka/bin/kafka-console-consumer.sh --bootstrap-server node-###1.xiaoniu.com:9092,node-2.xiaoniu.com:9092,node-3.xiaoniu.com:9092 --topic huanyu --from-beginning 
## 查看分区

```

