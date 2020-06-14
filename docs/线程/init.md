多线程

##了解多线程

### 进程和线程的区别

>每个正在系统上运行的程序都是一个进程。每个进程包含一到多个线程。线程是一组指令的集合，或者是程序的特殊段，它可以在程序里独立执行。也可以把它理解为代码运行的上下文。所以线程基本上是轻量级的进程，它负责在单个程序里执行多任务。通常由操作系统负责多个线程的调度和执行。
>
>使用线程可以把占据时间长的程序中的任务放到后台去处理，程序的运行速度可能加快，在一些等待的任务实现上如用户输入、文件读写和网络收发数据等，线程就比较有用了。在这种情况下可以释放一些珍贵的资源如内存占用等等。
>
>如果有大量的线程,会影响性能，因为操作系统需要在它们之间切换，更多的线程需要更多的内存空间，线程的中止需要考虑其对程序运行的影响。通常块模型数据是在多个线程间共享的，需要防止线程死锁情况的发生。
>
>总结:进程是所有线程的集合，每一个线程是进程中的一条执行路径。

### 为什么要使用多线程

>**总结多线程的好处提高程序的效率。**

### 多线程的创建方式

[多线程的创建方式](线程创建方式使用.md)

### 多线程的常用方法

| **常用线程****api****方法**      |                                                    |
| -------------------------------- | -------------------------------------------------- |
| start()                          | 启动线程                                           |
| currentThread()                  | 获取当前线程对象                                   |
| getID()                          | 获取当前线程ID   Thread-编号 该编号从0开始         |
| getName()                        | 获取当前线程名称                                   |
| sleep(long mill)                 | 休眠线程                                           |
| Stop（）                         | 停止线程                                           |
| **常用线程构造函数**             |                                                    |
| Thread（）                       | 分配一个新的 Thread 对象                           |
| Thread（String name）            | 分配一个新的 Thread对象，具有指定的 name正如其名。 |
| Thread（Runable r）              | 分配一个新的 Thread对象                            |
| Thread（Runable r, String name） | 分配一个新的 Thread对象                            |

### 线程的状态

