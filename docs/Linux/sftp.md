##sftp

### 适用场景

>我们平时习惯了使用FTP来上传下载文件，尤其是很多Linux的环境下，我们一般都会通过第三方的SSH工具连接到Linux的，但是当我们需要传输文件到Linux的服务器当中，很多人习惯用FTP来传输，其实Linux的默认是不提供FTP的，需要你额外安装FTP服务器。而且FTP服务器端会占用一定的VPS服务器资源。其实笔者更建议使用SFTP代替FTP。
>
>　　主要因为：一，可以不用额外安装任何服务器端程序（我比较中意这个，哈哈~~，很多公司为了安全性的Linux没有外网环境，只有SSH的时候，想传输文件是很悲催的问题）二，会更省系统资源。三，SFTP使用加密传输认证信息和传输数据，相对来说会更安全。四，也不需要单独配置，对新手来说比较简单（开启SSH默认就开启了SFTP）。
>
>

### sftp是什么

>FTP是一种文件传输协议，一般是为了方便数据共享的。包括一个FTP服务器和多个FTP客户端.FTP客户端通过FTP协议在服务器上下载资源。而SFTP协议是在FTP的基础上对数据进行加密，使得传输的数据相对来说更安全。但是这种安全是以牺牲效率为代价的，也就是说SFTP的传输效率比FTP要低（不过现实使用当中，没有发现多大差别）。

### sftp的好处

>SFTP不要安装; SFTP更安全，但更安全带来副作用就是的效率比FTP要低些。　　
>
>SFTP同样是使用加密传输认证信息和传输的数据，所以，使用SFTP是非常安全的。

###和ftp的优劣点

>　FTP是一种文件传输协议，一般是为了方便数据共享的。包括一个FTP服务器和多个FTP客户端.FTP客户端通过FTP协议在服务器上下载资源。而SFTP协议是在FTP的基础上对数据进行加密，使得传输的数据相对来说更安全。但是这种安全是以牺牲效率为代价的，也就是说SFTP的传输效率比FTP要低（不过现实使用当中，没有发现多大差别）。个人肤浅的认为就是：一; FTP要安装，SFTP不要安装二; SFTP更安全，但更安全带来副作用就是的效率比FTP要低些。

### Java中如何使用

[Java实现的SFTP](https://blog.csdn.net/weixin_36795183/article/details/78626215)

[hutool]()

```
hutool封装了这些方法的使用
```

### sftp常用命令

```shell
## linux远程连接
sftp root@192.168.1.200
get (下载)，
put 上传
mget 多文件下载
mput 多文件上传
bye/exit/quit 退出该ip
cd 切换服务器目录（远程主机）
lcd 切换本地目录（本地主机）
pwd 显示服务器当前路径（远程主机）
lpwd 显示本地当前路径（本地主机）
mkdir 在远程服务器上创建目录；（远程主机）
lmkdir 在本地系统上创建目录。（本地主机）
ls 显示服务器目录（远程主机）
lls 显示本地目录（本地主机）
```



```
put()：      文件上传
get()：      文件下载
cd()：       进入指定目录
ls()：       得到指定目录下的文件列表
rename()：   重命名指定文件或目录
rm()：       删除指定文件
mkdir()：    创建目录
rmdir()：    删除目录
```
