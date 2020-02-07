### 语法

```mysql
#!bin/bash
#可执行权限
chmod +x test.sh
#打印语句
echo "jar启动完成"
```

### 定义变量

```mysql
#定义A
A=10
#打印A
echo $A
#将一个变量赋值给另一个变量
A=$STR
#列出所有的变量：
 set 
#删除变量：
unset NAME
```

### 单引号和双引号的作用

```
现象：单引号里的内容会全部输出，而双引号里的内容会有变化
原因：单引号会将所有特殊字符脱意
NUM=10    
SUM="$NUM hehe"     echo $SUM     输出10 hehe
SUM2='$NUM hehe'     echo $SUM2    输出$NUM hehe
```

### $

```

```

| $n   | n为数字，$0代表命令本身，$1-$9代表第一到第9个参数,  十以上的参数需要用大括号包含，如${10}。 |
| ---- | ------------------------------------------------------------ |
| $*   | 代表命令行中所有的参数，把所有的参数看成一个整体。以"$1 $2 … $n"的形式输出所有参数 |
| $@   | 代表命令行中的所有参数，把每个参数区分对待。以"$1" "$2" …  "$n" 的形式输出所有参数 |
| $#   | 代表命令行中所有参数的个数。添加到shell的参数个数            |

| $?   | 执行上一个命令的返回值  执行成功，返回0，执行失败，返回非0（具体数字由命令决定） |
| ---- | ------------------------------------------------------------ |
| $$   | 当前进程的进程号（PID），即当前脚本执行时生成的进程号        |
| $!   | 后台运行的最后一个进程的进程号（PID），最近一个被放入后台执行的进程  & |

### 运算符

```mysql
(注意：运算符前后必须要有空格) 
num1=11
num2=22
sum=$num1+$num2
echo $sum
#格式 :expr m + n 或$((m+n)) 注意expr运算符间要有空格
expr命令：对整数型变量进行算术运算
#$( )的用途和反引号``一样，用来表示优先执行的命令
eg:echo $(ls a.txt)
#${ } 就是取变量了  eg：echo ${PATH}
#$((运算内容)) 适用于数值运算
```

### 比较

```mysql
#内置test命令
#内置test命令常用操作符号[]表示，将表达式写在[]中，如下：
[ expression ]
[ “$name” ] && echo ok
test “$name” == ”yangmi” && echo ok  || echo  invalid
```



### if

```mysql
if [ 条件判断式 ]
    then
        程序
fi    
或者
if [ 条件判断式 ] ; then 
    程序
fi

```

### case

```mysql
case命令是一个多分支的if/else命令，case变量的值用来匹配value1,value2,value3等等。匹配到后则执行跟在后面的命令直到遇到双分号为止(;;)case命令以esac作为终止符。
格式
	CMD=$1
case $CMD in
start)
	echo "starting"
	;;
Stop)
	echo "stoping"
	;;
*)
	echo "Usage: {start|stop} “
esac

```

### for

```mysql
for N in 1 2 3 
do
	echo $N
done
或
for N in 1 2 3; do echo $N; done
或
for N in {1..3}; do echo $N; done

for ((i = 0; i <= 5; i++))
do
	echo "welcome $i times"
done
或
for ((i = 0; i <= 5; i++)); do echo "welcome $i times"; done
```

while

```
while命令根据紧跟其后的命令(command)来判断是否执行while循环，当command执行后的返回值(exit status)为0时，则执行while循环语句块，直到遇到done语句，然后再返回到while命令，判断command的返回值，当得打返回值为非0时，则终止while循环。
while expression
do
command
…
done

```

### 自定义函数

