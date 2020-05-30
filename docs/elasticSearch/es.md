### es了解

>**elasticsearch简写es，es是一个高扩展、开源的全文检索和分析引擎，它可以准实时地快速存储、搜索、分析海量的数据。**
>
> 全文检索是指计算机索引程序通过扫描文章中的每一个词，对每一个词建立一个索引，指明该词在文章中出现的次数和位置，当用户查询时，检索程序就根据事先建立的索引进行查找，并将查找的结果反馈给用户的检索方式。这个过程类似于通过字典中的检索字表查字的过程。全文搜索搜索引擎数据库中的数据。 

### es的应用场景

[大白话告诉你es干嘛用的](https://blog.csdn.net/paicMis/article/details/82535018?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.nonecase)

>- 一个线上商城系统，用户需要搜索商城上的商品。
>  在这里你可以用es存储所有的商品信息和库存信息，用户只需要输入”空调”就可以搜索到他需要搜索到的商品。
>- 一个运行的系统需要收集日志，用这些日志来分析、挖掘从而获取系统业务未来的趋势。
>  你可以用logstash（elk中的一个产品，elasticsearch/logstash/kibana）收集、转换你的日志，并将他们存储到es中。一旦数据到达es中，就你可以在里面搜索、运行聚合函数等操作来挖掘任何你感兴趣的信息。
>- 如果你有想基于大量数据（数百万甚至数十亿的数据）快速调查、分析并且要将分析结果可视化的需求。
>  你可以用es来存储你的数据，用kibana构建自定义的可视化图形、报表，为业务决策提供科学的数据依据。

```
直白点讲，es是一个企业级海量数据的搜索引擎，可以理解为是一个企业级的百度搜索，除了搜索之外，es还可以快速的实现聚合运算。
```

### es环境搭建

```

```



####es 的分布式架构原理能说一下么（es 是如何实现分布式的啊）？

[原理](https://blog.csdn.net/qq_22186183/article/details/102859785)

####es 写入数据的工作原理是什么啊？es 查询数据的工作原理是什么啊？底层的 lucene 介绍一下呗？倒排索引了解吗？

[es](https://www.jianshu.com/p/d6fd7e8cf220)

####es 在数据量很大的情况下（数十亿级别）如何提高查询效率啊？

[提升查询效率](https://www.jianshu.com/p/fa510352ce1a)

####es 生产集群的部署架构是什么？每个索引的数据量大概有多少？每个索引大概有多少个分片？

[生产部署](https://blog.csdn.net/weixin_39388918/article/details/97624125)

