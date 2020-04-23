## Docker

[Docker官方网址](https://docs.docker.com/)

[Docker中文网址](http://www.docker.org.cn/)

>Docker是一个开源的容器引擎，它有助于更快地交付应用。 Docker可将应用程序和基础设施层隔离，并且能将基础设施当作程序一样进行管理。使用 Docker可更快地打包、测试以及部署应用程序，并可以缩短从编写到部署运行代码的周期。

###Docker优点

#### 简化程序

>Docker 让开发者可以打包他们的应用以及依赖包到一个可移植的容器中，然后发布到任何流行的 Linux 机器上，便可以实现虚拟化。Docker改变了虚拟化的方式，使开发者可以直接将自己的成果放入Docker中进行管理。方便快捷已经是 Docker的最大优势，过去需要用数天乃至数周的 任务，在Docker容器的处理下，只需要数秒就能完成。

#### 避免选择恐惧症

>如果你有选择恐惧症，还是资深患者。Docker 帮你 打包你的纠结！比如 Docker 镜像；Docker 镜像中包含了运行环境和配置，所以 Docker 可以简化部署多种应用实例工作。比如 Web 应用、后台应用、数据库应用、大数据应用比如 Hadoop 集群、消息队列等等都可以打包成一个镜像部署。

#### 节省开支

>一方面，云计算时代到来，使开发者不必为了追求效果而配置高额的硬件，Docker 改变了高性能必然高价格的思维定势。Docker 与云的结合，让云空间得到更充分的利用。不仅解决了硬件管理的问题，也改变了虚拟化的方式。

### Docker架构

![img](https://www.runoob.com/wp-content/uploads/2016/04/576507-docker1.png)[]

```
镜像（Image）：Docker 镜像（Image），就相当于是一个 root 文件系统。比如官方镜像 ubuntu:16.04 就包含了完整的一套 Ubuntu16.04 最小系统的 root 文件系统。仓库（Repository）：仓库可看着一个代码控制中心，用来保存镜像。
容器（Container）：镜像（Image）和容器（Container）的关系，就像是面向对象程序设计中的类和实例一样，镜像是静态的定义，容器是镜像运行时的实体。容器可以被创建、启动、停止、删除、暂停等。
仓库（Repository）：仓库可看着一个代码控制中心，用来保存镜像。
```

### Docker安装

[**镜像仓库**](https://hub.docker.com/search?q=java&type=image)

>阿里云 https://cr.console.aliyun.com/cn-hangzhou/mirrors

```shell
##通过 uname -r 命令查看你当前的内核版本
uname -r
##使用 root 权限登录 Centos。确保 yum 包更新到最新。
yum -y update
##卸载旧版本(如果安装过旧版本的话)
yum remove docker docker-common docker-selinux docker-engine
##安装需要的软件包， yum-util 提供yum-config-manager功能，另外两个是devicemapper驱动依赖的
yum install -y yum-utils device-mapper-persistent-data lvm2
##设置yum源
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
## 使用阿里
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
##可以查看所有仓库中所有docker版本，并选择特定版本安装
yum list docker-ce --showduplicates | sort -r
##安装docker
sudo yum install -y docker-ce     #由于repo中默认只开启stable仓库，故这里安装的是最新稳定版18.03.1
##启动并加入开机启动
systemctl start docker
systemctl enable docker
##验证安装是否成功(有client和service两部分表示docker安装启动都成功了)
docker version
```

### 镜像相关命令

[docker安装jdk](https://blog.csdn.net/huangbaokang/article/details/99749296)

```shell
##搜索镜像
docker search java
##下载镜像
docker pull java:8
##使用命令docker pull命令即可从 Docker Registry上下载镜像，执行该命令后，Docker会从 Docker Hub中的 java仓库下载最新版本的 Java镜像。如果要下载指定版本则在java后面加冒号指定版本
## 列表已下载的镜像
docker images
##删除镜像
docker rmi d23bdf5b1b1b
```

