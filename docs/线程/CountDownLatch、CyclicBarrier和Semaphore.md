### 

[Semaphore](https://blog.csdn.net/longgeqiaojie304/article/details/91127730)

* 作用

防止资源过多使用,请求同时处理的线程数量

[CountDownLatch,CyclicBarrier,](https://www.cnblogs.com/dolphin0520/p/3920397.html)

> 两者区别总结：
>
>CyclicBarrier：是可以重重复使用的，只有等待线程积累到一定的数量的时候才会释放屏障，在释放屏障的时候还可以使用接口初始化CyclicBarrier变量（在里面定义释放屏障之前的动作，可以是释放线程池，终止上面所说的线程）。也可以使用只有int数据类型创建CyclicBarrier变量，这样只有在释放屏障的时候没有多余的动作。
>
>CountDownLatch：含有两个重要的方法CountDown()和await(),前者调用一次，CountDownLatch对象就会计数减少一次。await()导致当前线程等待计数器减少到零为止，除非出现中断的情况，而且CountDownLatch只有一次的机会，只会阻塞线程一次。
>
>触发屏障的事件不同：前者只有足够的线程调用await时候才会触发线程，后者是计数器减少到零的时候才会触发屏障，但是导致计数器减少的可能只是一个线程。

### Sempahop

```
Semaphore semaphore = new Semaphore(3);
        for (int i = 0; i < 5; i++) {
            new Thread(() -> {
                try {
                    semaphore.acquire();
                    System.out.println(Thread.currentThread().getName());
                    Thread.sleep(2000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                } finally {
                    semaphore.release();
                }
            }).start();
        }
```

### CountDownLatch

* 作用

```
需要等待其他线程执行完,才执行任务;
```

### CyclicBarrier

```
//可以重用
public static void main(String[] args) {
        CyclicBarrier cyclicBarrier = new CyclicBarrier(3);
        //放行;如果达到临界就放行吧
        startBarrier(cyclicBarrier);
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        startBarrier(cyclicBarrier);

    }
    private static void startBarrier(CyclicBarrier cyclicBarrier) {
        for (int i = 0; i < 3; i++) {
            new Thread(() -> {
                try {
                    System.out.println(Thread.currentThread().getName() + "开始===b");
                    Thread.sleep(2000);
                    System.out.println(Thread.currentThread().getName() + "结束===b");
                    cyclicBarrier.await();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                } catch (BrokenBarrierException e) {
                    e.printStackTrace();
                }
                System.out.println("b线程:" + Thread.currentThread().getName());
                try {
                    Thread.sleep(3000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }).start();
        }
    }
```

### 多线程取返回值

[多线程](https://blog.csdn.net/liuyishan1993/article/details/82347703)