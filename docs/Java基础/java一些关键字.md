### static

#### 静态变量和静态方法

```
随着类的加载而加载

```

#### 静态代码块

```
随着类的加载而加载,只启动一次;
对象实例化代码块每次都执行,但是代码块只执行一次
静态代码块是严格按照父类静态代码块->子类静态代码块的顺序加载的，且只加载一次。
```

#### **static修饰类**

```java
这个用得相对比前面的用法少多了，static一般情况下来说是不可以修饰类的，如果static要修饰一个类，说明这个类是一个静态内部类（注意static只能修饰一个内部类），也就是匿名内部类。像线程池ThreadPoolExecutor中的四种拒绝机制CallerRunsPolicy、AbortPolicy、DiscardPolicy、DiscardOldestPolicy就是静态内部类。

class OuterClass{
  private static String msg = "GeeksForGeeks";
  // 静态内部类
  public static class NestedStaticClass{
    // 静态内部类只能访问外部类的静态成员
    public void printMessage() {
     // 试着将msg改成非静态的，这将导致编译错误
     System.out.println("Message from nested static class: " + msg);
    }
  }
  // 非静态内部类
  public class InnerClass{
    // 不管是静态方法还是非静态方法都可以在非静态内部类中访问
    public void display(){
     System.out.println("Message from non-static nested class: "+ msg);
    }
  }
}
```

### final

*修饰类:*

 public final class String 、 public final class Math 、 public final class Scanner
等，很多我们学习过的类，都是被final修饰的，目的就是供我们使用，而不让我们所以改变其内容。

*修饰变量:*

> 局部变量——基本类型;基本类型的局部变量，被final修饰后，只能赋值一次，不能再更改。
>
> 局部变量——引用类型 也不可以修改,改变了地址值
>
> 成员变量----  初始化没值的,初始化完成才有值

*修饰方法:*

重写被  final 修饰的方法，编译时就会报错。

