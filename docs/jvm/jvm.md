## JVM

###文章

[jdk8之前用法](https://blog.csdn.net/qq_41701956/article/details/81664921)

[jdk8和jdk7比较](https://blog.csdn.net/zhou920786312/article/details/97984663)

[jdk8内存有啥变化](https://blog.csdn.net/chlu113/article/details/51890469)

jdk8之后新增了元空间

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

[引用](https://blog.csdn.net/u012998254/article/details/81428621)

**强引用**

```

```

**软引用**

```

```

**弱引用**

```

```

**虚引用**

```

```

## 垃圾收集算法

### 标记-清除算法

```
标记,并清楚
缺点:效率不高,空间会产生大量碎片
```

### 复制算法

```

```

### 标记-整理算法

```

```

### 分代收集算法

```

```

**新生代**

**老年代**

## 垃圾回收器

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91c2VyLWdvbGQtY2RuLnhpdHUuaW8vMjAxNy85LzQvMTVmYjc1NDc2MmZmNWRmM2Y3ZjYzZTVjMjZkNGQzYWU_aW1hZ2VWaWV3Mi8wL3cvMTI4MC9oLzk2MC9mb3JtYXQvd2VicC9pZ25vcmUtZXJyb3IvMQ)

### Serial 收集器

```
这是一个单线程收集器。意味着它只会使用一个 CPU 或一条收集线程去完成收集工作，并且在进行垃圾回收时必须暂停其它所有的工作线程直到收集结束。
```

### ParNew 收集器

```
指多条垃圾收集线程并行工作，此时用户线程处于等待状态
```

### Parallel Scavenge 收集器

```
这是一个新生代收集器，也是使用复制算法实现，同时也是并行的多线程收集器。
```

### Parallel Old 收集器

```
Parallel Old 是 Parallel Scavenge 收集器的老年代版本。多线程，使用 标记 —— 整理
```

### CMS收集器

```
CMS (Concurrent Mark Sweep) 收集器是一种以获取最短回收停顿时间为目标的收集器。基于 标记 —— 清除 算法实现。
```

### G1 收集器

```
面向服务端的垃圾回收器。
```

