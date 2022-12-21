# Mysql

## 数据库三层结构

数据三层为：DBMS—管理多个—数据库DB—包含多个—表scheme

安装Mysql等同于安装数据库管理系统（DBMS，database manage system），
这个管理系统可以管理多个数据库，一个数据库可以创建多个表来保存数据。数据库——普通表的本质仍然是文件。

表由行和列构成：一行为一条数据记录，一行在Java中往往对应一个对象；列为数据库中数据的一项，对应Java中类的一个属性

SQL语句分类：
- DDL：数据定义语句【创建表和数据库】
- DML：数据操作语句【增删改】
- DQL：数据查询语句【查】
- DCL：数据管理语句【管理数据库：如用户权限：grant revoke】

## 数据库操作

创建数据库( []中内容可选 ):

create_sqecification包括`CHARACTER SET`和`COLLATE`
- CHARACTER SET：指定数据库采用字符集，默认utf-8
- COLLATE：指定数据库字符集的校对规则，默认utf8_general_ci（不区分大小写）

```sql
# 创建数据库/表时，可以用反引号'`'规避关键字
CREATE DATABASE [IF NOT EXISTS] db_name
    [create_sqecification[, create_sqecification]...];

CREATE DATABASE [IF NOT EXISTS] db_01 CHARACTER SET utf8 COLLATE utf8_bin;
```
查看、删除数据库
```sql
SHOW DATABASES

SHOW CREATE DATABASES db_name

DROP DATABASE db_name;
```

备份/恢复数据库
```commandline
# 备份
mysqldump -u username -p -B database1 database2 ... databasen > file.sql
# 只有部分表时不带-B参数
mysqldump -u username -p database table1 table2 ... tablen > file.sql
# 恢复1
source file.sql
# 恢复2：直接复制sql文件内容并执行
```

## 表操作

创建表：
1. 字符集默认为数据库字符集
2. 校对规则默认为数据库校对规则
3. 存储引擎
```sql
CREATE TABLE table_name
(
    field1 datatype,
    field2 datatype,
    fieldn datatype,
) character set 字符集 collate 校对规则 engine 存储引擎
```

### Mysql常用数据类型
- [mysql datatype](https://www.runoob.com/mysql/mysql-data-types.html)

BIT(M)：位类型，M指定位数（默认1，范围1～64），在数据库中以二进制串的形式表示

整型数据类型规范：确定能满足需求的情况下，尽量选择占用空间小的类型

字符串 char()和varchar()表示字符数量，中文和英文都按字符存放；
char是定长，每个数据都占用固定分配的大小，varchar是变长，按照实际占用空间来分配
- 数据是定长时使用char（手机号/身份证），数据长度不确定时使用varchar（留言等）

varchar使用时
- utf8最大字符数量 (65535-3) / 3 = 21844
- gbk最大字符数量 (65535-3) / 2 = 32766

时间戳：NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP 
非空，默认值为当前时间，更新数据时更新为当前时间

表修改
```sql
-- 添加列
ALTER TABLE table_name
ADD         (column datatype [DEFAULT expr]
            [, column datatype]...)
-- 修改列
ALTER TABLE table_name
MODIFY      (column datatype [DEFAULT expr]
            [, column datatype]...)
-- 修改列名
ALTER TABLE table_name
CHANGE `name` (column datatype [DEFAULT expr])
-- 删除列
ALTER TABLE table_name
DROP        (column)
-- 查看表的结构
desc table_name;
-- 修改表名
RENAME TABLE table_name to new_name
-- 修改字符集
ALTER TABLE table_name character set to new_set

```

## CRUD [[create](#insert) [read](#select) [update](#update) [delete](#delete)]
> CRUD都是针对表中的记录（即行）的数据进行操作
### INSERT

```sql 
INSERT INTO table_name [(column [, clolumn...])]
VALUES      (value[, value...])
```

1. 插入数据和字段数据的位置需要对应，类型保持相同，且长度符合范围
2. 字符和日期类型数据应包含在单引号中
3. 如果给所有字段插入数据，可以不写字段名称，还可以在values后添加多条数据
4. 有默认值时，不给某个字段值，有默认值则添加

### UPDATE
```sql 
UPDATE  table_name 
        SET  col_name1=expr1 [, col_name2=expr2...]
        [WHERE where_definition]
