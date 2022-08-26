## JVM

###文章

[jdk8之前用法](https://blog.csdn.net/qq_41701956/article/details/81664921)

[jdk8和jdk7比较](https://blog.csdn.net/zhou920786312/article/details/97984663)

[jdk8内存有啥变化](https://blog.csdn.net/chlu113/article/details/51890469)

![一篇足够](https://img-blog.csdn.net/20180808112156511?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2h1eXV5YW5nNjY4OA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

##Java内存区域

**jdk8之前**

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91c2VyLWdvbGQtY2RuLnhpdHUuaW8vMjAxNy85LzQvZGQzYjE1YjNkODgyNmZhZWFlMjA2Mzk3NmZiOTkyMTM_aW1hZ2VWaWV3Mi8wL3cvMTI4MC9oLzk2MC9mb3JtYXQvd2VicC9pZ25vcmUtZXJyb3IvMQ)

**jdk8**

![img](https://pic4.zhimg.com/v2-5f76b14ab44f1d2525456d49c08bfd63_r.jpg)

**每个区域存储的内容**

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91c2VyLWdvbGQtY2RuLnhpdHUuaW8vMjAxNy85LzQvZGE3N2Q5MDE0Njc4NmMwY2IzZTE3MGI5YzkzNzZhZTQ_aW1hZ2VWaWV3Mi8wL3cvMTI4MC9oLzk2MC9mb3JtYXQvd2VicC9pZ25vcmUtZXJyb3IvMQ)

### 程序计数器

```
内存空间小，线程私有。字节码解释器工作是就是通过改变这个计数器的值来选取下一条需要执行指令的字节码指令，分支、循环、跳转、异常处理、线程恢复等基础功能都需要依赖计数器完成
```

### Java虚拟机栈

```
线程私有，生命周期和线程一致。描述的是 Java 方法执行的内存模型：每个方法在执行时都会床创建一个栈帧(Stack Frame)用于存储局部变量表、操作数栈、动态链接、方法出口等信息。每一个方法从调用直至执行结束，就对应着一个栈帧从虚拟机栈中入栈到出栈的过程。
```

### 本地方法栈

```
区别于 Java 虚拟机栈的是，Java 虚拟机栈为虚拟机执行 Java 方法(也就是字节码)服务，而本地方法栈则为虚拟机使用到的 Native 方法服务。也会有 StackOverflowError 和 OutOfMemoryError 异常。
```

### Java堆

```
对于绝大多数应用来说，这块区域是 JVM 所管理的内存中最大的一块。线程共享，主要是存放对象实例和数组。内部会划分出多个线程私有的分配缓冲区(Thread Local Allocation Buffer, TLAB)。可以位于物理上不连续的空间，但是逻辑上要连续。
```

### 方法区

```
属于共享内存区域，存储已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据。
```

### 运行时常量池

```
属于方法区一部分，用于存放编译期生成的各种字面量和符号引用。编译器和运行期(String 的 intern() )都可以将常量放入池中。内存有限，无法申请时抛出 OutOfMemoryError。
```

### 直接内存

```
服务器
```

## 垃圾收集器和内存分配策略

>程序计数器、虚拟机栈、本地方法栈 3 个区域随线程生灭(因为是线程私有)，栈中的栈帧随着方法的进入和退出而有条不紊地执行着出栈和入栈操作。而 Java 堆和方法区则不一样，一个接口中的多个实现类需要的内存可能不一样，一个方法中的多个分支需要的内存也可能不一样，我们只有在程序处于运行期才知道那些对象会创建，这部分内存的分配和回收都是动态的，垃圾回收期所关注的就是这部分内存。
>

### 如何判断对象已死

####引用计数法

```
给对象添加一个引用计数器。但是难以解决循环引用问题。
```

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91c2VyLWdvbGQtY2RuLnhpdHUuaW8vMjAxNy85LzQvNGMyODlhMjI0Y2I0OTQ0ZTQ5OWZiNWJmZDMzZTU5MmY_aW1hZ2VWaWV3Mi8wL3cvMTI4MC9oLzk2MC9mb3JtYXQvd2VicC9pZ25vcmUtZXJyb3IvMQ)

从图中可以看出，如果不下小心直接把 Obj1-reference 和 Obj2-reference 置 null。则在 Java 堆当中的两块内存依然保持着互相引用无法回收

####可达性算法

通过一系列的 ‘GC Roots’ 的对象作为起始点，从这些节点出发所走过的路径称为引用链。当一个对象到 GC Roots 没有任何引用链相连的时候说明对象不可用。

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91c2VyLWdvbGQtY2RuLnhpdHUuaW8vMjAxNy85LzQvNThiZmFjMTVjYTZkMzA3NmRlZjUxNzRlZDVjYTVhOTk_aW1hZ2VWaWV3Mi8wL3cvMTI4MC9oLzk2MC9mb3JtYXQvd2VicC9pZ25vcmUtZXJyb3IvMQ)

### 引用

[引用](https://www.cnblogs.com/rgever/p/8902210.html)

[引用](https://blog.csdn.net/gdhgr/article/details/80450921)

**强引用**

```
我们平日里面的用到的new了一个对象就是强引用，例如 Object obj = new Object();
当JVM的内存空间不足时，宁愿抛出OutOfMemoryError使得程序异常终止也不愿意回收具有强引用的存活着的对象！记住是存活着，不可能是你new一个对象就永远不会被GC回收。
当一个普通对象没有其他引用关系，只要超过了引用的作用域或者显示的将引用赋值为null时，你的对象就表明不是存活着，这样就会可以被GC回收了。当然回收的时间是不一定的具体得看GC回收策略。
当一个普通对象没有其他引用关系，只要超过了引用的作用域或者显示的将引用赋值为null时，你的对象就表明不是存活着，这样就会可以被GC回收了。当然回收的时间是不一定的具体得看GC回收策略。
Object obj=new Object(); 
JuGyeong
```

**软引用**

```java
软引用的生命周期比强引用短一些。软引用是通过SoftReference类实现的。
这样就是一个简单的软引用使用方法。通过get()方法获取对象。当JVM认为内存空间不足时，就回去试图回收软引用指向的对象，也就是说在JVM抛出OutOfMemoryError之前，会去清理软引用对象。软引用可以与引用队列(ReferenceQueue)联合使用。
//如下
Object obj=new Object();
WeakReference<Object> queue=new WeakReference<Object>(obj);
SoftReference soft=new SoftReference<>(obj);
obj=null;//去除软引用
```

**弱引用**

```java
弱引用是通过WeakReference类实现的，它的生命周期比软引用还要短，也是通过get()方法获取对象。
//如下
Object obj=new Object();
WeakReference<Object> weak=new WeakReference<Object>(obj);
obj=null;//去除弱引用
```

**虚引用**(幻像引用)

```java
是通过PhantomReference类实现的。任何时候可能被GC回收，就像没有引用一样。
Object obj=new Object();
ReferenceQueue<Object> queue=new ReferenceQueue<>();
PhantomReference<Object> phantomReference = new PhantomReference<>(obj, queue);
obj=null;//去除虚引用
```

## 垃圾收集算法

[收集算法](https://www.cnblogs.com/aspirant/p/8662690.html)

[收集算法](https://www.cnblogs.com/dengpengbo/p/10454484.html)

### 引用计数法

```
引用计数法实现简单，效率较高，在大部分情况下是一个不错的算法。其原理是：给对象添加一个引用计数器，每当有一个地方引用该对象时，计数器加1，当引用失效时，计数器减1，当计数器值为0时表示该对象不再被使用。需要注意的是：引用计数法很难解决对象之间相互循环引用的问题，主流Java虚拟机没有选用引用计数法来管理内存。
```

### 标记-清除算法

```
标记-清除算法采用从根集合（GC Roots）进行扫描，对存活的对象进行标记，标记完毕后，再扫描整个空间中未被标记的对象，进行回收，如下图所示。标记-清除算法不需要进行对象的移动，只需对不存活的对象进行处理，在存活对象比较多的情况下极为高效，但由于标记-清除算法直接回收不存活的对象，因此会造成内存碎片。
```

### 复制算法

```
复制算法的提出是为了克服句柄的开销和解决内存碎片的问题。它开始时把堆分成 一个对象 面和多个空闲面， 程序从对象面为对象分配空间，当对象满了，基于copying算法的垃圾 收集就从根集合（GC Roots）中扫描活动对象，并将每个 活动对象复制到空闲面(使得活动对象所占的内存之间没有空闲洞)，这样空闲面变成了对象面，原来的对象面变成了空闲面，程序会在新的对象面中分配内存。

这种算法虽然实现简单，运行高效且不容易产生内存碎片，但是却对内存空间的使用做出了高昂的代价，因为能够使用的内存缩减到原来的一半。
```

### 标记-整理算法

```
该算法标记阶段和Mark-Sweep一样，但是在完成标记之后，它不是直接清理可回收对象，而是将存活对象都向一端移动然后清理掉端边界以外的内存
标记-整理算法是在标记-清除算法的基础上，又进行了对象的移动;
```

### 分代收集算法

```
目前大部分垃圾收集器对于新生代都采取Copying算法，因为新生代中每次垃圾回收都要回收大部分对象，也就是说需要复制的操作次数较少，但是实际中并不是按照1：1的比例来划分新生代的空间的，一般来说是将新生代划分为一块较大的Eden空间和两块较小的Survivor空间（一般为8:1:1），每次使用Eden空间和其中的一块Survivor空间，当进行回收时，将Eden和Survivor中还存活的对象复制到另一块Survivor空间中，然后清理掉Eden和刚才使用过的Survivor空间。

而由于老年代的特点是每次回收都只回收少量对象，一般使用的是Mark-Compact算法。
```

**年轻代（Young Generation）的回收算法 (回收主要以Copying为主)**

> a) 所有新生成的对象首先都是放在年轻代的。年轻代的目标就是尽可能快速的收集掉那些生命周期短的对象。
>
> b) 新生代内存按照8:1:1的比例分为一个eden区和两个survivor(survivor0,survivor1)区。一个Eden区，两个 Survivor区(一般而言)。大部分对象在Eden区中生成。回收时先将eden区存活对象复制到一个survivor0区，然后清空eden区，当这个survivor0区也存放满了时，则将eden区和survivor0区存活对象复制到另一个survivor1区，然后清空eden和这个survivor0区，此时survivor0区是空的，然后将survivor0区和survivor1区交换，**即保持survivor1区为空(美团面试，问的太细，为啥保持survivor1为空，答案：为了让eden和survivor0 交换存活对象)**， 如此往复。当Eden没有足够空间的时候就会 触发jvm发起一次Minor GC
>
> c) 当survivor1区不足以存放 eden和survivor0的存活对象时，就将存活对象直接存放到老年代。若是老年代也满了就会触发一次Full GC(Major GC)，也就是新生代、老年代都进行回收。
>
> d) 新生代发生的GC也叫做Minor GC，MinorGC发生频率比较高(不一定等Eden区满了才触发)。

**年老代（Old Generation）的回收算法（回收主要以Mark-Compact为主）**

>a) 在年轻代中经历了N次垃圾回收后仍然存活的对象，就会被放到年老代中。因此，可以认为年老代中存放的都是一些生命周期较长的对象。
>
>b) 内存比新生代也大很多(大概比例是1:2)，当老年代内存满时触发Major GC即Full GC，Full GC发生频率比较低，老年代对象存活时间比较长，存活率标记高。

