##zookeeper

[官网](https://zookeeper.apache.org/doc/current/zookeeperOver.html)

### 什么是zookeeper

>Zookeeper是一个分布式开源框架，提供了协调分布式应用的基本服务，它向外部应用暴露一组通用服务——分布式同步（Distributed Synchronization）、命名服务（Naming Service）、集群维护（Group Maintenance）等，简化分布式应用协调及其管理的难度，提供高性能的分布式服务。ZooKeeper本身可以以单机模式安装运行，不过它的长处在于通过分布式ZooKeeper集群（一个Leader，多个Follower），基于一定的策略来保证ZooKeeper集群的稳定性和可用性，从而实现分布式应用的可靠性。
>
>1、zookeeper是为别的分布式程序服务的
>
>2、Zookeeper本身就是一个分布式程序（只要有半数以上节点存活，zk就能正常服务）
>
>3、Zookeeper所提供的服务涵盖：主从协调、服务器节点动态上下线、统一配置管理、分布式共享锁、统> 一名称服务等
>
>4、虽然说可以提供各种服务，但是zookeeper在底层其实只提供了两个功能：
>
>管理(存储，读取)用户程序提交的数据（类似namenode中存放的metadata）； 
> 并为用户程序提供数据节点监听服务；

#### 集群机制

>Zookeeper集群的角色： Leader 和 follower 
> 只要集群中有半数以上节点存活，集群就能提供服务

#### Zookeeper特性

>1、Zookeeper：一个leader，多个follower组成的集群
>
>2、全局数据一致：每个server保存一份相同的数据副本，client无论连接到哪个server，数据都是一致的
>
>3、分布式读写，更新请求转发，由leader实施
>
>4、更新请求顺序进行，来自同一个client的更新请求按其发送顺序依次执行
>
>5、数据更新原子性，一次数据更新要么成功，要么失败
>
>6、实时性，在一定时间范围内，client能读到最新数据

#### 数据结构

[数据结构](https://www.cnblogs.com/xums/p/7074008.html)

### zookeeper应用场景

>统一命名服务
>
>​    分布式环境下，经常需要对应用/服务进行统一命名，便于识别不同服务。类似于域名与ip之间对应关系，域名容易记住。通过名称来获取资源或服务的地址，提供者等信息按照层次结构组织服务/应用名称可将服务名称以及地址信息写到Zookeeper上，客户端通过Zookeeper获取可用服务列表类。
>
>配置管理
>
>​    分布式环境下，配置文件管理和同步是一个常见问题。一个集群中，所有节点的配置信息是一致的，比如Hadoop。对配置文件修改后，希望能够快速同步到各个节点上配置管理可交由Zookeeper实现。可将配置信息写入Zookeeper的一个znode上。各个节点监听这个znode。一旦znode中的数据被修改，zookeeper将通知各个节点。
>
>集群管理
>
>​    分布式环境中，实时掌握每个节点的状态是必要的。可根据节点实时状态作出一些调整。Zookeeper可将节点信息写入Zookeeper的一个znode上。监听这个znode可获取它的实时状态变化。典型应用比如Hbase中Master状态监控与选举。
>
>分布式通知/协调
>
>​    分布式环境中，经常存在一个服务需要知道它所管理的子服务的状态。例如，NameNode须知道各DataNode的状态，JobTracker须知道各TaskTracker的状态。心跳检测机制和信息推送也是可通过Zookeeper实现。
>
>分布式锁
>
>​    Zookeeper是强一致的。多个客户端同时在Zookeeper上创建相同znode，只有一个创建成功。Zookeeper实现锁的独占性。多个客户端同时在Zookeeper上创建相同znode ，创建成功的那个客户端得到锁，其他客户端等待。Zookeeper 控制锁的时序。各个客户端在某个znode下创建临时znode （类型为CreateMode. EPHEMERAL _SEQUENTIAL），这样，该znode可掌握全局访问时序。
>
>分布式队列
>
>​    两种队列。当一个队列的成员都聚齐时，这个队列才可用，否则一直等待所有成员到达，这种是同步队列。队列按照 FIFO 方式进行入队和出队操作，例如实现生产者和消费者模型。（可通过分布式锁实现）
>​     同步队列。一个job由多个task组成，只有所有任务完成后，job才运行完成。可为job创建一个/job目录，然后在该目录下，为每个完成的task创建一个临时znode，一旦临时节点数目达到task总数，则job运行完成。

### zookeeper的环境搭建

[路径](https://blog.csdn.net/xiaolang85/article/details/13021339)

>一共三个节点
> (zk服务器集群规模不小于3个节点),要求服务器之间系统时间保持一致。

**192.168.73.128**

```shell

####进行解压： 
tar -zxvf zookeeper-3.4.6.tar.gz
##重命名：
mv zookeeper-3.4.6 zookeeper
## 配置环境变量
vi /etc/profile
## export JAVA_HOME=/opt/jdk1.8.0_71
export ZOOKEEPER_HOME=/usr/local/zookeeper
## export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$JAVA_HOME/bin:$ZOOKEEPER_HOME/bin:$PATH
source /etc/profile
##修改zoo_sample.cfg文件
cd /usr/local/zookeeper/conf
mv zoo_sample.cfg zoo.cfg
vi zoo.cfg
##修改conf: vi zoo.cfg 修改两处
##（1） dataDir=/usr/local/zookeeper/data（注意同时在zookeeper创建data目录）
dataDir=/usr/local/zookeeper/data
##（2）最后面添加
server.0=192.168.73.128:2888:3888
server.1=192.168.73.129:2888:3888
server.2=192.168.73.130:2888:3888
cd /usr/local/zookeeper/data
vi myid 
0
##复制其它其它节点
scp -r /usr/local/zookeeper  hdp-02:/usr/local/zookeeper
scp -r /usr/local/zookeeper  hdp-01:/usr/local/zookeeper
## 修改其它的配置文件
vi /usr/local/zookeeper/data/myid
##
/usr/local/zookeeper/bin/zkServer.sh start
/usr/local/zookeeper/bin/zkServer.sh restart
/usr/local/zookeeper/bin/zkServer.sh stop
/usr/local/zookeeper/bin/zkServer.sh status
zkServer.sh status 查询状态
## 命令使用
/usr/local/zookeeper/bin/zkCli.sh  ##启动
##创建文件，并设置初始内容： create /zk "test" 创建一个新的 znode节点“ zk ”以及与它关联的字符串
ls /
create /zk "test"
```

### zk语法

```shell
/usr/local/zookeeper/bin/zkCli.sh  ##启动
##创建文件，并设置初始内容： create /zk "test" 创建一个新的 znode节点“ zk ”以及与它关联的字符串
ls /
## 创建
create /mygirls "angelbaby"
## 改
set /mygirls "yangmi"
set /servers "yangmi"
# 
create  /mygirls/node "zijiedian"
```

### zookeeper可视化工具

[可视化工具]([https://www.cnblogs.com/kylingx/p/12916345.html](https://www.cnblogs.com/kylingx/p/12916345.html)

```
java -jar zookeeper-dev-ZooInspector.jar
```

### zookeeper 集群机制

```shell
可用/不可用
leader 是否可用的状态/不可用状态
节点,不要把zookeeper当作数据库使用,需要快速,使用
## 如何使用 node
zookeeper 是一个目录树结构,node 可以存数据1M,分为持久节点和临时节点
临时节点的 锁释放,靠的session
序列节点
## 完整属性
集群创建节点,要么全部成功,要么全部失败
## 
```

![img](img/zk1.jpg)

```

```



#### 了解