```
1. set指示要修改哪些列和给予哪些值
2. 不写where则对所有值都修改

### DELETE

```sql 
DELETE  FROM table_name 
        [WHERE where_definition]
```
1. 不写where会将所有记录都删除
2. delete不能删除某一列的值
3. delete仅能删除记录，删除表需要用「DROP TABLE table_name」

### SELECT

```sql
# 基本语法
SELECT [DISTINCT] *|{column1 | expresstion, column2 | expresstion, column3, ...} 
        FROM table_name
        [WHERE Clause]
        [LIMIT N][ OFFSET M];
-- DISTINCT 用于过滤重复的数据（每列都相同的数据才算重复） 
-- column | expresstion可以查询列名或者表达式
-- 使用as来创建列的别名
SELECT column as 别名 FROM table_name;

SELECT column FROM table1
    order by column_ord asc | desc;
-- order by 用作排序，用于排序的列可以是表中的列，也可以是select后指定的列名/别名
-- asc升序（默认），desc 降序
-- order by 通常在select语句末尾

SELECT column1, column2, column3 
    FROM table1
    group by column_grp_list having...;
-- group by 按照指定的列分组
-- group by 可以指定多个列，按照顺序依次分组
-- having后可以指定过滤条件，也可以使用别名过滤
```

where运算符
比较运算符
1. \> < >= <= = != 
2. BETWEEN AND 在某个区间（闭区间）
3. IN(set) 在列表中的值，如in(1, 2, 3)
4. LIKE / NOT LIKE 模糊查询 '%'表示多个字符，'_'表示单个字符
5. IS NULL 判定空
逻辑运算符
1. and 与
2. or  或
3. not 非，如where not(salary > 100)

统计函数 count(\*) | count(列名)：

count(\*)返回满足条件的记录的总行数；
count(列)统计满足条件的某列有多少个，会排除为空的列

求和函数 sum(列名)

平均函数 avg(列名)

最值函数 max(列名) / min(列名)

[mysql函数](https://www.runoob.com/mysql/mysql-functions.html)
- 字符串函数
- 日期函数：使用int保存unix时间戳，再使用from_unixtime()来进行转换
- 加密函数
    1. MD5(str) 为字符串计算一个MD5 32的字符串，常用加密
    2. PASSWORD(str) 加密函数，使用另外一个加密算法
- 流程控制函数
    1. IF(expr1, expr2, expr3) 如果表达式1为真则2，否则3
    2. IFNULL(expr1, expr2) 如果表达式1不为真则返回1，否则返回2（判断是否为空使用 is NULL / is not NULL）
    3. SELECT CASE WHEN expr1 THEN expr2 WHEN expr3 THEN expr4 ELSE expr5 END;
        如果WHEN后面的表达式真，则返回对应的THEN后面的表达式，如果都不为真，则返回ELSE后的表达式

### 查询加强

使用where子句：
1. 在mysql中，日期可以直接比较，后面的时间 > 前面的时间
2. 分页查询
    ```sql
    select ... limit start, rows
    -- limit 表示从start + 1开始取，取出rows行，start从0开始计算
    ```
3. 一个select语句同时有group, having, limit, order，顺序为group by, having,  order by, limit

## 多表查询
多表查询是基于两个或以上的表查询。

【笛卡尔集】默认情况下，同时查询两个表会从第一张表中取出每一行和第二张表的每一行进行组合。
一共返回的记录数为 【第一张表记录数 * 第二张表记录数】。这样多表处理默认返回的结果称为`笛卡尔集`

- 解决方法：使用where+正确的过滤条件（同名的列需要table_name.colname）
- 多表查询条件数量不能小于`表的个数 - 1`，否则会出现笛卡尔集

【自连接】在同一张表的连接查询，即将一张表看作两张表
- 自连表中，需要给表取别名「FROM table as t1，table as t2」
- 列名不明确时使用AS取别名

### 子查询
嵌入在其他sql语句中的select语句，也叫嵌套查询
- 单行子查询：只返回一行数据的子查询语句「col = (select a from...)」
- 多行子查询：返回多行数据的子查询，通常使用in匹配（使用distinct去重）「col in (select a, b, c from ...)」

【子查询做临时表】查询结果也是一张表，可以用子查询来作查询源
```sql
select a,b,c
    from (
        select a1, b1, max(c1) as c2
        from table_name
        group by a1
    ) result, table_name
    where...
