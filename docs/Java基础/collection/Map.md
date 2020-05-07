### Map

| 类型            | 数据结构          | 特点描述                        |
| ------------- | ------------- | --------------------------- |
| HashMap       | 散列表(拉链法)      | 最常用，无序，线程不安全                |
| Hashtable     | 散列表(拉链法)      | 无序，线程安全                     |
| LinkedHashMap | 双向链表+散列表(拉链法) | 有序(插入顺)，线程不安全               |
| WeakHashMap   | 散列表(拉链法)      | 无序，线程不安全，弱引用键，保存对象被回收时，自动删除 |
| TreeMap       | 红黑树           | 有序(自动排序)，线程不安全              |

[Map总结](https://blog.csdn.net/qq_41030039/article/details/90515349)

[https://blog.csdn.net/jichuang123/article/details/88143118](https://blog.csdn.net/jichuang123/article/details/88143118)

### 线程安全的map

[几个线程安全的Map](https://blog.csdn.net/weixin_42812598/article/details/90708472)

###并发线程map

####currentHashMap

[说明](https://www.cnblogs.com/shineipangtuo/p/12395489.html)

>ConcurrentMap接口下有俩个重要的实现 :
> ConcurrentHashMap
> ConcurrentskipListMap (支持并发排序功能。弥补ConcurrentHas hMa p)
> **ConcurrentHashMap****内部使用段(Segment)来表示这些不同的部分，每个段其实就是一个
> 小的HashTable,它们有自己的锁。只要多个修改操作发生在不同的段上，它们就可以并
> 发进行。把一个整体分成了16个段(Segment.也就是最高支持16个线程的并发修改操作。
> 这也是在重线程场景时减小锁的粒度从而降低锁竞争的一种方案。并且代码中大多共享变
> 量使用volatile关键字声明，目的是第一时间获取修改的内容，性能非常好。**

#### synchronizedMap

```

```





