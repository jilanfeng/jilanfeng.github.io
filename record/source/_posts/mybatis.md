---
title: mybatis
date: 2023-05-28 23:59:23
tags:
---


## 简述Mybatis 的插件运行原理，以及如何编写一个插件
``` bash
mybatis 仅可以编写针对ParameterHandler ，ResultSetHandler, StatementHandler 、 Executor 这4种接口的 插件。Mybatis 使用JDK 
动态代理，为需要拦截的接口生成代理对象以实现接口方法拦截功能，每当执行这4种接口对象的方法时，就会进入拦截方法，具体就是InvovationHandler 的invoker() 方法
当然，只会拦截哪些你指定需要拦截的方法

编写插件： 实现Mybatis 的Inteceptor 接口并复写intercept() 方法，然后再给插件编写注解，执行要拦截哪一个接口的哪些方法即可，记住，别忘了在配置文件中配置你编写的插件，
```

## 什么是mybatis 的接口绑定？ 有哪些实现方式？
``` bash
接口绑定，就是mYbatis 中任意定义接口，然后把接口里面的方法 和sql 语句绑定，我们直接调用接口方法就可以，这样比起原来的SqlSession 提供的方法我们可以有更加灵活的选择和设置
```

## Mybatis 的一级缓存。二级缓存?
``` bash
1) 一级缓存：基于PrepetualCache 的HashMap 本地缓存，其存储作用域为Session，当Session flush 或 Close 之后，该session 中的所有Cache 就将
清空，默认打开一级缓存
2) 二级缓存与一级缓存其机制相同，默认就是采用PerpetualCache ,HashMap 存储，不同在与其作用与为Mapper（namespace) SessionFactory,并且可以自定义存储源，如Encache,默认
不打开二级缓存，要开启二级缓存，使用二级缓存属性类需要实现Serialize序列化接口（可用来保存对象的状态） ，可在它的映射文件中配置。
3) 对于缓存更新机制，当某一个作用域（一级缓存 Session / 二级缓存NameSpaces) 的进行C/U/D 操作后，默认该作用域下所有Select 中的缓存将被clear
```

## mybatis 是否支持延迟加载？ 如果 支持，它的实现原理是什么
``` bash
mybatis 仅支持association关联对象和collection 关联集合对象的延迟加载，association 指的就是一对一，collection 指的是一对多查询。在Mybatis 配置文件中，可以配置是否启用延迟加载
layLoadingEnabled = true | false
```

## mybatis 实现一对多有几种方式，怎么操作的？
``` bash
有联合查询和嵌套查询 。联合查询就是几个表联合，只查询一次，通过在resultMap 里面的collection 节点配置一对多的类就可以完成；嵌套查询是先查一个表，
根据这个表里面的结果的外键Id ,再去另外一个表里面查询数据，也是通过配置Collection ，但另外一个表的查询通过select节点配置
```

## XMl 映射文件中，除了常见的select | insert | update | delete  标签之外，还有哪些标签？
``` bash
答： 加上动态sql 的9个标签，其中为sql 片段的标签，通过标签引入sql 片段，为不支持自增的主键生成策略标签
```

## mybatis 动态sql 有什么用？执行原理？ 有哪些动态SQL?
``` bash
mybatis 动态sql 可以在XML 映射文件内，以标签的形式编写动态sql ,执行原理是根据表达式的值完成逻辑判断并动态拼接sql 的功能
```

## Mybaits 提供9种动态sql 标签：
``` bash
trim | where | forEach  | set | if | choose | when | otherwise | bind 
```

## 如何获取自动生成的主键值
``` bash
insert 方法总是返回一个int  值，这个值就是插入的行数，如果采用自增长策略，自动生成的键值在insert 方法执行完成后可以被设置到传入的参数对象种
```

## mybatis 是如何进行分页的？分页插件的原理是什么？
``` bash
mybatis   对象进行分页，他是针对ResultSet 结果集执行的内存分页，而非物理分页，可以在sql内直接书写带有物理分页的参数来完成物理分页功能，也可以使用分页插件来完成物理分页

分页插件的基本原理就是使用mybatis 提供的插件的接口，实现自定义插件，在插件的拦截方法内拦截待执行的sql 然后执行的是sql，然后重写sql ，根据dialect 方言，添加对应的物理分页语句和物理分页参数
```

## 什么是mybatis
``` bash
mybaits是一个半ORM (对象关系映射)框架，它内部封装了JDBC ,开发是只需要关注SQL 语句本身，不需要花费精力去处理加载驱动，创建连接，创建statement 等繁杂过程。程序员之恶极编写原生态SQL ,可以严格控制sql执行性能，灵活度高
2.mybatis 可以使用XML 或注解来配置和映射原生信息，将pojo映射成数据库的记录，避免了几乎所有的JDBC代码和手动设置参数以及获取结果集。

通过xml 文件或注解的方式将要执行的各种statement 配置起来，并通过java 对象和statemnet 中sql的动态参数 进行映射生成最终执行的sql语句，， 最终执行的sql语句，最后有mybatis 框架执行sql 并将结果映射为Java对象并返回。（从执行sql 到返回result 的过程）
```

## mybatis 的 优点：
``` bash
基于sql语句编程，相当灵活，不会对应用程序或者数据库的现有设计造成影响，sql 写在xml 里，解除sql 与程序代码 的耦合，边喝统一管理；提供xml 标签，支持编写动态SQL 语句，并可重用
与JDBC 相比，减少50% 以上的代码两，消除了JDBC 大量的冗余代码，不需要手动开关连接；
3.很好的与各种数据库兼容（因为Mybatis 使用JDBC 来连接数据库，所以只要JDBC 支持的数据库mybatis 都支持）
4.能够与spring很好的集成
5.提供映射标签，支持对象与数据库的orm 支持映射；提供对象关系映射标签，支持对象关系组件维护
```


## mybatis 的缺点：
``` bash
1.sql 语句的编写工作量较大，尤其当字段多，关联表多时，对开发人呀u你编写sql语句的工地有一定要求。
2.sql 语句依赖于数据库，导致数据库移植性差，不能随意跟换数据库
```