**持久代（Permanent Generation）(也就是方法区)的回收算法**

用于存放静态文件，如Java类、方法等。持久代对垃圾回收没有显著影响，但是有些应用可能动态生成或者调用一些class，例如Hibernate 等，在这种时候需要设置一个比较大的持久代空间来存放这些运行过程中新增的类。持久代也称方法区，具体的回收

 方法区存储内容是否需要回收的判断可就不一样咯。方法区主要回收的内容有：废弃常量和无用的类。对于废弃常量也可通过引用的可达性来判断，但是对于无用的类则需要同时满足：

- 该类所有的实例都已经被回收，也就是Java堆中不存在该类的任何实例；
- 加载该类的`ClassLoader`已经被回收；
- 该类对应的`java.lang.Class`对象没有在任何地方被引用，**无法在任何地方通过反射访问该类的方法。**

**新生代和老年代区别**

所谓的新生代和老年代是针对于分代收集算法来定义的，新生代又分为Eden和Survivor两个区。加上老年代就这三个区。数据会首先分配到Eden区 当中（当然也有特殊情况，如果是大对象那么会直接放入到老年代（大对象是指需要大量连续内存空间的java对象）。），当Eden没有足够空间的时候就会 触发jvm发起一次Minor GC。如果对象经过一次Minor GC还存活，并且又能被Survivor空间接受，那么将被移动到Survivor空 间当中。并将其年龄设为1，对象在Survivor每熬过一次Minor GC，年龄就加1，当年龄达到一定的程度（默认为15）时，就会被晋升到老年代 中了，当然晋升老年代的年龄是可以设置的。如果老年代满了就执行：Full GC 因为不经常执行，因此采用了 Mark-Compact算法清理

