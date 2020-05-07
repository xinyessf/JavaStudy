### List

[ArrayList,LinkedList,Vector](https://blog.csdn.net/kuangsonghan/article/details/79861170)

### List去重的方法

```
 private String name;
    private Integer age;
    private String bithday;
    public Person() {
    }
    public Person(String name, int age, String bithday) {
        this.name = name;
        this.age = age;
        this.bithday = bithday;
    }
//省略get set方法
public static void main(String[] args) throws Exception {
        Person p = new Person("张三", 20, "1999-12-22");
        Person p2 = new Person("李四", 20, "1999-12-22");
        Person p3 = new Person("张三", 22, "1999-12-22");
        List<Person> data = new ArrayList<Person>();
        //方式一去重
        data.add(p);
        data.add(p2);
        data.add(p3);
        List<Person> distinctClass = data.stream().collect(Collectors.collectingAndThen
                (Collectors.toCollection(() -> new TreeSet<>(Comparator.comparing(o ->
                        o.getName() + ";" + o.getAge()))), ArrayList::new));

        System.out.println(distinctClass.size());
        //==方式二重写hash和equals,不建议
        Set set = new HashSet();
        set.addAll(data);
        System.out.println(set.size());
        //方式三,冒泡法for--for
        //方式四 Comparator
        Set<Person> set2 = new TreeSet<Person>(new Comparator<Person>() {
            @Override
            public int compare(Person a, Person b) {
                //完善
                return (a.getName().equals(b.getName())&&a.getAge().equals(b.getAge()))?0:-1;
            }
        });
        set2.addAll(data);
        System.out.println(set2.size());
}
```



### 线程安全的List

[CopyOnWriteArrayList,synchronizedList](https://www.jianshu.com/p/6455a4e66e14)

[线程安全的List](https://blog.csdn.net/p_programmer/article/details/86027076)

### Arraylist源码分析

```

```

### Linkedlist源码

```

```

### Vector源码

```

```

## 并发包List

### ConcurrentLinkedDeque

>ConcurrentLinkedQueue : 是一个适用于高并发场景下的队列，通过无锁的方式，实现
> 了高并发状态下的高性能，通常ConcurrentLinkedQueue性能好于BlockingQueue.它
> 是一个基于链接节点的无界线程安全队列。该队列的元素遵循先进先出的原则。头是最先
> 加入的，尾是最近加入的，该队列不允许null元素。
> ConcurrentLinkedQueue重要方法:
> add 和offer() 都是加入元素的方法(在ConcurrentLinkedQueue中这俩个方法没有任何区别)
> poll() 和peek() 都是取头元素节点，区别在于前者会删除元素，后者不会。

```java
//先进先出	
ConcurrentLinkedDeque q = new ConcurrentLinkedDeque();
	q.offer("哈比");
	q.offer("码云");
	q.offer("蚂蚁课堂");
	q.offer("张杰");
	q.offer("艾姐");
	//从头获取元素,删除该元素
	System.out.println(q.poll());
	//从头获取元素,不刪除该元素
	System.out.println(q.peek());
	//获取总长度
	System.out.println(q.size());
```

### BlockingQueue

>阻塞队列（BlockingQueue）是一个支持两个附加操作的队列。这两个附加的操作是：
>
>在队列为空时，获取元素的线程会等待队列变为非空。
> 当队列满时，存储元素的线程会等待队列可用。 
>
>阻塞队列常用于生产者和消费者的场景，生产者是往队列里添加元素的线程，消费者是从队列里拿元素的线程。阻塞队列就是生产者存放元素的容器，而消费者也只从容器里拿元素。

```
先进先出（FIFO）：先插入的队列的元素也最先出队列，类似于排队的功能。从某种程度上来说这种队列也体现了一种公平性。
后进先出（LIFO）：后插入队列的元素最先出队列，这种队列优先处理最近发生的事件。
```

#### ArrayBlockingQueue

>ArrayBlockingQueue是一个有边界的阻塞队列，它的内部实现是一个数组。有边界的意思是它的容量是有限的，我们必须在其初始化的时候指定它的容量大小，容量大小一旦指定就不可改变。
>
>ArrayBlockingQueue是以先进先出的方式存储数据，最新插入的对象是尾部，最新移出的对象是头部。下面

```java
	ArrayBlockingQueue<String> arrays = new ArrayBlockingQueue<String>(3);
	arrays.add("李四");
	 arrays.add("张军");
	arrays.add("张军");
	// 添加阻塞队列
	arrays.offer("张三", 1, TimeUnit.SECONDS);
```

#### LinkedBlockingQueue

>LinkedBlockingQueue阻塞队列大小的配置是可选的，如果我们初始化时指定一个大小，它就是有边界的，如果不指定，它就是无边界的。说是无边界，其实是采用了默认大小为Integer.MAX_VALUE的容量 。它的内部实现是一个链表。
>
>和ArrayBlockingQueue一样，LinkedBlockingQueue 也是以先进先出的方式存储数据，最新插入的对象是尾部，最新移出的对象是头部。下面是一个初始化和使LinkedBlockingQueue的例子：

```java
LinkedBlockingQueue linkedBlockingQueue = new LinkedBlockingQueue(3);
linkedBlockingQueue.add("张三");
linkedBlockingQueue.add("李四");
linkedBlockingQueue.add("李四");
System.out.println(linkedBlockingQueue.size());
```

#### PriorityBlockingQueue

>PriorityBlockingQueue是一个没有边界的队列，它的排序规则和 java.util.PriorityQueue一样。需要注 
>意，PriorityBlockingQueue中允许插入null对象。
>所有插入PriorityBlockingQueue的对象必须实现 java.lang.Comparable接口，队列优先级的排序规则就 
>是按照我们对这个接口的实现来定义的。
>另外，我们可以从PriorityBlockingQueue获得一个迭代器Iterator，但这个迭代器并不保证按照优先级顺 
>序进行迭代。

#### SynchronousQueue

> **SynchronousQueue**队列内部仅允许容纳一个元素。当一个线程插入一个元素后会被阻塞，除非这个元素被另一个线程消费。