```

【all和any】用于解决比【条件下】所有值都【比较】和比【条件下】一个值【比较】
```sql
select a,b,c
    from table_name
    where a > all/any(
        select a1,
        from table_name
    )
```
【多列子查询】查询返回多个列数据的子查询语句
使用(字段1, 字段2) = (select 字段1, 字段2 from ...)
```sql
select a,b,c
    from table_name
    where (a1, b1)(
        select a1, b1
        from table_name
        where ...
    )
```

【蠕虫复制】
```sql
-- 创建大量数据用于测试
insert into table1
        select * from table1
```

【合并查询】
> 使用union合并多条查询语句结果

```sql
select a, b, c from abc where a > 1
union all -- union all 仅简单合并，不会去重
select a, b, c from abc where c = 5

select a, b, c from abc where a > 1
union -- union 会自动去掉重复行且合并记录
select a, b, c from abc where c = 5
```

## 外连接

> 用之前的多表查询只能匹配两表都包括的内容，无法查询单侧包含等其他信息

左外连接：左侧表完全显示，即左侧表没有匹配到右表的内容也会显示

```sql
    select * 
        from table1 
        left join table2 on (condition);
```

右外连接：右侧表完全显示，即右侧表没有匹配到左表的内容也会显示
```sql
    select * 
        from table1 
        right join table2 on (condition);
```

## 约束 constraint

约束用来确保数据库中数据满足特定的规则

1. primary key 主键

    1. 用于唯一的标示表行的数据，定义主键约束后，该列不能重复，且不能为空值
    2. 一张表最多只能有一个主键，可以使用复合主键，复合主键需要每个值都相同才算重复
    3. 两种定义方式：定义列时指定，或在最后定义「primary key (a, b, ...)」

2. not null 不能插入空值

3. unique 不能存在重复值

    1. 如果不指定not null，则unique列可以有多个null
    2. 一个表可以有多个unique列
    3. 如果一个列同时是unique 和 not null修饰的，则等同于主键

4. foreign key 外键
    
    1. 用于约束主表和从表之间的关系，外键约束定义在从表上，主表必须有主键约束或unique约束
    2. 定义外键约束后，要求外键列数据必须在主表的唯一键存在或为null值
    3. 表的类型是innodb才支持外键
    4. 定义外键列和主表对应列必须是同一类型
    ```sql
        create table stu(
            id INT primary key,
            `name` varchar(32) not null default '',
            class_id INT,
            foreign key (class_id) references class(id)
        );
    ```

5. check 

    1. check用于对插入的数据做一定的限制
    2. sql server 和 oracle支持 check， 但mysql5.7还不支持check，只做语法校验

### 自增长

添加数据时，某个值能够自动增长「name type primaey key auto_increment」，按照下列方式添加数据

1. insert into table (a,b,...,n) values (null, 2, 3,...n) null对应自增长a值
2. insert into table (b,...,n) values (2, 3,...n) 自增长a值默认增长
3. insert into table values (null, 2, 3,...n) null对应自增长值

1. 自增长一般和unique或主键约束配合使用
2. 一般是整数型数据使用自增长
3. 默认从1开始，也可以设置表的auto_increment = x
4. 若插入数据时有指定值，则不使用自增长值，使用指定值

## 索引 index

```sql
    -- 索引用来优化查询，提高查询速度，索引本身也会占用空间
    -- 索引优化只针对创建了索引的列有效
    create index table_index on table1 (column)
