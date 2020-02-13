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