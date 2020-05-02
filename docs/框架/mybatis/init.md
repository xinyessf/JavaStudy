了解mybatis

>MyBatis是支持普通\SQL\查询\，存储过程\和高级映射\的优秀持久层框架。MyBatis消除了几乎所有的JDBC代码和参数的手工设置以及对结果集的检索封装。MyBatis可以使用简单的XML或注解用于配置和原始映射，将接口和Java的POJO（Plain Old Java Objects，普通的Java对象）映射成数库中的记录.JDBC- MyBatis-àHibernate

### 占位符区别

```
动态 sql 是 mybatis 的主要特性之一，在 mapper 中定义的参数传到 xml 中之后，在查询之前 mybatis 会对其进行动态解析。mybatis 为我们提供了两种支持动态 sql 的语法：#{} 以及 ${}。
```

### mybatis提供了注解

```
Mybatis提供了增删改查注解、@select @delete @update
```

