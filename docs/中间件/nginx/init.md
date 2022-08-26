## Nginx

### 了解nginx

>nginx是一款高性能的http 服务器/反向代理服务器及电子邮件（IMAP/POP3）代理服务器。由俄罗斯的程序设计师Igor Sysoev所开发，官方测试nginx能够支支撑5万并发链接，并且cpu、内存等资源消耗却非常低，运行非常稳定，所以现在很多知名的公司都在使用nginx。nginx是一款高性能的http 服务器/反向代理服务器及电子邮件（IMAP/POP3）代理服务器。由俄罗斯的程序设计师Igor Sysoev所开发，官方测试nginx能够支支撑5万并发链接，并且cpu、内存等资源消耗却非常低，运行非常稳定，所以现在很多知名的公司都在使用nginx。

### nginx的应用场景

>1、http服务器。Nginx是一个http服务可以独立提供http服务。可以做网页静态服务器。
>
>2、虚拟主机。可以实现在一台服务器虚拟出多个网站。例如个人网站使用的虚拟主机。
>
>3、反向代理，负载均衡。当网站的访问量达到一定程度后，单台服务器不能满足用户的请求时，需要用多台服务器集群可以使用nginx做反向代理。并且多台服务器可以平均分担负载，不会因为某台服务器负载高宕机而某台服务器闲置的情况。

![](https://img-blog.csdn.net/20140922163329773?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYWJjMTk5MDA4Mjg=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

### nginx优缺点

>占内存小，可以实现高并发连接、处理响应快。
>
>可以实现http服务器、虚拟主机、反向代理、负载均衡。
>
>nginx配置简单
>
>可以不暴露真实服务器IP地址

### nginx的使用

#### 反向代理

反例:什么是正向代理

在如今的网络环境下，我们如果由于技术需要要去访问国外的某些网站，此时你会发现位于国外的某网站我们通过浏览器是没有办法访问的，此时大家可能都会用一个操作FQ进行访问，FQ的方式主要是找到一个可以访问国外网站的代理服务器，我们将请求发送给代理服务器，代理服务器去访问国外的网站，然后将访问到的数据传递给我们！

>反向代理（Reverse Proxy）方式是指以[代理服务器](https://baike.baidu.com/item/代理服务器)来接受internet上的连接请求，然后将请求转发给内部网络上的服务器，并将从服务器上得到的结果返回给internet上请求连接的客户端，此时代理服务器对外就表现为一个反向代理服务器。
>
>启动一个Tomcat 127.0.0.1:8080
>
>使用nginx反向代理 8080.itmayiedu.com 直接跳转到127.0.0.1:8080

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MTUyNTk1LWYxZTdlZTA5MDdiZjJhMTUucG5n?x-oss-process=image/format,png)

#### 负载均衡

>负载均衡 建立在现有网络结构之上，它提供了一种廉价有效透明的方法扩展网络设备和服务器的带宽、增加吞吐量、加强网络数据处理能力、提高网络的灵活性和可用性。

### linux中安装nginx

[安装教程](https://www.cnblogs.com/xxoome/p/5866475.html)

```shell
## ip 192.168.73.129
## 是否安装了gcc、pcre-devel、zlib-devel、openssl-devel
## 是否安装了gcc
yum list installed | grep "gcc"
yum list installed | grep "pcre-devel"
yum list installed | grep "zlib-devel"
yum list installed | grep "openssl-devel"
##
cd /usr/local/
## 下载
wget http://nginx.org/download/nginx-1.8.0.tar.gz
##
tar -zxvf nginx-1.8.0.tar.gz
##进入nginx目录
cd nginx-1.8.0
## 配置
./configure --prefix=/usr/local/nginx
# make
make
make install
##启动
/usr/local/nginx/sbin/nginx
/usr/local/nginx/sbin/nginx -s reload ##重启
##停止
/usr/local/nginx/sbin/nginx -s stop
##测试配置文件是否正常：
/usr/local/nginx/sbin/nginx -t 
##强制关闭
pkill nginx
##设置开机自启动
vim /etc/rc.d/rc.local  ##添加内容 /usr/local/nginx/sbin/nginx
```

