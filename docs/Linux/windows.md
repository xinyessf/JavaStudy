### windows 常用命令

```
cd   切换目录

例：cd   // 显示当前目录

例：cd ..   // 进入父目录

例：cd /d d:   // 进入上次d盘所在的目录（或在直接输入：d:）

例：cd /d E:\   // 进入d盘根目录

例：cd d: // 显示上次d盘所在的目录

例：cd /d d:\src // 进入d:\src目录

例：cd prj\src\view  // 进入当前目录下的prj\src\view文件夹
```

### 查看关闭端口

```
1.netstat -ano |findstr  端口号    得到进程号   (findstr 很像linux下的grep命令)

2.  taskkill /pid  进程号  /F


```

### 安装mysql

[mysql]([https://blog.csdn.net/weixin_42394615/article/details/82596085](https://blog.csdn.net/weixin_42394615/article/details/82596085))

[解压安装]([https://blog.csdn.net/ermaner666/article/details/79096939/](https://blog.csdn.net/ermaner666/article/details/79096939/))

```shell
##解压版
##路径
D:\javaTools\mysql-5.6.43-winx64
配置环境变量
D:\javaTools\mysql-5.6.43-winx64\bin
## 到 bin下 ，以管理员身份
cd /d d:\  
cd D:\javaTools\mysql-5.6.43-winx64\bin
mysqld install
net start mysql
mysql -uroot -p
```

### redis安装

```

```

### svn安装

[svn]([https://www.cnblogs.com/zhigu/p/10794283.html](https://www.cnblogs.com/zhigu/p/10794283.html))

### vue

[window镜像安装]([https://blog.csdn.net/zjh_746140129/article/details/80460965](https://blog.csdn.net/zjh_746140129/article/details/80460965))

```

```