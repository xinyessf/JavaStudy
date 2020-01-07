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
tar -zxvf 解压文件
chmod 777  r4 w2 x1
free -k   或者-m -g
top -d 3  查看系统健康状态  每隔3秒
useradd  zhangsan  用户添加
passwd zhangsan    用户密码修改
userdel    用户删除
```