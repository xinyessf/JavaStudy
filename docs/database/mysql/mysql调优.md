# SQL

### mysql调优的方式

>**慢查询日志**，**EXPLAIN 分析查询**，**profiling分析**以及**show命令查询系统状态及系统变量**，通过定位分析性能的瓶颈，才能更好的优化数据库系统的性能。

```mysql
show status -- 显示状态信息（扩展show status like ‘XXX’）

show variables -- 显示系统变量（扩展show variables like ‘XXX’）

show innodb status -- 显示InnoDB存储引擎的状态

show processlist -- 查看当前SQL执行，包括执行状态、是否锁表等\

select * from information_schema.processlist;

kill id -- 杀死正在执行的sql线程

mysqladmin variables -u username -p password -- 显示系统变量

mysqladmin extended-status -u username -p password -- 显示状态信息
```

### 执行计划

[explain执行计划](https://www.cnblogs.com/xiaoboluo768/p/5400990.html)

>- EXPLAIN不考虑触发器、存储过程或用户自定义函数对查询的影响
>- EXPLAIN不考虑缓存
>- EXPLAIN只能分析执行计划，不能显示存储引擎在执行查询过程中进行的操作
>- 部分统计信息是估算的，并非精确值

```
作用:查看涉及多少行、使用哪些索引、运行时间
-- 可以看到上边这些列
| id | select_type | table | type | possible_keys | key | key_len | ref | rows | Extra
-- id: 查询的唯一标识
id列数字越大越先执行，如果说数字一样大，那么就从上往下依次执行，id列为null的就表是这是一个结果集，不需要使用它来进行查询。
-- select_type 查询的类型
（1）分别用来表示查询的类型，主要是用于区别普通查询、联合查询、子查询等的复杂查询。
常用的值：
   SIMPLE:简单SELECT(不使用UNION或子查询)
   PRIMARY:最外面的SELECT
   UNION:UNION中的第二个或后面的SELECT语句
   DEPENDENT UNION:UNION中的第二个或后面的SELECT语句,取决于外面的查询
   UNION RESULT:UNION 的结果
   SUBQUERY:子查询中的第一个SELECT
   DEPENDENT SUBQUERY:子查询中的第一个SELECT,取决于外面的查询
   DERIVED:导出表的SELECT(FROM子句的子查询)
-- table 查询的表, 可能是数据库中的表/视图，也可能是 FROM 中的子查询
显示这一行的数据是关于哪张表的
-- type 搜索数据的方法
（1）依次从好到差：system > const > eq_ref > ref > fulltext > ref_or_null > unique_subquery > index_subquery > range > index_merge > index > ALL
（2）除了all之外，其他的type都可以使用到索引，除了index_merge之外，其他的type只可以用到一个索引
（3）最常用的从好到差依次是：system > const > eq_ref > ref > range > index > all
（4）一般来说，得保证查询至少达到range级别，最好能达到ref。
（5）system ：表只有一行记录（等于系统表），这是const类型的特例，平时不会出现，这个也可以忽略不计
（6）const ：表示通过索引一次就找到了，const用于比较primary key 或者unique索引。因为只匹配一行数据，所以很快。如将主键置于where列表中，MySQL就能将该查询转换为一个常量。 
（7）eq_ref ：唯一性索引扫描，对于每个索引键，表中只有一条记录与之匹配。常见于主键或唯一索引扫描
（8）ref ：非唯一性索引扫描，返回匹配某个单独值的所有行，本质上也是一种索引访问，它返回所有匹配某个单独值的行，然而，它可能会找到多个符合条件的行，所以他应该属于查找和扫描的混合体。
（8）range ：只检索给定范围的行，使用一个索引来选择行，key列显示使用了哪个索引，一般就是在你的where语句中出现between、< 、>、in等的查询，这种范围扫描索引比全表扫描要好，因为它只需要开始于索引的某一点，而结束于另一点，不用扫描全部索引。
（9）index ：Full Index Scan，Index与All区别为index类型只遍历索引树。这通常比ALL快，因为索引文件通常比数据文件小。（也就是说虽然all和Index都是读全表，但index是从索引中读取的，而all是从硬盘读取的）
（10）all ：Full Table Scan 将遍历全表以找到匹配的行 
-- possible_keys 可能使用的索引
显示可能应用在这张表中的索引。如果为空，没有可能的索引。可以为相关的域从WHERE语句中选择一个合适的语句
-- key 最终决定要使用的key
实际使用的索引。如果为NULL，则没有使用索引。很少的情况下，MYSQL会选择优化不足的索引。这种情况下，可以在SELECT语句中使用USE INDEX（indexname）来强制使用一个索引或者用IGNORE INDEX（indexname）来强制MYSQL忽略索引
-- key_len 查询索引使用的字节数。通常越少越好
使用的索引的长度。在不损失精确性的情况下，长度越短越好
-- ref 查询的列或常量
显示索引的哪一列被使用了，如果可能的话，是一个常数
-- rows  需要扫描的行数，估计值。通常越少越好
MYSQL认为必须检查的用来返回请求数据的行数
-- Extra 额外的信息
关于MYSQL如何解析查询的额外信息。但这里可以看到的坏的例子是Using temporary和Using filesort，意思MYSQL根本不能使用索引，结果是检索会很慢
using filesort: 查询时执行了排序操作而无法使用索引排序。虽然名称为'file'但操作可能是在内存中执行的，取决是否有足够的内存进行排序。
应尽量避免这种filesort出现。
using temporary: 使用临时表存储中间结果，常见于ORDER BY和GROUP BY语句中。临时表可能在内存中也可能在硬盘中，应尽量避免这种操作出现。
using index: 索引中包含查询的所有列(覆盖索引)不需要查询数据表。可以加快查询速度。
using index condition: 索引条件推送(MySQL 5.6 新特性)，服务器层将不能直接使用索引的查询条件推送给存储引擎，从而避免在服务器层进行过滤。
using where: 服务器层对存储引擎返回的数据进行了过滤
distinct: 优化distinct操作，查询到匹配的数据后停止继续搜索
```

### 如何查看慢

[慢查询学习](https://blog.csdn.net/qq_33862644/article/details/79821134)

>**慢查询日志**：记录所有执行时间超过long_query_time秒的所有查询或者不使用索引的查询
>
>注意：平常不要开，只有分析的时候才开（因为开启后sql语句需要往日志里写，也要耗时间）。

```mysql
show global status like ‘%slow%’;
-- 慢查询日志开启,路径
-- 首先查看慢查询日志是否开启
-- 打开慢查询,临时打开
set global slow_query_log=on; 
set long_query_time=0.2;  
-- 查看慢查询设置时长
show variables like '%long%'
-- 不使用索引的慢查询
show variables like '%quer%';
-- 查看慢查询的日志
show variables like '%slow_query_log_file%';
-- 测试
select sleep(2);
explain select sleep(2);



```

### mysql日志

>**MySQL中的日志包括**：错误日志、二进制日志、通用查询日志、慢查询日志等等。

```mysql
-- 当前数据库与版本号相关的东西
show variables like '%version%';

-- 前的通用日志查询是否开启，如果general_log的值为ON则为开启，为OFF则为关闭（默认情况下是关闭的）
show variables like '%general%';
-- 查看当前日志输出的格式
show variables like '%log_output%';
```

* 通用日志查询设置

```mysql
-- 开启通用日志查询： 
set global general_log=on;
-- 关闭通用日志查询： 
set globalgeneral_log=off;

-- 设置通用日志输出为表方式： 
set globallog_output=’TABLE’;

-- 设置通用日志输出为文件方式： 
set globallog_output=’FILE’;

-- 设置通用日志输出为表和文件方式：
set global log_output=’FILE,TABLE’;
```

### sql写法优化

[sql语句优化](https://blog.csdn.net/qq_38789941/article/details/83744271)

