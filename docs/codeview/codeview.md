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

### 类

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

### 方法

```
//父类方法注释,子类方法可不用注释
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

### 变量

```
举个栗子:
//注释
```

### 常量

```
举个栗子:
全局常量 都是大写
/*成功标记**/
private final int RANGEDOM_LENGTH = 10;
```

### 其它

维护项目: modify by author 2019-12-25 修改用户名     add by author 2019-12-25 修改用户名

## 命名