```

索引原理：
1. 没有索引时，执行查询语句时，进行全表扫描，逐条查询是否满足条件，速度慢
2. 索引会建立二叉树，还有B+树，红黑树等结构
3. 索引会占用磁盘空间，且对增加，修改，删除的性能会有较大影响，因为值变化后索引需要刷新

创建规则
1. 作为查询条件较频繁的字段应创建索引
2. 唯一性太差的字段不适合创建索引
3. 更新太频繁的字段不适合创建索引
4. 不作为筛选条件的字段不适合创建索引

索引类型

1. 主键索引
    > 某列是主键，则该列同时也是主键索引（查询快）

2. 唯一索引 （unique）
    > 某列是唯一的，则该列同时也是唯一索引

3. 普通索引 （index）

4. 全文索引（适用于MyISAM）
    > mysql自带全文索引效果一般，开发中使用全文搜索Solr和ElasticSearch

mysql索引操作方法
```sql
    create table index_table
    (
        id     int,
        `name` varchar(32)
    );

    -- 添加唯一索引使用unique
    create unique index id_index on index_table (id);
    -- 某列值不会重复时，优先考虑使用unique索引
    -- 添加普通索引
    alter table index_table
        add index id_index2 (id);
    -- 添加主键索引可以直接定义主键，或用alter
    alter table index_table
        add primary key (id);

    -- 删除索引
    drop index id_index2 on index_table;
    -- 删除主键索引，相当于取消主键
    alter table index_table
        drop primary key;
    -- 查询是否有索引3种
    show indexes from index_table;
    show index from index_table;
    show keys from index_table;
