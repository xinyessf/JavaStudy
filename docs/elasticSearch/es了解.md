##简介
###Elasticsearch简单介绍

```
Elasticsearch (ES)是一个基于Lucene构建的开源、分布式、RESTful 接口全文搜索引擎。Elasticsearch 还是一个分布式文档数据库，其中每个字段均是被索引的数据且可被搜索，它能够扩展至数以百计的服务器存储以及处理PB级的数据。它可以在很短的时间内在储、搜索和分析大量的数据。它通常作为具有复杂搜索场景情况下的核心发动机。
Elasticsearch就是为高可用和可扩展而生的。可以通过购置性能更强的服务器来完成。
```
###优势
```
横向可扩展性:只需要增加台服务器，做一点儿配置，启动一下Elasticsearch就可以并入集群。

分片机制提供更好的分布性:同一个索引分成多个分片(sharding), 这点类似于HDFS的块机制;分而治之的方式可提升处理效率。
    
高可用:提供复制( replica) 机制，一个分片可以设置多个复制，使得某台服务器在宕机的情况下，集群仍旧可以照常运行，并会把服务器宕机丢失的数据信息复制恢复到其他可用节点上。
口使用简单:共需一条命令就可以下载文件，然后很快就能搭建一一个站内搜索引擎。

```
###应用场景
```
大型分布式日志分析系统ELK  elasticsearch（存储日志）+logstash(收集日志)+kibana(展示数据)

大型电商商品搜索系统、网盘搜索引擎等。

```
###Elasticsearch存储结构
```
Elasticsearch是文件存储，Elasticsearch是面向文档型数据库，一条数据在这里就是一个文档，用JSON作为文档序列化的格式，比如下面这条用户数据：
{
    "name" :     "sq",
    "sex" :      0,
    "age" :      25
}
关系数据库     ⇒ 数据库 ⇒ 表    ⇒ 行    ⇒ 列(Columns)
Elasticsearch    ⇒ 索引(Index)   ⇒ 类型(type)  ⇒ 文档(Docments)  ⇒ 字段(Fields)  
```
###es_Linux环境安装单机
[教程](https://blog.csdn.net/luo1544943710/article/details/93196147)

**1安装jdk**

**2.下载elasticsearch安装包**

官方文档https://www.elastic.co/downloads/elasticsearch
注意：linux安装内存建议1g内存以上
**3.上传elasticsearch安装包**

**4.解压elasticsearch**

**5.修改 elasticsearch.yml**

**6.启动报错,百度**
1.java 端口9300 ,http端口9200

这里加用户密码为:fun-es-es
chown es /usr/local/java/es/elasticsearch-6.4.3/ -R

### 安装完成

访问地址:http://192.168.73.130:9200/

```
后台启动
cd /usr/local/java/es/elasticsearch-6.4.3/bin
./elasticsearch -d
ps -ef|grep elasticsearch
```

### es集群

#### 倒叙索引

>数据索引

```
包含当前关键词的documentlist
关键词在每个document中出现的次数，即tern frequency（TF），TF越高，则匹配相关度越高
关键词在整个索引中出现的次数  inverse document frequency（IDF），IDF越高，则匹配相关度越低
关键词在当前document中出现的次数
每个doc的长度，越长相关度越低
包含这个关键词的所有doc的平均长度
```

####正序索引

```
es官方是建议，es大量是基于os cache来进行缓存和提升性能的，不建议用jvm内存来进行缓存，那样会导致一定的gc开销和oom问题，给jvm更少的内存，给os cache更大的内存。比如64g服务器，给jvm最多4~16g（1/16~1/4），os cache可以提升doc value和倒排索引的缓存和查询效率
```

![](img/图解-正排索引VS倒排索引.jpg)

#### Filter缓存

![](img/Filter缓存原理.png)

```
过滤器缓存，增加查询速度
```



####为什么要集群

[分片](https://blog.csdn.net/weixin_39471249/article/details/80600176)

```
ES集群中索引可能由多个分片构成，并且每个分片可以拥有多个副本。通过将一个单独的索引分为多个分片，我们可以处理不能在一个单一的服务器上面运行的大型索引，简单的说就是索引的大小过大，导致效率问题。不能运行的原因可能是内存也可能是存储。由于每个分片可以有多个副本，通过将副本分配到多个服务器，可以提高查询的负载能力。
```

 ![这里写图片描述](https://www.elastic.co/guide/cn/elasticsearch/guide/current/images/elas_0402.png) 

#### 集群原理

```
ES集群核心原理分析:
 数据存储。
1、每个索引会被分成多个分片shards进行存储，默认创建索引是分配5个分片进行存储。
每个分片都会分布式部署在多个不同的节点上进行部署，该分片成为primary shards。
   注意：索引的主分片primary shards定义好后，后面不能做修改。

2、为了实现高可用数据的高可用，主分片可以有对应的备分片replics shards，replic shards分片承载了负责容错、以及请求的负载均衡。
  注意: 每一个主分片为了实现高可用，都会有自己对应的备分片，主分片对应的备分片不能存放同一台服务器上。，主分片primary shards可以和其他replics shards存放在同一个node节点上。
```

#### 容错机制

```

```

![](img/ES容错机制 .png)

####选举机制

![img](img/Master选举.png)



####集群搭建7.6.0

[百度网盘下载](https://blog.csdn.net/weixin_37281289/article/details/101483434)

[搭建详细步骤](https://blog.csdn.net/xiaoge335/article/details/100575925)

[7.6.0安装](https://www.cnblogs.com/hxuhongming/p/12825695.html)

[Docker搭建ES集群](https://www.cnblogs.com/toov5/p/11361413.html)

[es集群原理和搭建](https://www.cnblogs.com/soft2018/p/10213266.html)

[安装]( https://blog.csdn.net/Aolani_L/article/details/105874159)

[原理](https://www.cnblogs.com/leeSmall/p/9220535.html)

>自带的jdk11,所以不用安装jdk即可使用

**7.6.0安装**

```shell

##3台 ，需要添加域名  jdk11
192.168.73.134 hbase-01
192.168.73.135 hbase-02
192.168.73.136 node-136

## 在 hbase-01
/usr/local/bigdata/
rz xx
tar -zxvf elasticsearch-7.6.0.tar.gz
cd elasticsearch-7.6.0
## 
cp elasticsearch.yml elasticsearch.yml.bak
vi elasticsearch.yml
## 修改jvm.options
-Xms512m
-Xmx512m
##
scp -r /usr/local/bigdata/hbase-1.2.1  192.168.73.135:/usr/local/bigdata/hbase-1.2.1
scp -r /usr/local/bigdata/elasticsearch-7.6.0 hbase-02:/usr/local/bigdata
scp -r /usr/local/bigdata/elasticsearch-7.6.0 node-136:/usr/local/bigdata
## 分别修改 node.name: hbase-01
## 添加用户
adduser es     #创建普通用户
passwd es      #修改es用户密码(需连续输入2次)  此处未设置密码
## 赋予es普通用户root权限
##vi /etc/sudoers
##chmod 777 /etc/sudoers
##es ALL=(ALL) NOPASSWD: ALL
## 赋予es用户es安装目录的文件权限
cd /usr/local/bigdata
chown -R es:es elasticsearch-7.6.0 #赋予es用户安装文件elasticsearch-7.6.0的权限
## 去掉jdk
vi /etc/profile
source /etc/profile
source ~/.bashrc
## 启动
su es      #由root用户切换到es用户
cd /usr/local/bigdata/elasticsearch-7.6.0
bin/elasticsearch
## 后台启动
./bin/elasticsearch -d 
## 报错，必须使用java11
```

```shell
# 设置集群名称，集群内所有节点的名称必须一致。
cluster.name: myes

# 表示该节点会不会作为主节点，true表示会；false表示不会
node.master: true
# 当前节点是否用于存储数据，是：true、否：false
node.data: true
# 索引数据存放的位置
path.data: /usr/local/bigdata/elasticsearch-7.6.0/data
# 日志文件存放的位置
path.logs: /usr/local/bigdata/elasticsearch-7.6.0/logs

#每个节点名称不一样 其他两台为node-2 ,node-3
node.name: hbase-01

#### 实际服务器ip地址
network.host: 0.0.0.0

# es对外提供的http端口，默认 9200
http.port: 9200
# TCP的默认监听端口，默认 9300

#transport.tcp.port: 9300

discovery.seed_hosts: ["192.168.73.134", "192.168.73.135","192.168.73.136"]
#
# Bootstrap the cluster using an initial set of master-eligible nodes:
#
cluster.initial_master_nodes: ["hbase-01", "hbase-02", "node-136"]

http.cors.enabled: true
http.cors.allow-origin: "*"
```

##### 常用url路径查看集群

```shell
192.168.73.134:9200
192.168.73.135:9200
192.168.73.136:9200
##集群健康状态查看
http://192.168.73.136:9200/_cluster/health?pretty=true
```

#### Head插件安装

[配置图形化界面工具Head插件](https://blog.csdn.net/D_Janrry/article/details/105461236?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.nonecase)

> 在Elasticsearch5.x版本以后，head插件已经是一个独立的Web App了，所以不需要和Elasticsearch进行集成。可以将Head插件安装到任何一台机器上，我就布置在我的第一个节点了。 

[github head源码](https://github.com/mobz/elasticsearch-head)

[head无法连接](https://blog.csdn.net/CsethCRM/article/details/90607095)

```shell
##1.安装node，看教程
##2.head插件下载
git clone git://github.com/mobz/elasticsearch-head.git
## 修改配置
elasticsearch-head/_site/app.js
中的get("app-base_api")|| 将localhost改为集群中的一个ip
## 可以window中下载打包，在linux中运行都可以，内网访问才可以
npm install
npm run start
http://localhost:9100/
## 访问

```

####5.6.0安装

[5.6.0安装](https://blog.csdn.net/u013571608/article/details/78479949)

[5.6.0集群搭建](https://blog.csdn.net/weixin_34192816/article/details/92166673)

[5.6.0](https://blog.csdn.net/sinat_42724379/article/details/102590447?utm_medium=distribute.pc_relevant.none-task-blog-OPENSEARCH-19.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-OPENSEARCH-19.nonecase)

>vi /etc/sysconfig/network
>
>weget  https://artifacts.elastic.co/downloads/kibana/kibana-5.6.0-linux-x86_64.tar.gz

**启动**

```shell
## 启动es
su es      #由root用户切换到es用户
cd /usr/local/java/elasticsearch-5.6.0 
bin/elasticsearch
./bin/elasticsearch -d 
#http://192.168.73.128:9200/_cluster/health?pretty=true
## 启动kibana 192.168.73.128
cd /usr/local/java/elasticsearch-5.6.0
cd /usr/local/java/kibana-5.6.0-linux-x86_64/bin
## 启动header 在window下
./kibana
### http://192.168.73.128:5601/app/kibana
```

**搭建**

```shell
##ip : 192.168.73.128  192.168.73.129
##路径
/usr/local/java/elasticsearch-5.6.0/data
##1.如何操作集群搭建
chown -R es:es elasticsearch-5.6.0
## 启动
su es      #由root用户切换到es用户
cd /usr/local/java/elasticsearch-5.6.0 
bin/elasticsearch
## 后台启动
./bin/elasticsearch -d 
## 报错，必须使用java8
##http://192.168.73.128:9200/_cluster/health?pretty=true

## slave
scp -r /usr/local/java/elasticsearch-5.6.0 hdp-02:/usr/local/java
```

** 设置集群配置**

```shell
# 设置集群名称，集群内所有节点的名称必须一致。
cluster.name: myes
# 表示该节点会不会作为主节点，true表示会；false表示不会
node.master: true
# 当前节点是否用于存储数据，是：true、否：false
node.data: true
# 索引数据存放的位置
path.data: /usr/local/java/elasticsearch-5.6.0/data
# 日志文件存放的位置
path.logs: /usr/local/java/elasticsearch-5.6.0/logs
#每个节点名称不一样 其他两台为node-2 ,node-3
node.name: hdp-01
#### 实际服务器ip地址
network.host: 0.0.0.0
# es对外提供的http端口，默认 9200
http.port: 9200
# TCP的默认监听端口，默认 9300
#transport.tcp.port: 9300
# 指定将当前节点的master的ip
##discovery.zen.ping.unicast.hosts: ["192.168.73.128"]
# Bootstrap the cluster using an initial set of master-eligible nodes:
#
##cluster.initial_master_nodes: ["hbase-01", "hbase-02", "node-136"]
http.cors.enabled: true
http.cors.allow-origin: "*"
```



## kibana

[国内下载kibana](https://www.newbe.pro/Mirrors/Mirrors-Kibana/)

###kibana简介

```
Kibana是一个基于Node.js的Elasticsearch索引库数据统计工具，可以利用Elasticsearch的聚合功能，生成各种图表，如柱形图，线状图，饼图等。
而且还提供了操作Elasticsearch索引数据的控制台，并且提供了一定的API提示，非常有利于我们学习Elasticsearch的语法。
```
[配置](https://www.cnblogs.com/snail90/p/11639859.html)
[教程](https://blog.csdn.net/HcJsJqJSSM/article/details/82972137)

[汉化地址](https://github.com/anbai-inc/Kibana_Hanization)

访问地址:http://192.168.73.130:5601/

[界面教程](https://blog.csdn.net/hanghangaidoudou/article/details/100182437)

```shell
## ip  192.168.73.134上操作
chown -R es:es /
##
cd /usr/local/bigdata/kibana
cd config
vi kibana.yml
## 修改
server.host=192.168.73.136
server.port=5601
elasticsearch.url:	
elasticsearch.hosts: ["http://192.168.73.134:9200","http://192.168.73.135:9200","http://192.168.73.136:9200"]
## cd bin
./kibana
## http://192.168.73.136:5601/app/kibana
```



### kibana语法使用

#### 创建索引

Elasticsearch采用Rest风格API，因此其API就是一次http请求，你可以用任何工具发起http请求

创建索引的请求格式：

- 请求方式：PUT
- 请求路径：/索引库名
- 请求参数：json格式：

```
{
    "settings": {
        "number_of_shards": 3,
        "number_of_replicas": 2
      }
}
number_of_shards: 分片数量
number_of_replicas:副本数量
```

**创建**

```
##restful创建:
PUT请求 http://192.168.73.131:9200/fun
##kibana创建:
PUT fun
```

#### 查看索引

```
##restful创建:
GET请求 http://192.168.73.131:9200/fun
GET /索引库名
GET fun
GET * 查看所有索引
###
HEAD fun  查看索引是否存在
```

#### 删除索引

```
##restful创建:
DELETE请求 http://192.168.73.131:9200/fun
##DELETE /索引库名
DELETE fun
```

###安装完成

```
后台启动:
ps -ef|grep kibana
##查看端口号
netstat -anp|grep 5601
##重新启动
nohup ./kibana &
```

##ik分词器

```
使用说明
scp -r /usr/local/bigdata/elasticsearch-7.6.0/plugins/ik hbase-01:/usr/local/bigdata/elasticsearch-7.6.0/plugins/ik
scp -r /usr/local/bigdata/elasticsearch-7.6.0/plugins/ik hbase-02:/usr/local/bigdata/elasticsearch-7.6.0/plugins/ik
```

[下载地址](https://github.com/medcl/elasticsearch-analysis-ik/releases)

[使用说明](https://blog.csdn.net/jiayoubing/article/details/105351519)

#### 如何自定义分词

[自定义分词](https://blog.csdn.net/qq_32502511/article/details/86229444)

```
测试
get请求:
http://192.168.73.136:9200/_analyze
参数:
{
	"analyzer":"ik_max_word",
	"text":"成都太一群穆斯林聚在一起美航空技术大量波音767-300ER的垃圾林"
}
```

## 操作索引

*mysql方便看数据*

```mysql
CREATE TABLE IF NOT EXISTS `product` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(50) COLLATE utf8mb4_unicode_ci DEFAULT NULL,
  `desc` varchar(300) COLLATE utf8mb4_unicode_ci DEFAULT NULL,
  `price` decimal(10,0) DEFAULT NULL,
  `tags` varchar(200) COLLATE utf8mb4_unicode_ci DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- Dumping data for table test_db.product: ~5 rows (大约)
/*!40000 ALTER TABLE `product` DISABLE KEYS */;
REPLACE INTO `product` (`id`, `name`, `desc`, `price`, `tags`) VALUES
	(1, 'xiaomi phone', 'shouji zhong de zhandouji', 13999, '"xingjiabi", "fashao","buka"'),
	(2, 'xiaomi nfc phone', 'zhichi quangongneng nfc,shouji zhong de jianjiji', 4999, '"xingjiabi", "fashao","gongjiaoka"'),
	(3, 'nfc phone', 'shouji zhong de hongzhaji ', 2999, '"xingjiabi", "fashao",\n"menjinka"'),
	(4, 'xiaomi erji', 'erji zhong de huangmenji', 999, '"low", "bufangshui","yinzhicha"'),
	(5, 'hongmi erji', 'erji zhong de kendeji nfc', 399, '"lowbee","xuhangduan",\n "zhiliangx"');
```

### 映射配置

索引有了，接下来肯定是添加数据。但是，在添加数据之前必须定义映射。

什么是映射？

> 映射是定义文档的过程，文档包含哪些字段，这些字段是否保存，是否索引，是否分词等

只有配置清楚，Elasticsearch才会帮我们进行索引库的创建（不一定）

#### 创建映射配置

```
PUT /索引库名/_mapping/类型名称
{
  "properties": {
    "字段名": {
      "type": "类型",
      "index": true，
      "store": true，
      "analyzer": "分词器"
    }
  }
}
```

- 类型名称：就是前面将的type的概念，类似于数据库中的不同表
  字段名：任意填写	，可以指定许多属性，例如：
- type：类型，可以是text、long、short、date、integer、object等
- index：是否索引，默认为true
- store：是否存储，默认为false
- analyzer：分词器，这里的`ik_max_word`即使用ik分词器

> 创建索引

```
PUT fun/_mapping/goods
{
  "properties": {
    "title": {
      "type": "text",
      "analyzer": "ik_max_word"
    },
    "images": {
      "type": "keyword",
      "index": "false"
    },
    "price": {
      "type": "float"
    }
  }
}
```

#### 查看映射配置

```
GET /索引库名/_mapping
GET /fun/_mapping
```

#### 字段属性

#####type

我们说几个关键的：

- String类型，又分两种：

  - text：可分词，不可参与聚合
  - keyword：不可分词，数据会作为完整字段进行匹配，可以参与聚合

- Numerical：数值类型，分两类

  - 基本数据类型：long、interger、short、byte、double、float、half_float
  - 浮点数的高精度类型：scaled_float
    - 需要指定一个精度因子，比如10或100。elasticsearch会把真实值乘以这个因子后存储，取出时再还原。

- Date：日期类型

  elasticsearch可以对日期格式化为字符串存储，但是建议我们存储为毫秒值，存储为long，节省空间。

#####index

index影响字段的索引情况。

- true：字段会被索引，则可以用来进行搜索。默认值就是true
- false：字段不会被索引，不能用来搜索

index的默认值就是true，也就是说你不进行任何配置，所有字段都会被索引。

但是有些字段是我们不希望被索引的，比如商品的图片信息，就需要手动设置index为false

#####store

是否将数据进行额外存储。

在学习lucene和solr时，我们知道如果一个字段的store设置为false，那么在文档列表中就不会有这个字段的值，用户的搜索结果中不会显示出来。

但是在Elasticsearch中，即便store设置为false，也可以搜索到结果。

原因是Elasticsearch在创建文档索引时，会将文档中的原始数据备份，保存到一个叫做`_source`的属性中。而且我们可以通过过滤`_source`来选择哪些要显示，哪些不显示。

而如果设置store为true，就会在`_source`以外额外存储一份数据，多余，因此一般我们都会将store设置为false，事实上，**store的默认值就是false。**

##### boost

### 数据操作

#### 新增数据

```
（1）创建索引：PUT /product?pretty
（2）查询索引：GET _cat/indices?v
（3）删除索引：DELETE /product?pretty
（4）插入数据：
PUT /index/_doc/id
{
    Json数据
}
（5）更新数据
1)	全量替换
2)	指定字段更新
（6）删除数据	DELETE /index/type/id
```

```
POST /索引库名/类型名
{
    "key":"value"
}
##示例:
POST /fun/goods/
{
    "title":"小米手机",
    "images":"http://image.leyou.com/12479122.jpg",
    "price":2699.00
}
##查看
GET _search
{
  "query": {
    "match_all": {}
  }
}
```

- `_source`：源文档信息，所有的数据都在里面。
- `_id`：这条文档的唯一标示，与文档自己的id字段没有关联

**自定义id**

```
POST /索引库名/类型/id值
{
    ...
}
POST /fun/goods/2
{
    "title":"大米手机",
    "images":"http://image.leyou.com/12479122.jpg",
    "price":2899.00
}
```

**智能映射**

```
放入非文本,可以自动映射为文本类型
200 -->"200"
```

#### 修改数据

```
把刚才新增的请求方式改为PUT，就是修改了。不过修改必须指定id，

- id对应文档存在，则修改
- id对应文档不存在，则新增

比如，我们把id为3的数据进行修改：
POST请求一样会修改
PUT /fun/goods/2
{
    "title":"超大米手机",
    "images":"http://image.leyou.com/12479122.jpg",
    "price":3899.00,
    "stock": 100,
    "saleable":true
}
```

#### 删除

```
DELETE /索引库名/类型名/id值
```

## 查询

```
新增数据
PUT /fun/goods/5
{
    "title":"小米电视4A",
    "images":"http://image.leyou.com/12479122.jpg",
    "price":3899.00,
    "subTitle":"岁寒"
}

PUT /fun/goods/4
{
    "title":"岁寒",
    "images":"http://image.leyou.com/12479122.jpg",
    "price":3899.00,
    "subTitle":"哈比"
}
```



###基本查询

####查询所有（match_all)

```
GET /fun/_search
{
    "query":{
        "match_all": {}
    }
}
##
query：代表查询对象
match_all：代表查询所有
```

####匹配查询

```
##使用
GET /fun/_search
##match类型查询，会把查询条件进行分词，然后进行查询,多个词条之间是or的关系
{
    "query":{
        "match":{
            "title":"小米电视"
        }
    }
}
###and关系
某些情况下，我们需要更精确查找，我们希望这个关系变成and，可以这样做：
GET /heima/_search
{
    "query":{
        "match": {
          "title": {
            "query": "小米电视",
            "operator": "and"
          }
        }
    }
}
```

- or和and之间？

在 `or` 与 `and` 间二选一有点过于非黑即白。 如果用户给定的条件分词后有 5 个查询词项，想查找只包含其中 4 个词的文档，该如何处理？将 operator 操作符参数设置成 `and` 只会将此文档排除。

有时候这正是我们期望的，但在全文搜索的大多数应用场景下，我们既想包含那些可能相关的文档，同时又排除那些不太相关的。换句话说，我们想要处于中间某种结果。

`match` 查询支持 `minimum_should_match` 最小匹配参数， 这让我们可以指定必须匹配的词项数用来表示一个文档是否相关。我们可以将其设置为某个具体数字，更常用的做法是将其设置为一个`百分数`，因为我们无法控制用户搜索时输入的单词数量：

```json
GET /fun/_search
{
    "query":{
        "match":{
            "title":{
            	"query":"小米曲面电视",
            	"minimum_should_match": "75%"
            }
        }
    }
}
```

本例中，搜索语句可以分为3个词，如果使用and关系，需要同时满足3个词才会被搜索到。这里我们采用最小品牌数：75%，那么也就是说只要匹配到总词条数量的75%即可，这里3*75% 约等于2。所以只要包含2个词条就算满足条件了。

####多字段查询（multi_match）

```
GET /fun/_search
{
    "query":{
        "multi_match": {
            "query":    "小米",
            "fields":   [ "title", "subTitle" ]
        }
	}
}
本例中，我们会在title字段和subtitle字段中查询小米这个词
```

####词条匹配(term)

`term` 查询被用于精确值 匹配，这些精确值可能是数字、时间、布尔或者那些**未分词**的字符串

```
GET /fun/_search
{
    "query":{
        "term":{
            "price":2699.00
        }
    }
}
##terms 查询和 term 查询一样，但它允许你指定多值进行匹配。如果这个字段包含了指定值中的任何一个值，那么这##个文档满足条件：
GET /fun/_search
{
    "query":{
        "terms":{
            "price":[2699.00,2899.00,3899.00]
        }
    }
}
```

###结果过滤

#### 直接指定字段

默认情况下，elasticsearch在搜索的结果中，会把文档中保存在`_source`的所有字段都返回。

如果我们只想获取其中的部分字段，我们可以添加`_source`的过滤

```
GET /fun/_search
{
  "_source": ["title","price"],
  "query": {
    "term": {
      "price": 2699
    }
  }
}
```

####指定includes和excludes

我们也可以通过：

- includes：来指定想要显示的字段
- excludes：来指定不想要显示的字段

二者都是可选的。

示例：

```
##
GET /fun/_search
{
  "_source": {
    "includes":["title","price"] ###标识要查询的字段
  },
  "query": {
    "term": {
      "price": 2699
    }
  }
}
###效果一样
GET /fun/_search
{
  "_source": {
     "excludes": ["images"]  ###标识排除images这个字段
  },
  "query": {
    "term": {
      "price": 2699
    }
  }
}
```

### 高级查询

#### 布尔组合（bool)

`bool`把各种其它查询通过`must`（与）、`must_not`（非）、`should`（或）的方式进行组合

```
GET /fun/_search
{
    "query":{
        "bool":{
        	"must":     { "match": { "title": "大米" }},
        	"must_not": { "match": { "title":  "电视" }},
        	"should":   { "match": { "title": "手机" }}
        }
    }
}
```

####范围查询(range)

`range` 查询找出那些落在指定区间内的数字或者时间

```
GET /fun/_search
{
    "query":{
        "range": {
            "price": {
                "gte":  1000.0,
                "lt":   2800.00
            }
    	}
    }
}
```

`range`查询允许以下字符：

| 操作符 |   说明   |
| :----: | :------: |
|   gt   |   大于   |
|  gte   | 大于等于 |
|   lt   |   小于   |
|  lte   | 小于等于 |

####模糊查询(fuzzy)

我们新增一个商品：

```
POST /fun/goods/4
{
    "title":"apple手机",
    "images":"http://image.leyou.com/12479122.jpg",
    "price":6899.00
}
```

我们可以通过`fuzziness`来指定允许的编辑距离：

```
GET /heima/_search
{
  "query": {
    "fuzzy": {
        "title": {
            "value":"appla",
            "fuzziness":1
        }
    }
  }
}
```

#### 过滤

所有的查询都会影响到文档的评分及排名。如果我们需要在查询结果中进行过滤，并且不希望过滤条件影响评分，那么就不要把过滤条件作为查询条件来用。而是使用`filter`方式：

```
GET /fun/_search
{
    "query":{
        "bool":{
        	"must":{ "match": { "title": "小米手机" }},
        	"filter":{
                "range":{"price":{"gt":2000.00,"lt":3800.00}}
        	}
        }
    }
}
```

注意：`filter`中还可以再次进行`bool`组合条件过滤。

> **无查询条件，直接过滤**

如果一次查询只有过滤，没有查询条件，不希望进行评分，我们可以使用`constant_score`取代只有 filter 语句的 bool 查询。在性能上是完全相同的，但对于提高查询简洁性和清晰度有很大帮助。

```
GET /fun/_search
{
    "query":{
        "constant_score":   {
            "filter": {
            	 "range":{"price":{"gt":2000.00,"lt":3000.00}}
            }
        }
}
```

#### 排序

```
##单字段排序
GET /heima/_search
{
  "query": {
    "match": {
      "title": "小米手机"
    }
  },
  "sort": [
    {
      "price": {
        "order": "desc"
      }
    }
  ]
}
##多字段排序
GET /goods/_search
{
    "query":{
        "bool":{
        	"must":{ "match": { "title": "小米手机" }},
        	"filter":{
                "range":{"price":{"gt":2000,"lt":6000}}
        	}
        }
    },
    "sort": [
      { "price": { "order": "desc" }},
      { "_score": { "order": "desc" }}
    ]
}
```

### 聚合aggregations

聚合可以让我们极其方便的实现对数据的统计、分析。例如：

- 什么品牌的手机最受欢迎？
- 这些手机的平均价格、最高价格、最低价格？
- 这些手机每月的销售情况如何？

实现这些统计功能的比数据库的sql要方便的多，而且查询速度非常快，可以实现实时搜索效果。

**創建索引**

```
PUT /cars
{
  "settings": {
    "number_of_shards": 1,
    "number_of_replicas": 0
  },
  "mappings": {
    "transactions": {
      "properties": {
        "color": {
          "type": "keyword"
        },
        "make": {
          "type": "keyword"
        }
      }
    }
  }
}
POST /cars/transactions/_bulk
{ "index": {}}
{ "price" : 10000, "color" : "red", "make" : "honda", "sold" : "2014-10-28" }
{ "index": {}}
{ "price" : 20000, "color" : "red", "make" : "honda", "sold" : "2014-11-05" }
{ "index": {}}
{ "price" : 30000, "color" : "green", "make" : "ford", "sold" : "2014-05-18" }
{ "index": {}}
{ "price" : 15000, "color" : "blue", "make" : "toyota", "sold" : "2014-07-02" }
{ "index": {}}
{ "price" : 12000, "color" : "green", "make" : "toyota", "sold" : "2014-08-19" }
{ "index": {}}
{ "price" : 20000, "color" : "red", "make" : "honda", "sold" : "2014-11-05" }
{ "index": {}}
{ "price" : 80000, "color" : "red", "make" : "bmw", "sold" : "2014-01-01" }
{ "index": {}}
{ "price" : 25000, "color" : "blue", "make" : "ford", "sold" : "2014-02-12" }
```

#### 聚合

```
首先，我们按照 汽车的颜色color来划分桶
GET /cars/_search
{
    "size" : 0,
    "aggs" : { 
        "popular_colors" : { 
            "terms" : { 
              "field" : "color"
            }
        }
    }
}
```

- size： 查询条数，这里设置为0，因为我们不关心搜索到的数据，只关心聚合结果，提高效率
- aggs：声明这是一个聚合查询，是aggregations的缩写
  - popular_colors：给这次聚合起一个名字，任意。
    - terms：划分桶的方式，这里是根据词条划分
      - field：划分桶的字段

**查询结果**

```
{
  "took": 1,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": 8,
    "max_score": 0,
    "hits": []
  },
  "aggregations": {
    "popular_colors": {
      "doc_count_error_upper_bound": 0,
      "sum_other_doc_count": 0,
      "buckets": [
        {
          "key": "red",
          "doc_count": 4
        },
        {
          "key": "blue",
          "doc_count": 2
        },
        {
          "key": "green",
          "doc_count": 2
        }
      ]
    }
  }
}
```

- hits：查询结果为空，因为我们设置了size为0
- aggregations：聚合的结果
- popular_colors：我们定义的聚合名称
- buckets：查找到的桶，每个不同的color字段值都会形成一个桶
  - key：这个桶对应的color字段的值
  - doc_count：这个桶中的文档数量

通过聚合的结果我们发现，目前红色的小车比较畅销！

#### 桶内度量

前面的例子告诉我们每个桶里面的文档数量，这很有用。 但通常，我们的应用需要提供更复杂的文档度量。 例如，每种颜色汽车的平均价格是多少？

因此，我们需要告诉Elasticsearch`使用哪个字段`，`使用何种度量方式`进行运算，这些信息要嵌套在`桶`内，`度量`的运算会基于`桶`内的文档进行

现在，我们为刚刚的聚合结果添加 求价格平均值的度量：

```
GET /cars/_search
{
    "size" : 0,
    "aggs" : { 
        "popular_colors" : { 
            "terms" : { 
              "field" : "color"
            },
            "aggs":{
                "avg_price": { 
                   "avg": {
                      "field": "price" 
                   }
                }
            }
        }
    }
}
```

- aggs：我们在上一个aggs(popular_colors)中添加新的aggs。可见`度量`也是一个聚合
- avg_price：聚合的名称
- avg：度量的类型，这里是求平均值
- field：度量运算的字段

```
...
  "aggregations": {
    "popular_colors": {
      "doc_count_error_upper_bound": 0,
      "sum_other_doc_count": 0,
      "buckets": [
        {
          "key": "red",
          "doc_count": 4,
          "avg_price": {
            "value": 32500
          }
        },
        {
          "key": "blue",
          "doc_count": 2,
          "avg_price": {
            "value": 20000
          }
        },
        {
          "key": "green",
          "doc_count": 2,
          "avg_price": {
            "value": 21000
          }
        }
      ]
    }
  }
...
```

#### 桶内嵌套桶

刚刚的案例中，我们在桶内嵌套度量运算。事实上桶不仅可以嵌套运算， 还可以再嵌套其它桶。也就是说在每个分组中，再分更多组。

比如：我们想统计每种颜色的汽车中，分别属于哪个制造商，按照`make`字段再进行分桶

```
GET /cars/_search
{
    "size" : 0,
    "aggs" : { 
        "popular_colors" : { 
            "terms" : { 
              "field" : "color"
            },
            "aggs":{
                "avg_price": { 
                   "avg": {
                      "field": "price" 
                   }
                },
                "maker":{
                    "terms":{
                        "field":"make"
                    }
                }
            }
        }
    }
}
```

- 原来的color桶和avg计算我们不变
- maker：在嵌套的aggs下新添一个桶，叫做maker
- terms：桶的划分类型依然是词条
- filed：这里根据make字段进行划分

结果:

```
{"aggregations": {
    "popular_colors": {
      "doc_count_error_upper_bound": 0,
      "sum_other_doc_count": 0,
      "buckets": [
        {
          "key": "red",
          "doc_count": 4,
          "maker": {
            "doc_count_error_upper_bound": 0,
            "sum_other_doc_count": 0,
            "buckets": [
              {
                "key": "honda",
                "doc_count": 3
              },
              {
                "key": "bmw",
                "doc_count": 1
              }
            ]
          },
          "avg_price": {
            "value": 32500
          }
        },
        {
          "key": "blue",
          "doc_count": 2,
          "maker": {
            "doc_count_error_upper_bound": 0,
            "sum_other_doc_count": 0,
            "buckets": [
              {
                "key": "ford",
                "doc_count": 1
              },
              {
                "key": "toyota",
                "doc_count": 1
              }
            ]
          },
          "avg_price": {
            "value": 20000
          }
        },
        {
          "key": "green",
          "doc_count": 2,
          "maker": {
            "doc_count_error_upper_bound": 0,
            "sum_other_doc_count": 0,
            "buckets": [
              {
                "key": "ford",
                "doc_count": 1
              },
              {
                "key": "toyota",
                "doc_count": 1
              }
            ]
          },
          "avg_price": {
            "value": 21000
          }
        }
      ]
    }
  }
}
```

####阶梯分桶Histogram间隔

```
比如，我们对汽车的价格进行分组，指定间隔interval为5000：
GET /cars/_search
{
  "size":0,
  "aggs":{
    "price":{
      "histogram": {
        "field": "price",
        "interval": 5000
      }
    }
  }
}

##我们可以增加一个参数min_doc_count为1，来约束最少文档数量为1，这样文档数量为0的桶会被过滤


GET /cars/_search
{
  "size":0,
  "aggs":{
    "price":{
      "histogram": {
        "field": "price",
        "interval": 5000,
        "min_doc_count": 1
      }
    }
  }
}
```

##spring-boot整合

[1](https://blog.csdn.net/jiayoubing/article/details/105354424)

#### big-data-es

```shell
## 初始新增数据
http://localhost:8080/ems/elastic/init    
## 查询所有
http://localhost:8080/ems/elastic/all
```

### 整合springboot

[Sort使用](https://blog.csdn.net/wiselyman/article/details/106395745)

[构造查询条件](https://blog.csdn.net/qq_25320187/article/details/106439962)

```
http://127.0.0.1:8080/api/city

{
"id":1
"name":"上海",
"description":"上海你好啊",
"score":4
}
```

### 面试

#### 集群健康查看

```
## 路径
_cat/health
_cluster/health

Green：所有Primary和Replica均为active，集群健康
Yellow：至少一个Replica不可用，但是所有Primary均为active，数据仍然是可以保证完整性的。
Red：至少有一个Primary为不可用状态，数据不完整，集群不可用。
```

#### ES如何实现高可用生产环境均为一台机器一个节点

>(1)  ES在分配单个索引的分片时会将每个分片尽可能分配到更多的节点上。但是，实际情况取决于集群拥有的分片和索引的数量以及它们的大小，不一定总是能均匀地分布。
>
>(2)  ES不允许Primary和它的Replica放在同一个节点中，并且同一个节点不接受完全相同的两个Replica
>
>(3)  同一个节点允许多个索引的分片同时存在。

#### 容错机制

>(1)  啥叫容错？
>
>①  傻X的代码你能看懂，牛X的代码你也能看懂
>
>②  只能看懂自己的代码，容错性低
>
>③  PS：**各种情况（支持的情况越多，容错性越好）**下，都能保证work 正常运行
>
>④  换到咱们ES上就是，就是在局部出错异常的情况下，保证服务正常运行并且有自行恢复能力。
>
>(2)  ES-node
>
>①   
>
>1)   Master：主节点，每个集群都有且只有一个
>
>a.   尽量避免Master节点 node.data ＝ true
>
>2)   voting：投票节点
>
>a.   Node.voting_only = true（仅投票节点，即使配置了data.master = true，也不会参选, 但是仍然可以作为数据节点）。
>
>3)   coordinating：协调节点
>
>每一个节点都隐式的是一个协调节点，如果同时设置了data.master = false和data.data=false，那么此节点将成为仅协调节点。
>
>4)   Master-eligible node（候选节点）：  
>
>5)   Data node（数据节点）：
>
>6)   Ingest node：
>
>7)   Machine learning node（机器学习节点）：
>
>①  两个配置：node.master和node.data
>
>1)   node.master = true   node.data = true
>
>这是ES节点默认配置，既作为候选节点又作为数据节点，这样的节点一旦被选举为Master，压力是比较大的，通常来说Master节点应该只承担较为轻量级的任务，比如创建删除索引，分片均衡等。
>
>2)   node.master = true   node.data = false
>
>只作为候选节点，不作为数据节点，可参选Master节点，当选后成为真正的Master节点。
>
>3)   node.master = false   node.data = false
>
>既不当候选节点，也不作为数据节点，那就是仅协调节点，负责负载均衡
>
>4)   node.master=false      node.data=true
>
>不作为候选节点，但是作为数据节点，这样的节点主要负责数据存储和查询服务。
>
>(3)   图解容错机制
>
>**①**  **第一步：Master选举（假如宕机节点是Master）**
>
>1)    脑裂：可能会产生多个Master节点
>
>2)   解决：discovery.zen.minimum_master_nodes=N/2+1
>
>
>
>②  第二步：Replica容错，新的（或者原有）Master节点会将丢失的Primary对应的某个副本提升为Primary
>
>③  第三步：Master节点会尝试重启故障机
>
>④  第四步：数据同步，Master会将宕机期间丢失的数据同步到重启机器对应的分片上去

#### 如何提高ES分布式系统的可用性以及性能最大化

>（1）每台节点的Shard数量越少，每个shard分配的CPU、内存和IO资源越多，单个Shard的性能越好，当一台机器一个Shard时，单个Shard性能最好。
>
>**（2）稳定的Master节点对于群集健康非常重要！**理论上讲，应该尽可能的减轻Master节点的压力，分片数量越多，Master节点维护管理shard的任务越重，并且节点可能就要承担更多的数据转发任务，可增加“仅协调”节点来缓解Master节点和Data节点的压力，但是在集群中添加过多的仅协调节点会增加整个集群的负担，因为选择的主节点必须等待每个节点的集群状态更新确认。
>
>（3）反过来说，如果相同资源分配相同的前提下，shard数量越少，单个shard的体积越大，查询性能越低，速度越慢，这个取舍应根据实际集群状况和结合应用场景等因素综合考虑。
>
>（4）数据节点和Master节点一定要分开，集群规模越大，这样做的意义也就越大。
>
>（5）数据节点处理与数据相关的操作，例如CRUD，搜索和聚合。这些操作是I / O，内存和CPU密集型的，所以他们需要更高配置的服务器以及更高的带宽，并且集群的性能冗余非常重要。
>
>（6）由于仅投票节不参与Master竞选，所以和真正的Master节点相比，它需要的内存和CPU较少。但是，所有候选节点以及仅投票节点都可能是数据节点，所以他们都需要快速稳定低延迟的网络。
>
>（7）高可用性（HA）群集至少需要三个主节点，其中**至少两个不是仅投票**节点。即使其中一个节点发生故障，这样的群集也将能够选举一个主节点。生产环境最好设置3台仅Master候选节点（node.master = true  node.data = true）
>
> （8）为确保群集仍然可用，集群**不能****同时停止投票配置中的一半或更多节点**。只要有一半以上的投票节点可用，群集仍可以正常工作。这意味着，如果存在三个或四个主节点合格的节点，则群集可以容忍其中一个节点不可用。如果有两个或更少的主机资格节点，则它们必须都保持可用
>
>