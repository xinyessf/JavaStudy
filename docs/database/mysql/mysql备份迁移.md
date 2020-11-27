### 文档推荐

[修改mysql数据文件存储路径方案](https://blog.csdn.net/u014180504/article/details/79220607)

### 1.数据库迁移

>从/var/lib/mysql 转存到/var/lib/data/mysql/mysql
>
>思路:'
>
>1.修改路径
>
>2.将文件重新配置启动即可

```mysql
192.168.73.128为例子
## 原存储目录
/usr/local/mysql/data/
## 查看深度
df -h /usr/local/mysql/data/
## 查看当前文件深度
du -sh
##/
cp -a /usr/local/mysql/data/  /data/mysql/data/
## 配置文件
cp /etc/my.cnf /etc/my.cnfbak
datadir=/data/mysql/data
service mysqld start
## 备份完成
```

