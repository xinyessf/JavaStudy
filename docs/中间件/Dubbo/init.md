## Dubbo

###了解Dubbo

####Dubbo的应用背景

>随着互联网的发展，网站应用的规模不断扩大，常规的垂直应用架构已无法应对，分布式服务架构以及流动计算架构势在必行，亟需一个治理系统确保架构有条不紊的演进。 

####什么是Dubbo

>Dubbo是一个分布式服务框架，致力于提供高性能和透明化的RPC远程服务调用方案，SOA服务治理方案。简单的说，dubbo就是个服务框架，如果没有分布式的需求，其实是不需要用的，只有在分布式的时候，才有dubbo这样的分布式服务框架的需求，并且本质上是个服务调用，说白了就是个远程服务调用的分布式框架。告别Web Service模式中的wsdl,以服务者与消费者的方式在dubbo 上注册)。
>
>其核心部分包含:
>
>1.远程通讯，提供对多种基于长连接的NIO框架抽象封装，包括多种线程模型，序列化，以及“请求一响应”模式的信息交换方式。
>
>2.集群容错: 提供基于接口方法的透明远程过程调用，包括多协议支持。以
>
>及负载均衡，失败容错，地址路由，动态配置等集群支持。
>
>**3.****自动发现:基于注册中心目录服务，使用服务消费能动态查找服务提供方,使地址透明,使用服务提供方可以平滑增加或减少服务器**

### Dubbo架构

![](https://img-blog.csdn.net/20180902123447214?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2Jhb3l1X0c=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

#### 节点角色说明：

```
Provider: 暴露服务的服务提供方。 
Consumer: 调用远程服务的服务消费方。 
Registry: 服务注册与发现的注册中心。 
Monitor: 统计服务的调用次调和调用时间的监控中心。
```

#### 调用关系说明：

服务容器负责启动，加载，运行服务提供者。 
 服务提供者在启动时，向注册中心注册自己提供的服务。 
 服务消费者在启动时，向注册中心订阅自己所需的服务。 
 注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送变更数据给消费者。 
 服务消费者，从提供者地址列表中，基于软负载均衡算法，选一台提供者进行调用，如果调用失败，再选另一台调用。 
 服务消费者和提供者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心。 
 \- 连通性： 
 注册中心负责服务地址的注册与查找，相当于目录服务，服务提供者和消费者只在启动时与注册中心交互，注册中心不转发请求，压力较小 
 监控中心负责统计各服务调用次数，调用时间等，统计先在内存汇总后每分钟一次发送到监控中心服务器，并以报表展示 
 服务提供者向注册中心注册其提供的服务，并汇报调用时间到监控中心，此时间不包含网络开销 
 服务消费者向注册中心获取服务提供者地址列表，并根据负载算法直接调用提供者，同时汇报调用时间到监控中心，此时间包含网络开销 
 注册中心，服务提供者，服务消费者三者之间均为长连接，监控中心除外 
 注册中心通过长连接感知服务提供者的存在，服务提供者宕机，注册中心将立即推送事件通知消费者 
 注册中心和监控中心全部宕机，不影响已运行的提供者和消费者，消费者在本地缓存了提供者列表 
 注册中心和监控中心都是可选的，服务消费者可以直连服务提供者

#### 健壮性

>监控中心宕掉不影响使用，只是丢失部分采样数据 
> 数据库宕掉后，注册中心仍能通过缓存提供服务列表查询，但不能注册新服务 
> 注册中心对等集群，任意一台宕掉后，将自动切换到另一台 
> 注册中心全部宕掉后，服务提供者和服务消费者仍能通过本地缓存通讯 
> 服务提供者无状态，任意一台宕掉后，不影响使用 
> 服务提供者全部宕掉后，服务消费者应用将无法使用，并无限次重连等待服务提供者恢复

#### 伸缩性：

>注册中心为对等集群，可动态增加机器部署实例，所有客户端将自动发现新的注册中心 
> 服务提供者无状态，可动态增加机器部署实例，注册中心将推送新的服务提供者信息给消费者

#### 升级性

### Dubbo环境搭建

>生产者
>
>消费者

#### dubbo-admin使用

>1. 将dubbo-admin.zip 解压到webapps目录下
>
>2. 修改dubbo.properties zk注册中心连接地址连接信息(apache-tomcat-7.0.94\webapps\dubbo-admin-2.5.8\WEB-INF)
>
>3. 启动tomcat即可

## 面试

####什么是Dubbo？

Duubbo是一个RPC远程调用框架， 分布式服务治理框架

什么是Dubbo服务治理？

服务与服务之间会有很多个Url、依赖关系、负载均衡、容错、自动注册服务。

####Dubbo整个架构流程

```
分为四大模块
生产者、消费者、注册中心、监控中心
生产者：提供服务
消费者: 调用服务
注册中心:注册信息(redis、zk)
监控中心:调用次数、关系依赖等。
首先生产者将服务注册到注册中心（zk），使用zk持久节点进行存储，消费订阅zk节点，一旦有节点变更，zk通过事件通知传递给消费者，消费可以调用生产者服务。
服务与服务之间进行调用，都会在监控中心中，存储一个记录。
```

#### RPC远程调用框架

```
SpringCloud、dubbo、Dubbox、thint、Hessian…
Rpc其实就是远程调用，服务与服务之间相互进行通讯。
目前主流 用http+json
```

#### SpringCloud与Dubbo区别？

```
dubbo与springcloud都可以实现RPC远程调用。
dubbo与springcloud都可以使用分布式、微服务场景下。
```

