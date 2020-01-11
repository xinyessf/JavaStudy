### 常用命令

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

### vi文件编辑

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

#### 其他

```
reboot : 重启
halt : 关机
ifconfig 显示网络设备
ping 
ps -ef|grep tomcat
kill -9 pid

```

