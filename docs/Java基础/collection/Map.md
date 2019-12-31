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