![](https://note.youdao.com/yws/public/resource/68bba2bcc59acb94a07c2dcac2dd0c6c/xmlnote/9577358869A442A78007CDD2D1F66430/4621)

>1. 新建状态:
>   使用 new 关键字和 Thread 类或其子类建立一个线程对象后，该线程对象就处于新建状态。它保持这个状态直到程序 start() 这个线程。
>
>2. 就绪状态:
>   当线程对象调用了start()方法之后，该线程就进入就绪状态。就绪状态的线程处于就绪队列中，要等待JVM里线程调度器的调度。
>
>3. 运行状态:
>   如果就绪状态的线程获取 CPU 资源，就可以执行 run()，此时线程便处于运行状态。处于运行状态的线程最为复杂，它可以变为阻塞状态、就绪状态和死亡状态。
>
>4. 阻塞状态:
>
>   如果一个线程执行了sleep（睡眠）、suspend（挂起）等方法，失去所占用资源之后，该线程就从运行状态进入阻塞状态。在睡眠时间已到或获得设备资源后可以重新进入就绪状态。可以分为三种：
>
>   - 等待阻塞：运行状态中的线程执行 wait() 方法，使线程进入到等待阻塞状态。
>   - 同步阻塞：线程在获取 synchronized同步锁失败(因为同步锁被其他线程占用)。
>   - 其他阻塞：通过调用线程的 sleep() 或 join() 发出了 I/O请求时，线程就会进入到阻塞状态。当sleep() 状态超时，join() 等待线程终止或超时，或者 I/O 处理完毕，线程重新转入就绪状态。
>
>5. 死亡状态:
>   一个运行状态的线程完成任务或者其他终止条件发生时，该线程就切换到终止状态。
>
>

### 线程的三大特性

>多线程有三大特性，原子性、可见性、有序性

```
原子性:其实就是保证数据一致、线程安全一部分，

可见性:当多个线程访问同一个变量时，一个线程修改了这个变量的值，其他线程能够立即看得到修改的值。
若两个线程在不同的cpu，那么线程1改变了i的值还没刷新到主存，线程2又使用了i，那么这个i值肯定还是之前的，线程1对变量的修改线程没看到这就是可见性问题。 

有序性:
int a=5;
int b=3;
a=a+b;
r=a*a;
```

### java内存模型

[内存](https://zhuanlan.zhihu.com/p/29881777)

>线程之间的共享变量存储在主内存（Main Memory）中
>每个线程都有一个私有的本地内存（Local Memory），本地内存是JMM的一个抽象概念，并不真实存在，它涵盖了缓存、写缓冲区、寄存器以及其他的硬件和编译器优化。本地内存中存储了该线程以读/写共享变量的拷贝副本。
>从更低的层次来说，主内存就是硬件的内存，而为了获取更好的运行速度，虚拟机及硬件系统可能会让工作内存优先存储于寄存器和高速缓存中。
>Java内存模型中的线程的工作内存（working memory）是cpu的寄存器和高速缓存的抽象描述。而JVM的静态内存储模型（JVM内存模型）只是一种对内存的物理划分而已，它只局限在内存，而且只局限在JVM的内存。

![内存](https://pic3.zhimg.com/80/v2-af520d543f0f4f205f822ec3b151ad46_1440w.jpg)

```

```



## 多线程安全

**为什么**

>当多个线程同时共享，同一个**全局变量或静态变量**，做写的操作时，可能会发生数据冲突问题，也就是线程安全问题。但是做读操作是不会发生数据冲突问题。

**怎么解决线程安全**

>1.使用多线程之间同步或使用锁(lock)。
>
>2.将可能会发生数据冲突问题(线程不安全问题)，只能让当前一个线程进行执行。代码执行完成后释放锁，让后才能让其他线程进行执行。这样的话就可以解决线程不安全问题。
>
>3.当多个线程共享同一个资源,不会受到其他线程的干扰.

### 使用同步代码块

>什么是同步函数？
>
>在方法上修饰synchronized 称为同步函数

```
synchronized(同一个数据){
 可能会发生线程冲突问题
}
```

### 锁的种类

[锁的种类](锁的种类.md)

### 同步函数

>同步函数使用this锁。
>
>证明方式: 一个线程使用同步代码块(this明锁),另一个线程使用同步函数。如果两个线程抢票不能实现同步，那么会出现数据错误。

```java
public synchronized void sale() {
			if (trainCount > 0) { 
			try {
					Thread.sleep(40);
				} catch (Exception e) {
				}
System.out.println(Thread.currentThread().getName() + ",出售 第" + (100 - trainCount + 1) + "张票.");
				trainCount--;
			}
	}
```

### 静态同步函数

>方法上加上static关键字，使用synchronized 关键字修饰 或者使用类.class文件。
>
>静态的同步函数使用的锁是 该函数所属字节码文件对象 
>
>可以用 getClass方法获取，也可以用当前 类名.class 表示。

```java
synchronized (ThreadTrain.class) {
			System.out.println(Thread.currentThread().getName() + ",出售 第" + (100 - trainCount + 1) + "张票.");
			trainCount--;
			try {
				Thread.sleep(100);
			} catch (Exception e) {
			}
}
```

### 多线程死锁

>同步中嵌套同步,导致锁无法释放;
>
>两个或者多个线程相互等待对方释放锁;

## 多线程之间通信

> 多线程之间通讯，其实就是多个线程在操作同一个资源，但是操作的动作不同。

### wait,notify,notifyAll

>wait()、notify()、notifyAll()是三个定义在Object类里的方法，可以用来控制线程的状态。
>
>这三个方法最终调用的都是jvm级的native方法。随着jvm运行平台的不同可能有些许差异。
>
>·    如果对象调用了wait方法就会使持有该对象的线程把该对象的控制权交出去，然后处于等待状态。
>
>·    如果对象调用了notify方法就会通知某个正在等待这个对象的控制权的线程可以继续运行。
>
>·    如果对象调用了notifyAll方法就会通知所有等待这个对象控制权的线程继续运行。
>

```
•	如果对象调用了wait方法就会使持有该对象的线程把该对象的控制权交出去，然后处于等待状态。
•	如果对象调用了notify方法就会通知某个正在等待这个对象的控制权的线程可以继续运行。
•	如果对象调用了notifyAll方法就会通知所有等待这个对象控制权的线程继续运行。
```

### join和yield

>join作用是让其他线程先执行,执行完了,再执行这个线程
>
>Thread.yield()方法的作用：暂停当前正在执行的线程，并执行其他线程。（可能没有效果）
>
>yield()让当前正在运行的线程回到可运行状态，以允许具有相同优先级的其他线程获得运行的机会。因此，使用yield()的目的是让具有相同优先级的线程之间能够适当的轮换执行。但是，实际中无法保证yield()达到让步的目的，因为，让步的线程可能被线程调度程序再次选中。
>
>结论：大多数情况下，yield()将导致线程从运行状态转到可运行状态，但有可能没有效果。
>
>

###Lock 接口与 synchronized 

>Lock接口可以尝试非阻塞地获取锁当前线程尝试获取锁。如果这一时刻锁没有被其他线程获取到，则成功获取并持有锁。
> Lock 接口能被中断地获取锁与synchronized不同，获取到锁的线程能够响应中断，当获取到的锁的线程被中断时，中断异常将会被抛出，同时锁会被释放。
>
>Lock 接口在指定的截止时间之前获取锁，如果截止时间到了依旧无法获取锁，则返回。

### Condition

```
Condition condition = lock.newCondition();
res. condition.await();  类似wait
res. Condition. Signal() 类似notify
```

### 线程停止

>1. 使用退出标志，使线程正常退出，也就是当run方法完成后线程终止。
>
>  2. 使用stop方法强行终止线程（这个方法不推荐使用，因为stop和suspend、resume一样，也可能发生不可预料的结果）。
>
>  3. 使用interrupt方法中断线程。

### 守护线程

>什么是守护线程? 守护线程 进程线程(主线程挂了) 守护线程也会被自动销毁.
>
>Java中有两种线程，一种是用户线程，另一种是守护线程。
>
> 当进程不存在或主线程停止，守护线程也会被停止。

```
threadDemo.setDaemon(true)
```

### 优先级

>现代操作系统基本采用时分的形式调度运行的线程，线程分配得到的时间片的多少决定了线程使用处理器资源的多少，也对应了线程优先级这个概念。在JAVA线程中，通过一个int priority来控制优先级，范围为1-10，其中10最高，默认值为5。下面是源码（基于1.8）中关于priority的一些量和方法。

### Volatile

>Volatile 关键字的作用是变量在多个线程之间可见。
>
>因为Volatile不用具备原子性。

```java
class ThreadVolatileDemo extends Thread {
	public    boolean flag = true;
	@Override
	public void run() {
		System.out.println("开始执行子线程....");
		while (flag) {
		}
		System.out.println("线程停止");
	}
	public void setRuning(boolean flag) {
		this.flag = flag;
	}

}

public class ThreadVolatile {
	public static void main(String[] args) throws InterruptedException {
		ThreadVolatileDemo threadVolatileDemo = new ThreadVolatileDemo();
		threadVolatileDemo.start();
		Thread.sleep(3000);
		threadVolatileDemo.setRuning(false);
		System.out.println("flag 已经设置成false");
		Thread.sleep(1000);
		System.out.println(threadVolatileDemo.flag);

	}
}

```

###CountDownLatch、CyclicBarrier和Semaphore

[线程](CountDownLatch、CyclicBarrier和Semaphore.md)

### threadLocal

> ThreadLocal提高一个线程的局部变量，访问某个线程拥有自己局部变量。
>
>　当使用ThreadLocal维护变量时，ThreadLocal为每个使用该变量的线程提供独立的变量副本，所以每一个线程都可以独立地改变自己的副本，而不会影响其它线程所对应的副本。

>void set(Object value)设置当前线程的线程局部变量的值。
>public Object get()该方法返回当前线程所对应的线程局部变量。
>public void remove()将当前线程局部变量的值删除，目的是为了减少内存的占用，该方法是JDK 5.0新增的方法。需要指出的是，当线程结束后，对应该线程的局部变量将自动被垃圾回收，所以显式调用该方法清除线程的局部变量并不是必须的操作，但它可以加快内存回收的速度。
>protected Object initialValue()返回该线程局部变量的初始值，该方法是一个protected的方法，显然是为了让子类覆盖而设计的。这个方法是一个延迟调用方法，在线程第1次调用get()或set(Object)时才执行，并且仅执行1次。ThreadLocal中的缺省实现直接返回一个null。

```
class Res {
	// 生成序列号共享变量
	public static Integer count = 0;
	public static ThreadLocal<Integer> threadLocal = new ThreadLocal<Integer>() {
		protected Integer initialValue() {

			return 0;
		};

	};

	public Integer getNum() {
		int count = threadLocal.get() + 1;
		threadLocal.set(count);
		return count;
	}
}

public class ThreadLocaDemo2 extends Thread {
	private Res res;

	public ThreadLocaDemo2(Res res) {
		this.res = res;
	}

	@Override
	public void run() {
		for (int i = 0; i < 3; i++) {
			System.out.println(Thread.currentThread().getName() + "---" + "i---" + i + "--num:" + res.getNum());
		}

	}

	public static void main(String[] args) {
		Res res = new Res();
		ThreadLocaDemo2 threadLocaDemo1 = new ThreadLocaDemo2(res);
		ThreadLocaDemo2 threadLocaDemo2 = new ThreadLocaDemo2(res);
		ThreadLocaDemo2 threadLocaDemo3 = new ThreadLocaDemo2(res);
		threadLocaDemo1.start();
		threadLocaDemo2.start();
		threadLocaDemo3.start();
	}

}


```



## 线程池

[线程池的使用](线程池的使用.md)

[springboot线程池](线程池的使用.md)