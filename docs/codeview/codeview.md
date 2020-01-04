# CodeView

[浅谈软件工程中的代码评审](https://blog.csdn.net/vipshop_fin_dev/article/details/83048909)

[代码审查怎么做](https://www.zhihu.com/question/20046020)

[如何从零开始做代码评审](https://www.cnblogs.com/yzycoder/p/6873601.html)

[阿里巴巴开发规范](https://github.com/alibaba/p3c)

[代码审查插件](https://www.cnblogs.com/chenjfblog/p/7685579.html)

[请停止代码注释](https://juejin.im/post/5cf60bc8f265da1baa1e609e)

### 目的

> 码出高效，码出质量
>
> 功能性 - 功能完整，是否严格按照产品说明书实现产品的所需的功能点
> 可读性 - 代码易读易懂，其它人能够轻松从代码中读逻辑和设计思想，命名是一个学问。
> 可测性 - 代码能够轻松被单元测试覆盖，一般来说无法被单元测试覆盖的代码不是一个好代码
> 可维护性 - 代码运行期间日志输出完整，运维人员或者其它人员可以从日志中了解应用的运行逻辑和状态。
> 性能 - 天下武功唯快不破，确保代码在可度量的数据量级下面保持一个稳定的性能表现。代码是否存在性能问题，预计峰值流量能到多少。
> 

# 要点

* 注释
* 命名
* 未关闭的流，未结束的循环
* 日志

[评审要点](https://www.cnblogs.com/erma0-007/p/8654982.html)

*先从简单开始*

## 注释

###eclipse

**类**

```
举个栗子:
/**
 * <p>Title:${type_name}</p>
 * <p>Description:${todo}</p>
 * <p>Copyright [${year}]</p>
 * <p>Company: 微智信业科技有限公司</p>
 * @author ${user}
 * @date ${date}
 * @version 1.0
 */

```

**方法**

```
//父类方法注释,子类方法可不用注释
所有抽象方法加注释,实现类不用加
单个方法的总行数不超过 80 行。
举个栗子:
/**
 * <p>Description:${todo}</p>
 * ${tags}
 * @param
 * @author ${user}
 * @date ${date} ${time}
 * @version 1.0
*/

```

###idea

```
类注释
/**
 * @Description: $description$
 * @author: sunsf
 * @date: $Date$ $time$
 */
方法注释,有异常参数
/**
 * @Description: $description$
 * @param:  $param$
 * @return: $return$
 * @author: sunsf
 * @date:   $date$ $time$
 */
```

### 常量

```
行尾注释不可用
private final int RANGEDOM_LENGTH = 10;
```

### 其它

维护项目: modify by author 2019-12-25 修改用户名     add by author 2019-12-25 修改用户名

## 命名规范

>命名规范
>
>1. 类驼峰, 方法首字母小写,驼峰
>
>2. 包命名小写, 
>
>3. 不要出现中英文混合的方式
>
>4. 常量大写,下划线连接
>
>5. 中括号是数组类型的一部分，数组定义如下：String[] args;
>
>6. 杜绝完全不规范的缩写，避免望文不知义。

### 常量

>1. 不要出现魔法值;
>
>2. Long 数值后边使用大写的L,不能是小写 l 容易和数字1混淆

### OOP

> 1. Object 的 equals 方法容易抛空指针异常，应使用常量或确定有值的对象来调用equals ;
>
> 2. 所有整型包装类对象之间值的比较，全部使用 equals 方法比较;
>
> 3. return 返回使用包装类;
>
> 4. 局部变量使用基本类型;
>
> 5. 所有的覆写方法，必须加@ Override 注解。

## 集合处理

>1. Map,List初始长度比较大的,要给指定一个初始容量,防止扩容影响性能;
>
>2. 不要在for中用size(),建议用一个变量,避免性能问题
>
>例如:
>
>​       for(int i=0;i<list.size();i++){}
>
>改为:
>
>​       int size=list.size();
>
>for(int i=0;i<size;i++){}

### 控制语句

>1. if语句不要有复杂逻辑
>
>2. if后面不省略括号 
>
>3. switch 语句,要有default默认

### Mysql

>1.mybatis中写sql,建议格式化后粘贴

### 其他

>方法不超过80行,类不超过500行