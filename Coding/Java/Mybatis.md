# Mybatis

Mybatis是优秀的持久层框架，用于简化JDBC开发。

它支持自定义SQL、存储过程以及高级映射。

MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集的工作。

MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。

- [Mybatis File](https://mybatis.org/mybatis-3/zh/index.html)
- 框架相当于半成品软件，是一套可重用的、通用的、软件基础代码模型
- 基于框架来构建软件更加高效、规范、通用、可扩展

---

JavaEE三层结构：表现层、业务层、持久层
- 表现层展示页面
- 业务层处理逻辑
- 持久层负责将数据保存到数据库

---

JDBC缺点
1. 硬编码：直接将数据库信息写入到代码中 -- > 配置文件
    - 注册驱动，获取链接
    - SQL语句

2. 操作繁琐 -- > 自动完成
    - 手动设置参数
    - 手动封装结果集

## Mybatis优缺点

优点：
1. 与JDBC相比，减少了50%以上的代码量。

2. MyBatis是最简单的持久化框架，小巧并且简单易学。

3. MyBatis灵活，不会对应用程序或者数据库的现有设计强加任何影响，SQL写在XML里，从程序代码中彻底分离，降低耦合度，便于统一管理和优化，可重用。

4. 提供XML标签，支持编写动态SQL语句（XML中使用if choose trim foreach）。

5. 提供映射标签，支持对象与数据库的ORM字段关系映射（在XML中配置映射关系，也可以使用注解）。

缺点：

1. SQL语句的编写工作量较大，尤其是字段多、关联表多时，更是如此，需要编写SQL语句能力。

2. SQL语句依赖于数据库，导致数据库移植性差，不能随意更换数据库。 

## Mybatis快速开发流程

1. 创建数据库表和模块，导入坐标，定义POJO类
2. 编写Mybatis核心配置文件
3. 编写SQL映射文件
4. 加载Mybatis核心配置文件，获取SqlSessionFactory对象
5. 获取SqlSession
6. 执行SQL语句
7. 释放资源

## Mapper代理开发

1. 定义与SQL映射文件同名的Mapper接口，并将Mapper接口和SQL映射文件放在同一目录下
2. 设置SQL映射文件的`namespace`属性为Mapper接口全限定名
3. 在Mapper接口中定义方法，方法名就是SQL映射文件中sql语句中的id，并保持参数类型和返回值类型一致
4. 通过`SqlSession`的`getMapper`方法获取Mapper接口的代理对象，再调用对应方法执行sql

## Mapper--SQL

### 字段不匹配

数据库表的字段名称和实体类的属性名不一样，不能自动封装
- 起别名（as）
    - 缺点：每次查询都要定义
    - sql片段封装
        - 不灵活

- resultMap
    1. 定义`<resultMap>`标签
    2. 在sql语句中，使用resultMap属性替换resultType属性

### 参数占位符

1. #{}：将其替换为?，防止SQL注入
2. ${}：直接拼sql，会存在SQL注入问题
- 参数传递都使用#
- 动态设置表名、列名时，只能使用$进行拼接，可能导致SQL注入

### 特殊字符处理

1. 转义字符
2. CDATA区

## 与Hibernate区别

1. Mybatis是半自动ORM框架，sql需要自己写在xml里面，灵活性高。

2. Hibernate是全自动ORM框架，不需要自己写sql语句，代码简便，数据库可移植性高（换数据库影响小） 