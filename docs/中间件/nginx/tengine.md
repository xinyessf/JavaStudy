### Tengine

#### 是什么

[官网]( http://tengine.taobao.org/ )

> Tengine是由淘宝网发起的Web服务器项目。它在[Nginx](http://nginx.org/)的基础上，针对大访问量网站的需求，添加了很多高级功能和特性。Tengine的性能和稳定性已经在大型的网站如[淘宝网](http://www.taobao.com/)，[天猫商城](http://www.tmall.com/)等得到了很好的检验。它的最终目标是打造一个高效、稳定、安全、易用的Web平台。 

#### 使用特性

>- 继承Nginx-1.17.3的所有特性，兼容Nginx的配置；
>- 支持HTTP的[CONNECT](http://tengine.taobao.org/document_cn/proxy_connect_cn.html)方法，可用于正向代理场景；
>- [支持异步OpenSSL](http://tengine.taobao.org/document_cn/ngx_http_ssl_asynchronous_mode_cn.html)，可使用硬件如:[QAT](http://tengine.taobao.org/document_cn/tengine_qat_ssl_cn.html)进行HTTPS的加速与卸载；
>- 增强相关运维、监控能力,比如[异步打印日志及回滚](http://tengine.taobao.org/document_cn/ngx_log_pipe_cn.html),[本地DNS缓存](http://tengine.taobao.org/document_cn/core_cn.html),[内存监控](http://tengine.taobao.org/document_cn/ngx_debug_pool_cn.html)等；
>- Stream模块支持[server_name](http://tengine.taobao.org/document_cn/stream_sni_cn.html)指令；
>- 更加强大的负载均衡能力，包括[一致性hash模块](http://tengine.taobao.org/document_cn/http_upstream_consistent_hash_cn.html)、[会话保持模块](http://tengine.taobao.org/document_cn/http_upstream_session_sticky_cn.html)，[还可以对后端的服务器进行主动健康检查](http://tengine.taobao.org/document_cn/http_upstream_check_cn.html)，根据服务器状态自动上线下线，以及[动态解析upstream中出现的域名](http://tengine.taobao.org/document_cn/http_upstream_dynamic_cn.html)；
>- [输入过滤器机制](http://blog.zhuzhaoyuan.com/2012/01/a-mechanism-to-help-write-web-application-firewalls-for-nginx/)支持。通过使用这种机制Web应用防火墙的编写更为方便；
>- 支持设置proxy、memcached、fastcgi、scgi、uwsgi[在后端失败时的重试次数](http://tengine.taobao.org/document_cn/ngx_limit_upstream_tries_cn.html)
>- [动态脚本语言Lua](https://github.com/alibaba/tengine/blob/master/modules/ngx_http_lua_module/README.markdown)支持。扩展功能非常高效简单；
>- 支持按指定关键字(域名，url等)[收集Tengine运行状态](http://tengine.taobao.org/document_cn/http_reqstat_cn.html)；
>- [组合多个CSS、JavaScript文件的访问请求变成一个请求](http://tengine.taobao.org/document_cn/http_concat_cn.html)；
>- [自动去除空白字符和注释](http://tengine.taobao.org/document_cn/http_trim_filter_cn.html)从而减小页面的体积
>- 自动根据CPU数目设置进程个数和绑定CPU亲缘性；
>- [监控系统的负载和资源占用从而对系统进行保护](http://tengine.taobao.org/document_cn/http_sysguard_cn.html)；
>- [显示对运维人员更友好的出错信息，便于定位出错机器；](http://tengine.taobao.org/document_cn/http_footer_filter_cn.html)
>- [更强大的防攻击（访问速度限制）模块](http://tengine.taobao.org/document_cn/http_limit_req_cn.html)；
>- [更方便的命令行参数，如列出编译的模块列表、支持的指令等](http://tengine.taobao.org/document_cn/commandline_cn.html)；
>- 可以根据访问文件类型设置过期时间；
>- ……

### 做什么用

>  Nginx是一款轻量级的Web 服务器/反向代理服务器及电子邮件（IMAP/POP3）代理服务器，并在一个BSD-like 协议下发行。由俄罗斯的程序设计师Igor Sysoev所开发，

```
高并发
负载均衡
高可用
虚拟主机
伪静态
动静分离
```

#### 安装

>系统依赖组件 ``` gcc openssl-devel pcre-devel zlib-devel```
>
>安装：``yum install gcc openssl-devel pcre-devel zlib-devel -y ``

```shell
# ip 192.168.73.129
##上传Nginx压缩包到服务器，一般安装在/usr/local目录下
cd /usr/local/
tar -zxvf tengine-2.3.1.tar.gz
./configure --prefix=/usr/local/tengine
make && make install
##拷贝Nginx启动脚本,见对应路径下文件
/etc/init.d/nginx
chmod 777 nginx
## 启动服务
service nginx start 
## 访问 http://192.168.73.129/
## 停止
service nginx stop
##状态
service nginx status
## 动态重载配置文件
service nginx reload
```

### 配置文件详解

```shell
## 1.了解
##/usr/local/tengine/conf
vi  nginx.conf
### 配置
## 句柄
new FileInputStream()  需要文件路径
## 
```

#### Loction配置

```shell
  server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;
        #access_log  "pipe:rollback logs/host.access_log interval=1d baknum=7 maxsize=2G"  main;
		### 找静态文件
        location / {
            root   html;
            index  index.html index.htm;
        }
        ### root 路径
        ### 路径下
        
```

### 反向代理

```shell
##配置

```

#### tomcat反向代理

```shell
##tomcat 集群
upstream httpds {
    server 192.168.73.129:8080;
    server 192.168.73.130:8080;
}
  server {
        listen       80;
        server_name  localhost;

      
        location / {
			proxy_pass http://tomcats;
        }
   } 
        

```