```

## 事务 transaction

> 事务用于保证数据的一致性，由一组相关的dml语句组成，该组语句作为一个整体，要么全部执行成功，要么全部失败。
> 执行事务操作时，mysql会在表上加锁，防止其他用户修改数据

start transaction：开始事务，还可以使用 set autocommit = off

savepoint：设置保存点

rollback to：回退事务到保存点

rollback：回退全部事务

commit：提交事务，所有操作生效，不能回退（执行时自动删除所有保存点）

回退：回退时会将所有数据返回到事务开始或设置保存点的状态，这之间所有变化失效，包括dml和保存点

提交：执行commit语句后，会确认事务变化，结束事务，删除保存点，释放锁，数据生效。使用commit结束事务后，其他会话将可以看到事务变化后的新数据。

1. 如果不开始事务，默认情况下dml会自动提交，不能回滚
2. 如果开始一个事务，但没有创建保存点，可以使用rollback回到事务开始的状态
3. Innodb存储引擎支持mysql事务，MyISAM不支持

### 隔离级别

> 多个链接开启各自事务操作数据时，数据库系统需要负责隔离操作，以保证各个链接在获取数据时的准确性。
> 隔离级别定义了事务之间的隔离程度

可能引发的问题：
1. 脏读：一个事务读取另一个事务尚未提交的修改时，产生脏读

2. 不可重复读：同一查询在同一事务中多次进行，由于其他事务所提交的`修改或删除`导致返回不一致的结果集，产生不可重复读

3. 幻读：同一查询在同一事务中多次进行，由于其他提交事务所提交的`插入`操作每次返回不同的结果集，产生幻读

隔离级别：
1. 读未提交：会产生脏读，幻读和不可重复读，且不加锁读

2. 读已提交：会产生幻读和不可重复读，且不加锁读

3. 可重复读：不加锁读，不会出现数据问题

4. 可串行化：加锁读，发现有其他事务正在操作时，会等待其他事务结束，不会出现数据问题

隔离级别操作
```sql
-- 查看会话当前隔离级别
select @@tx_isolation;
-- 查看系统当前隔离级别
select @@global.tx_isolation;
-- 设置当前会话隔离级别
set session transaction isolation level read committed;
-- 设置系统当前隔离级别
set global transaction isolation level read uncommitted;
```

### ACID特性

1. 原子性 Automicity：指事务是一个不可分割的单位，事务中的操作要么都发生，要么都不发生

2. 一致性 Consistency：事务必须使数据库从一个一致的状态转换到另一个一致的状态

3. 隔离性 Isolation：多个用户并发访问数据库时，数据库为每一个用户开启的事务不能被其他事务的操作数据干扰，多个并发事务要相互隔离

4. 持久性 Durability：事务一旦提交，对数据库中数据的改变就是用久性的

## mysql表类型和存储引擎

1. MySQL的表类型由存储引擎决定，主要包括MyISAM，innoDB，Memory
2. 共有六种引擎：CSV，MGR_MYISAM，ARCHIVE，MyISAM，innoDB，Memory
3. 分为”事务安全型“，如InnoDB，和”非事务安全型“，其他五种都是第二类

myISAM：不支持事务和外键，访问速度很快，对事务完整性没有要求

InnoDB：具有提交，回滚和崩溃恢复的事务安全，访问速度较慢且数据和索引和数据都要占据较大空间，支持行级锁

Memory：基于哈希的，存储在内存中（断电失效），对临时表有效，访问速度非常快
使用memory的表，关闭服务时会丢失数据，但是表结构还保留

选择方法：
1. 应用不需要事务，只需要CRUD基本操作，则选择MyISAM，速度极快
2. 需要支持事务则选择InnoDB
3. Memory由于没有IO等待，速度极快，但是重启服务将丢失，用于存储变化极快且不太重要的数据
```sql
InnoDB,DEFAULT,"Supports transactions, row-level locking, and foreign keys",YES,YES,YES
MRG_MYISAM,YES,Collection of identical MyISAM tables,NO,NO,NO
MEMORY,YES,"Hash based, stored in memory, useful for temporary tables",NO,NO,NO
BLACKHOLE,YES,/dev/null storage engine (anything you write to it disappears),NO,NO,NO
MyISAM,YES,MyISAM storage engine,NO,NO,NO
CSV,YES,CSV storage engine,NO,NO,NO
ARCHIVE,YES,Archive storage engine,NO,NO,NO
PERFORMANCE_SCHEMA,YES,Performance Schema,NO,NO,NO
FEDERATED,NO,Federated MySQL storage engine,,,
```

## 视图
> 视图是一个虚拟表，内容由查询定义，同真实表一样，视图包含列，数据来自对应真实表

1. 视图根据基表创建，视图是虚拟的表（基表可能有多个）
2. 视图也有列，数据是来自基表
3. 通过视图也可以修改基表的数据，基表改变也会影响视图
4. 视图只是基表的映射，并不会产生实际数据文件
5. 视图中还可以使用视图，数据仍然来自基表

实践：
1. 安全。防止权限不够的人员查看到保密字段

2. 性能。分表存储的数据通常使用外键建立连接，使用join查询效率较低，使用视图直接组合相关的表和字段，可以提高效率

3. 灵活。视图可以结合不同的表来达到重新设计的目的，意味着使用较少的改动就升级了数据表

```sql
create view vn as select...
alter view vn as select...
show create view vn
drop view vn1, vn2
``` 

## 用户管理

不同数据库用户，登录到DBMS后，根据相应的权限，能看到/操作的表，数据库和数据对象都不一样
```sql
-- root查看用户
select distinct * from mysql.user;
-- 创建用户
create user 'xbt'@'localhost' identified by 'pswd';
-- 删除用户
drop user  'xbt'@'localhost';
-- 修改当前用户密码
set password = password ('123321');
-- 修改指定用户密码
set password for 'xbt'@'localhost' = password('xbt');
select password('123321');

-- 用户权限管理
-- 给用户授权（加上identify by表示给已有用户修改密码 和给不存在用户设置密码）
grant [list] on database.table to 'user'@'host' [identify by 'password'];

-- 回收用户权限
revoke [list] on database.table from 'user'@'host';
```

1. 创建用户时，不指定host时为'%'，代表从任何ip都有权限连接数据库
2. 指定时可以用 'name'@'192.168.1.%'，用%代表通配符
3. 删除用户时如果host不是%，则必须明确指定host