其实新生代和老年代就是针对于对象做分区存储，更便于回收等等

## 垃圾回收器

[CMS垃圾回收机制](https://www.cnblogs.com/aspirant/p/8663911.html)

[CMS和G1收集器](https://www.cnblogs.com/aspirant/p/8663911.html)

[G1垃圾收集器](https://www.cnblogs.com/aspirant/p/8663872.html)

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91c2VyLWdvbGQtY2RuLnhpdHUuaW8vMjAxNy85LzQvMTVmYjc1NDc2MmZmNWRmM2Y3ZjYzZTVjMjZkNGQzYWU_aW1hZ2VWaWV3Mi8wL3cvMTI4MC9oLzk2MC9mb3JtYXQvd2VicC9pZ25vcmUtZXJyb3IvMQ)

### Serial 收集器(复制算法)

```
新生代单线程收集器，标记和清理都是单线程，优点是简单高效。是client级别默认的GC方式，可以通过-XX:+UseSerialGC来强制指定。
```

### Serial Old收集器(标记-整理算法)

```
Serial Old收集器(标记-整理算法)
老年代单线程收集器，Serial收集器的老年代版本。
```

### ParNew 收集器(停止-复制算法)　

```
指多条垃圾收集线程并行工作，此时用户线程处于等待状态
新生代收集器，可以认为是Serial收集器的多线程版本,在多核CPU环境下有着比Serial更好的表现。
```

### Parallel Scavenge 收集器

```
并行收集器，追求高吞吐量，高效利用CPU。吞吐量一般为99%， 吞吐量= 用户线程时间/(用户线程时间+GC线程时间)。适合后台应用等对交互相应要求不高的场景。是server级别默认采用的GC方式，可用-XX:+UseParallelGC来强制指定，用-XX:ParallelGCThreads=4来指定线程数。
```

### Parallel Old 收集器

```
Parallel Old 是 Parallel Scavenge 收集器的老年代版本。多线程，使用 标记 —— 整理
```

### CMS收集器(标记 —清除 )

```
CMS (Concurrent Mark Sweep) 收集器是一种以获取最短回收停顿时间为目标的收集器。基于 标记 —— 清除 算法实现。
高并发、低停顿，追求最短GC回收停顿时间，cpu占用比较高，响应时间快，停顿时间短，多核cpu 追求高响应时间的选择。
```

### G1 收集器

```
面向服务端的垃圾回收器。
```

##GC什么时候触发

  由于对象进行了分代处理，因此垃圾回收区域、时间也不一样。GC有两种类型：Scavenge GC和Full GC。

###Scavenge GC

一般情况下，当新对象生成，并且在Eden申请空间失败时，就会触发Scavenge GC，对Eden区域进行GC，清除非存活对象，并且把尚且存活的对象移动到Survivor区。然后整理Survivor的两个区。这种方式的GC是对年轻代的Eden区进行，不会影响到年老代。因为大部分对象都是从Eden区开始的，同时Eden区不会分配的很大，所以Eden区的GC会频繁进行。因而，一般在这里需要使用速度快、效率高的算法，使Eden去能尽快空闲出来。

### Full GC

对整个堆进行整理，包括Young、Tenured和Perm。**Full GC因为需要对整个堆进行回收**，所以比Scavenge GC要慢，因此应该尽可能减少Full GC的次数。在对JVM调优的过程中，很大一部分工作就是对于Full GC的调节。有如下原因可能导致Full GC：

a) 年老代（Tenured）被写满；

b) 持久代（Perm）被写满；

c) System.gc()被显示调用；

d) 上一次GC之后Heap的各域分配策略动态变化；