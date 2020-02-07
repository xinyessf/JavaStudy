### 常用命令

vm如何设置

[设置可以连接](https://blog.csdn.net/u014466635/article/details/80284792)

```
cd usr：   切换到该目录下usr目录  
cd ..（或cd../）：  切换到上一层目录 
cd /：   切换到系统根目录  
cd ~：   切换到用户主目录 
cd -：   切换到上一个操作所在目录
ll  查看列表
ls  查看列表
mkdir 创建文件
pwd   查看当前路径
chmod 777 添加所有权限
find  /home  -name  "*.txt"   查找文件
find  /home  -iname  "*.txt"  忽略大小写查找文件     
touch a.txt  创建文件
vi 进入编辑,如下
##########
IOA
dd删除一行
pp复制一行
/string  搜索
?string
:%s/old/new/g  全文替换
##########
rm -rf 删除
tar -zcvf 打包压缩后的文件名 要打包压缩的文件
tar -zxvf aa.tar.gz -C 具体的目录
tar -zxvf 解压文件
mv 原文件名称 新文件名称
chmod 777  r4 w2 x1
free -k   或者-m -g
top -d 3  查看系统健康状态  每隔3秒
useradd  zhangsan  用户添加
passwd zhangsan    用户密码修改
userdel    用户删除
```

#### 基本命令

```
cd .. 或者 cd ../ 回到上一级目录
cd x 进入下一级x目录  cd usr
cd ~ 进入root目录
cd / 回到 根目录
cd -  切换到上一个操作所在目录
```

#### 显示文件内容

```
ls -l  显示的更详细点， 可以看到该文件的权限、大小等等信息
ls -a  显示所有内容，包含隐藏文件
ls -al  
```

#### pwd命令

```
显示工作目录
```

#### 文件操作

```
mkdir 目录名称
rm -rf 目录名称  ： -r : 删除目录 -f :强制删除
mv 原来的目录名称  新的目录名称
touch|vi 文件名
mv 原文件名称 新文件名称
强制复制
\cp -rf aa /root/ 
复制
cp aa bb
```

####vi文件编辑

```
:wq 保存
:w  保存不退出
:q! 保存
i 在光标前插入
shift + i 在光标当前行开始插入
a 在光标后插入
shif + a 在光标当前行末尾插入
o  在光标当前行下一行插入
shift + o 在光标当前行上一行插入新行
yy 单行复制
nyy 多行复制
p   粘贴
定位:
gg到文本第一行
G 或者shift +g 到文本最后一行
dd删除一行
pp复制一行
/string  搜索
?string
:%s/old/new/g  全文替换

```



####权限

![img](https://images0.cnblogs.com/i/659307/201408/112054388731146.png)

```
chmod 777 xx
```



#### 显示文件内容

```
cat 一次性显示
more  
less  
以上两个分页显示
tail  -- 只显示末尾的内容
```

#### df

```
作用：用于查看Linux文件系统的状态信息,显示各个分区的容量、已使用量、未使用量及挂载点等信息。看剩余空间
语法：df [-hkam] [挂载点]
-h（human-readable）根据磁盘空间和使用情况 以易读的方式显示 KB,MB,GB等
-k 　以KB 为单位显示各分区的信息，默认
-M	以MB为单位显示信息
-a 　显示所有分区包括大小为0 的分区
```

#### du

```
作用：用于查看文件或目录的大小（磁盘使用空间）
语法：du [-ahs] [文件名目录]
	-a 显示子文件的大小
	-h以易读的方式显示 KB,MB,GB等
	-s summarize 统计总占有量
eg:
du -a(all) /home 　显示/home 目录下每个子文件的大小,默认单位为kb
du -h /home 以K，M,G为单位显示/home 文件夹下各个子目录的大小
```

#### free

```
作用：查看内存及交换空间使用状态
语法： free [-kmg]
选项：
-k:    以KB为单位显示，默认就是以KB为单位显示
-m:    以MB为单位显示
-g:    以GB为单位显示
```

#### top

```
作用：查看系统健康状态  
显示当前系统中耗费资源最多的进程,以及系统的一些负载情况。
语法：top [选项]
	-d 秒数，指定几秒刷新一次，默认3秒（动态显示）
```

#### 用户相关

```
useradd [选项] 用户名
passwd [选项] [用户名]
userdel -r 删除账号时同时删除宿主目录（remove）
```

#### RPM

```
RPM命令使用
查询所有安装的rpm包: # rpm –qa
查询mysql相关的包： # rpm –qa | grep mysql
安装：rpm  -ivh  jdk.rpm
卸载： rpm –e mysql*
强行卸载：rpm –e mysql*  --nodeps
YUM管理
yellowdog updater modified  软件包管理工具
查询
yum list     查询所有可用软件包列表
yum search  关键字     搜索服务器上所有和关键字相关的包
可以通过yum info 关键字 来查找包名
安装
yum -y install   包名     -y  自动回答yes  
```

### 端口号

```
之前查询端口是否被占用一直搞不明白，问了好多人，终于搞懂了，现在总结下：

netstat  -anp  |grep   端口号
3306为例，netstat  -anp  |grep  3306
netstat   -nultp
```



###yum

```
yum clean all
yum makecache
yum update
```

#### 上传和下载

```
yum install lrzsz
//上传
rz
//下载
sz
#wget
yum -y install wget
```



#### 其他

```find
reboot : 重启
halt : 关机
ifconfig 显示网络设备
ping 
ps -ef|grep tomcat
kill -9 pid
yum install lrzsz
//上传
rz
//下载
sz
```
