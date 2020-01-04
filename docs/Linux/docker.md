# Docker

[Docker入门总结](http://dockone.io/article/8350)

[学习](http://www.ruanyifeng.com/blog/2018/02/docker-tutorial.html)

### 安装

```
## 检查内核版本,必须大于3.0
uname -r  
### ==1.安装yum 源
yum -y install 
##更新yum
yum update
## 查看之前的版本
yum list installed | grep docker
yum remove -y xx 卸载
或者yum remove docker-*
rm -rf /var/lib/docker  删除
## yum安装docker
yum install docker-io
或者  yum install docker-ce
## 查看docker 版本
docker version
## 启动docker 
service docker start
# 开机自启
systemctl enable docker 
# 启动docker服务  
systemctl start docker
systemctl stop docker       #关闭 docker
systemctl status docker    #查看 docker 的启动状态
systemctl restart docker    #重启 docker
```

### 使用

```
##查看
docker images
docker ps ## 查看正在运行的容器
## 查看容器
docker ps -n 5
## 查看所有的容器(包含了正在运行的容器以及之前启动过的容器)
docker ps -a  
##关闭
docker stop 
##启动
docker start id
## gogs
docker run -d ##name=gogs -p 10022:22 -p 3000:3000 -v /var/gogsdata:/datagogs/gogs
```

### mysql

```
## 搜索镜像
docker search mysql
##拉取镜像
docker pull mysql
##创建数据库容器mysql
docker run -di ##name=tensquare_mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=root centos/mysql-57-centos7
```

### mongodb

```
##创建mongodb容器
docker run ‐di ‐‐name=tensquare_mongo ‐p 27017:27017 mongo
```

### rabbitmq

```
## rabbitmq
docker run -di ##name=tensquare_rabbitmq -p 5671:5617 -p 5672:5672 -p 4369:4369 -p 15671:15671 -p 15672:15672 -p 25672:25672 rabbitmq:management
```