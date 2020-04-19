### 了解maven

[官网](maven.apache.org)



```

```

###maven

```

```

### maven常用命令

####clean

>clean是maven工程的清理命令，执行 clean会删除target目录及其目录下所有内容

#### Compile

>compile是maven工程的编译命令，作用是将src/main/java下的java源文件编译为class文件并输出到target下的classes目录下。

#### test

>test是maven工程的测试命令 mvn test,会执行src/test/java下的单元测试类。
>
>cmd执行mvn test执行src/test/java下单元测试类,下图为测试结果,运行1个测试用例,全部成功。
>
>会编译java源码，同时也编译测试目录下的java源码，接着会运行测试类里的测试方法

####package

> package是maven工程的打包命令, 对于java工程执行package打成jar包,对于web工程打成war包工程目录下执行 mvn package

#### install

>install是maven工程的安装命令，执行install将maven打成jar包或war包发布到本地仓库,以上命令都会执行



### 依赖的传递性

#### 

### jar冲突解决

> idea中的快捷键:列出来

```
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
        <version>1.4.4.RELEASE</version>
        <exclusions>
            <exclusion>
                <groupId>com.google.guava</groupId>
                <artifactId>guava</artifactId>
            </exclusion>
    </exclusions>
</dependency>
```

### maven开发使用

```

```