```mysql
函数代表着一个或一组命令的集合，表示一个功能模块，常用于模块化编程。
以下是关于函数的一些重要说明：	
在shell中，函数必须先定义，再调用
使用return value来获取函数的返回值
函数在当前shell中执行，可以使用脚本中的变量。
函数名()
{
命令1…..
命令2….
return 返回值变量
}
[ function ] funname [()]
{
  action;
  [return int;]
}
function start()  / function start  / start()
#####stop的引用:
function stop(){
if [ "$pidlist" == "" ]
  then
    echo "----tomcat 已经关闭----"
    
 else
    echo "tomcat进程号 :$pidlist"
    kill -9 $pidlist
    echo "KILL $pidlist:"
fi
}
stop
```

### awk和sed 

```mysql
一个强大的文本分析工具
把文件逐行的读入，以空格为默认分隔符将每行切片，切开的部分再进行各种分析处理。
语法：awk ‘条件1{动作1}条件2{动作2}...’文件名
条件（Pattern）:
一般使用关系表达式作为条件： >   >=  <=等
动作（Action）：
格式化输出
流程控制语句
eg:#df -h | awk '{print $1 "\t" $3}'      显示第一列和第三列 
```

### 脚本调试

```mysql
sh -x script
这将执行该脚本并显示所有变量的值。
在shell脚本里添加  
set -x  对部分脚本调试
sh -n scripts
不执行脚本只是检查语法的模式，将返回所有语法错误。
sh –v script
执行并显示脚本内容
```



### tomcat启动脚本

```mysql
#!/bin/sh
export BUILD_ID=module-shop-soa_build_id

# 1.关闭tomcat,此处tomcat根据个人配置
pidlist=`ps -ef|grep apache-tomcat-9.0.20|grep -v "grep"|awk '{print $2}'`
function stop(){
if [ "$pidlist" == "" ]
  then
    echo "----tomcat 已经关闭----"
    
 else
    echo "tomcat进程号 :$pidlist"
    kill -9 $pidlist
    echo "KILL $pidlist:"
fi
}

stop
#根据个人配置
pidlist2=`ps -ef| grep apache-tomcat-9.0.20|grep -v "grep"|awk '{print $2}'`
if [ "$pidlist2" == "" ]
    then 
       echo "----关闭tomcat成功----"
else
    echo "----关闭tomcat失败----"
fi

# 2.复制jenkins生成的war包到tomcat中webapps中,根据个人配置
\cp -rf  /root/.jenkins/workspace/module-shop-soa/mvtech-shop-web/target/ROOT/* /root/mvtech_shop/app/ROOT


# 3.启动tomcat,根据个人配置
cd /usr/local/java/tomcat/apache-tomcat-9.0.20/bin
./startup.sh
```

### jar启动脚本

```mysql
#!/bin/sh

BUILD_ID=mvtech-shop-schedule_build_id #后台执行

pid=$(ps -ef|grep mvtech-shop-schedule-exec.jar|grep -v grep | awk '{print $2}')

#copy jar 到启动目录
\cp -rf /root/.jenkins/workspace/module-shop-soa/mvtech-shop-schedule/target/mvtech-shop-schedule-exec.jar /root/mvtech_shop/app

# 关闭已经启动的jar进程
function stop(){
if [ -n "$pid" ]
  then
  	echo "pid进程 :$pid"
  	kill -9 $pid
    
 else
    echo "jar没有启动"
fi
}
stop

sleep 5s

#发布jar服务
function start(){
  nohup java -Xmx512m -XX:+HeapDumpOnOutOfMemoryError -jar /root/mvtech_shop/app/mvtech-shop-schedule-exec.jar > /dev/null 2>/root/mvtech_shop/data/logs/mvtech-shop-schedule/mvtech-shop-schedule-error.log &
}
echo "jar启动完成"
start

```

### vue

```
#!/bin/sh
cd /root/.jenkins/workspace/shop
echo "====================================start installing dependencies===================="
cnpm install;
echo "====================================start building===================================="
npm run build
echo "====================================start transfering dist files======================"
\cp -rf dist/* /root/mvtech_shop/app/ROOT/
```

