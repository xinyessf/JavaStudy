####lvs

```shell
LVS：

node01:
	ifconfig  eth0:8 192.168.150.100/24
node02~node03:
	1)修改内核：
		echo 1  >  /proc/sys/net/ipv4/conf/eth0/arp_ignore 
		echo 1  >  /proc/sys/net/ipv4/conf/all/arp_ignore 
		echo 2 > /proc/sys/net/ipv4/conf/eth0/arp_announce 
		echo 2 > /proc/sys/net/ipv4/conf/all/arp_announce 
	2）设置隐藏的vip：
		ifconfig  lo:3  192.168.150.100  netmask 255.255.255.255
		
RS中的服务：
node02~node03:
	yum install httpd -y
	service httpd start
	vi   /var/www/html/index.html
		from 192.168.150.1x

LVS服务配置
node01:
		yum install ipvsadm 
	ipvsadm -A  -t  192.168.150.100:80  -s rr
	ipvsadm -a  -t 192.168.150.100:80  -r  192.168.150.12 -g -w 1
	ipvsadm -a  -t 192.168.150.100:80  -r  192.168.150.13 -g -w 1
	ipvsadm -ln

验证：
	浏览器访问  192.168.150.100   看到负载  疯狂F5
	node01：
		netstat -natp   结论看不到socket连接
	node02~node03:
		netstat -natp   结论看到很多的socket连接
	node01:
		ipvsadm -lnc    查看偷窥记录本
		TCP 00:57  FIN_WAIT    192.168.150.1:51587 192.168.150.100:80 192.168.150.12:80
		FIN_WAIT： 连接过，偷窥了所有的包
		SYN_RECV： 基本上lvs都记录了，证明lvs没事，一定是后边网络层出问题			
```

#### keepalived实验

```shell
主机： node01~node04
node01:
	ipvsadm -C
	ifconfig eth0:8 down

----------------------------
node01,node04:
	yum install keepalived ipvsadm -y
	配置：
		cd  /etc/keepalived/
		cp keepalived.conf keepalived.conf.bak
		vi keepalived.conf
			node01:
			vrrp：虚拟路由冗余协议！
				vrrp_instance VI_1 {
					state MASTER         //  node04  BACKUP
					interface eth0
					virtual_router_id 51
					priority 100		 //	 node04	 50
					advert_int 1
					authentication {
						auth_type PASS
						auth_pass 1111
					}
					virtual_ipaddress {
						192.168.150.100/24 dev eth0 label  eth0:3
					}
				}
			virtual_server 192.168.150.100 80 {
				delay_loop 6
				lb_algo rr
				lb_kind DR
				nat_mask 255.255.255.0
				persistence_timeout 0
				protocol TCP

				real_server 192.168.150.12 80 {
					weight 1
					HTTP_GET {
						url {
						  path /
						  status_code 200
						}
						connect_timeout 3
						nb_get_retry 3
						delay_before_retry 3
					}   
				}       
				real_server 192.168.150.13 80 {
					weight 1
					HTTP_GET {
						url {
						  path /
						  status_code 200
						}
						connect_timeout 3
						nb_get_retry 3
						delay_before_retry 3
					}
				}
scp  ./keepalived.conf  root@node04:`pwd`
```

### keepalived搭建

[安装使用](https://blog.csdn.net/wengengeng/article/details/80837079)

[安装使用1](https://blog.csdn.net/bbwangj/article/details/80346428)

[依赖报错](https://blog.csdn.net/qq_36663951/article/details/80494965)

[使用](https://blog.csdn.net/bbwangj/article/details/80346428)

>

#####在线安装

```shell
安装依赖包
yum install -y curl gcc openssl-devel libnl3-devel net-snmp-devel
yum install -y keepalived
vi /etc/keepalived/keepalived.conf
## 启动
systemctl start keepalived  ##启动keepalived
systemctl enable keepalived  ##加入开机启动keepalived
systemctl restart keepalived ##重新启动keepalived
systemctl status keepalived   ##查看keepalived状态
```

#### 解压版

```shell
cd /usr/local
wget http://www.keepalived.org/software/keepalived-2.0.7.tar.gz
tar xvf keepalived-2.0.7.tar.gz
cd keepalived-2.0.7
##  yum -y install libnl libnl-devel  
##  yum -y install openssl-devel
## 依赖报错看上文
./configure --prefix=/usr/local/keepalived

make && make install

## 初始化及启动
# keepalived启动脚本变量引用文件，默认文件路径是/etc/sysconfig/，也可以不做软链接，直接修改启动脚本中文件路径即可（安装目录下）
cp /usr/local/keepalived/etc/sysconfig/keepalived  /etc/sysconfig/keepalived 
# 将keepalived主程序加入到环境变量（安装目录下）
cp /usr/local/keepalived/sbin/keepalived /usr/sbin/keepalived
# keepalived启动脚本（源码目录下），放到/etc/init.d/目录下就可以使用service命令便捷调用
cp /usr/local/keepalived-2.0.7/keepalived/etc/init.d/keepalived  /etc/init.d/keepalived

## 安装完成后,下边路径生成
/usr/local/etc/keepalived/keepalived.conf
/usr/local/etc/sysconfig/keepalived
/usr/local/sbin/keepalived
# 将配置文件放到默认路径下
mkdir /etc/keepalived
cp /usr/local/keepalived/etc/keepalived/keepalived.conf /etc/keepalived/keepalived.conf
##加为系统服务：
chkconfig –add keepalived
##开机启动：
chkconfig keepalived on
##查看开机启动的服务：
chkconfig –list
##启动、关闭、重启
service keepalived start|stop|restart
service keepalived restart
```

#### hainan-01(138)

```shell
global_defs {
    router_id LVS_DEVEL
    
    script_user root
    enable_script_security 
}

vrrp_script check_mysql
{
    script "/etc/keepalived/test3306.sh"    //执行监控脚本
    interval 2  
    weight -30
}

vrrp_instance VI_1
{
    state MASTER
    interface ens33        //配置成系统上网卡的名字
    virtual_router_id 71     //两台服务器的id必须相同
    priority 80        //优先级，通过该值判断应该用哪个实际ip
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1111
    }
    virtual_ipaddress {
     192.168.73.131        //虚拟IP
    }
    track_script {
      check_mysql
    }
}

```

#### hainan-02(139)

```shell
global_defs {
    router_id LVS_DEVEL
    
    script_user root
    enable_script_security 
}

vrrp_script check_mysql
{
    script "/etc/keepalived/test3306.sh"    //执行监控脚本
    interval 2  
    weight -30
}

vrrp_instance VI_1
{
    state MASTER
    interface ens33        //配置成系统上网卡的名字
    virtual_router_id 71     //两台服务器的id必须相同
    priority 100        //优先级，通过该值判断应该用哪个实际ip
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1111
    }
    virtual_ipaddress {
     192.168.73.131        //虚拟IP
    }
    track_script {
      check_mysql
    }
}
```

