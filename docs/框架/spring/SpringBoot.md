# Spring Boot

[spring中文导航](http://springboot.fun/)

[spring-boot实践](https://github.com/xkcoding/spring-boot-demo)

```

```

#### 多个main方法找不到入库类

```xml
<build>
   <plugins>
      <plugin>  
          <groupId>org.springframework.boot</groupId>  
          <artifactId>spring-boot-maven-plugin</artifactId>  
                <configuration>  
                    <mainClass>com.mashibing.MyApp</mainClass>  
                </configuration>  
            </plugin>  
        </plugins>
</build>
```

#### 常用配置

```shell
##修改tomcat端口
server.port=90
##修改项目路径
server.servlet.context-path=/demo
##端口号
server.port=8888


##数据库配置
##数据库地址
spring.datasource.url=jdbc:mysql://localhost:3306/erp?characterEncoding=utf8&useSSL=false&serverTimezone=UTC
##数据库用户名
spring.datasource.username=root
##数据库密码
spring.datasource.password=840416
##数据库驱动
spring.datasource.driver-class-name=com.mysql.jdbc.Driver


##validate  加载hibernate时，验证创建数据库表结构
##create   每次加载hibernate，重新创建数据库表结构，这就是导致数据库表数据丢失的原因。
##create-drop        加载hibernate时创建，退出是删除表结构
##update                 加载hibernate自动更新数据库结构
##validate 启动时验证表的结构，不会创建表
##none  启动时不做任何操作
#spring.jpa.hibernate.ddl-auto=validate 

##控制台打印sql
spring.jpa.show-sql=true

spring.devtools.restart.enabled: true
spring.thymeleaf.cache=false
spring.thymeleaf.encoding=UTF-8


spring.http.encoding.force=true
spring.http.encoding.charset=UTF-8
spring.http.encoding.enabled=true
server.tomcat.uri-encoding=UTF-8

spring.mvc.date-format=yyyy-MM-dd
```

### springboot教程

[spring boot](http://c.biancheng.net/spring_boot/)

### springCloud入门教程

[Spring Cloud入门学习](http://c.biancheng.net/view/5316.html)

[解决boot和cloud版本不一致的问题](https://blog.csdn.net/kxj19980524/article/details/87860876)

[spring config入门学习](https://blog.csdn.net/qazwsxpcm/article/details/88578076)

[spring config入门学习2](https://blog.csdn.net/qazwsxpcm/article/details/88803428?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_title~default-0.control&spm=1001.2101.3001.4242)

### gateway

[gateway过滤策略](https://blog.csdn.net/rain_web/article/details/102469745)

```
https://blog.csdn.net/rain_web/article/details/102469745
```

