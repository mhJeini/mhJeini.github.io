1. MYSQL 数据库服务器性能分析的方法命令有哪些？
Show status，一些值得监控的变量值：

>Bytesreceived和Bytessent和服务器之间来往的流量。 Com_*服务器正在执行的命令。 Created_*在查询执行期限间创建的临时表和文件。 Handler_*存储引擎操作。 Select_*不同类型的联接执行计划。 Sort_*几种排序信息。

Show profiles 是MySql用来分析当前会话SQL语句执行的资源消耗情况。

# 2. MySQL 中记录货币用什么字段类型比较好？
货币在数据库中MySQL常用Decimal和Numric类型表示，这两种类型被MySQL实现为同样的类型。他们被用于保存与金钱有关的数据。

salary DECIMAL(9,2)，9(precision)代表将被用于存储值的总的小数位数，而2(scale)代表将被用于存储小数点后的位数。存储在salary列中的值的范围是从-9999999.99到9999999.99。

DECIMAL和NUMERIC值作为字符串存储，而不是作为二进制浮点数，以便保存那些值的小数精度。

# 3. 什么是内连接、外连接、交叉连接、笛卡尔积？
内连接（inner join）： 取得两张表中满足存在连接匹配关系的记录。

外连接（outer join）： 取得两张表中满足存在连接匹配关系的记录，以及某张表（或两张表）中不满足匹配关系的记录。

交叉连接（cross join）： 显示两张表所有记录一一对应，没有匹配关系进行筛选，也被称为：笛卡尔积。

# 4. Mysql 中 binlog 有几种录入格式？分别有什么区别？
Mysql中有三种格式statement，row和mixed。

statement： 每一条会修改数据的sql都会记录在binlog中。不需要记录每一行的变化，减少了binlog日志量，节约了IO，提高性能。由于sql的执行是有上下文的，因此在保存的时候需要保存相关的信息，同时还有一些使用了函数之类的语句无法被记录复制。

row： 不记录sql语句上下文相关信息，仅保存哪条记录被修改。记录单元为每一行的改动，基本是可以全部记下来但是由于很多操作，会导致大量行的改动(比如alter table)，因此这种模式的文件保存的信息太多，日志量太大。

mixed： 一种折中的方案，普通操作使用statement记录，当无法使用statement的时候使用row。

# 5. MySQL 中 InnoDB 引擎 4大特性，了解过吗？
插入缓冲（insert buffer）

二次写（double write）

自适应哈希索引（ahi）

预读（read ahead）

# 6. 索引有哪几种类型？
主键索引: 数据列不允许重复，不允许为NULL，一个表只能有一个主键。

唯一索引: 数据列不允许重复，允许为NULL值，一个表允许多个列创建唯一索引。

普通索引: 基本的索引类型，没有唯一性的限制，允许为NULL值。

全文索引：是目前搜索引擎使用的一种关键技术，对文本的内容进行分词、搜索。

覆盖索引：查询列要被所建的索引覆盖，不必读取数据行。

组合索引：多列值组成一个索引，用于组合搜索，效率大于索引合并。

# 7. MySQL 中创建索引有三种方式？
执行CREATE TABLE时创建索引

```sql
CREATE TABLE `employee` (
`id` int(11) NOT NULL,
`name` varchar(255) DEFAULT NULL,
`age` int(11) DEFAULT NULL,
`date` datetime DEFAULT NULL,
`sex` int(1) DEFAULT NULL,
PRIMARY KEY (`id`),
KEY `idx_name` (`name`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

使用ALTER TABLE命令添加索引

```sql
ALTER TABLE table_name ADD INDEX index_name (column);
```
使用CREATE INDEX命令创建

```sql
CREATE INDEX index_name ON table_name (column);
```
# 8. 什么是覆盖索引、回表等，了解过吗？
覆盖索引：查询列要被所建的索引覆盖，不必从数据表中读取，换句话说查询列要被所使用的索引覆盖。

回表：二级索引无法直接查询所有列的数据，所以通过二级索引查询到聚簇索引后，再查询到想要的数据，这种通过二级索引查询出来的过程，就叫做回表。

# 9. B+树在满足聚簇索引和覆盖索引的时候不需要回表查询数据？
在B+树的索引中，叶子节点可能存储了当前的key值，也可能存储了当前的key值以及整行的数据，这就是聚簇索引和非聚簇索引。在InnoDB中，只有主键索引是聚簇索引，如果没有主键，则挑选一个唯一键建立聚簇索引。如果没有唯一键，则隐式的生成一个键来建立聚簇索引。

当查询使用聚簇索引时，在对应的叶子节点，可以获取到整行数据，因此不用再次进行回表查询。

# 10. 非聚簇索引一定会回表查询吗？
不一定，如果查询语句的字段全部命中了索引，那么就不必再进行回表查询。

举个简单例子，假设在用户表上建立索引，那么当进行

```sql
select age from user where age < 20;
```
查询时，在索引的叶子节点上，已经包含了age信息，不会再次进行回表查询。

# 11. 组合索引是什么？为什么需要注意组合索引中的顺序？
组合索引是指用户可以在多个列上建立索引，这种索引叫做组合索引。

因为InnoDB引擎中的索引策略的最左原则，所以需要注意组合索引中的顺序。

# 12. 什么是最左前缀原则？
最左前缀原则就是最左优先，在创建多列索引时，要根据业务需求，where子句中使用最频繁的一列放在最左边。

当创建一个组合索引的时候，如（k1,k2,k3），相当于创建了（k1）、(k1,k2）和（k1,k2,k3）三个索引，这就是最左匹配原则。。

# 13. 从锁的类别角度讲，MySQL 都有哪些锁？
从锁的类别上来讲，有共享锁和排他锁。

共享锁： 又叫做读锁。当用户要进行数据的读取时，对数据加上共享锁。共享锁可以同时加上多个。

排他锁： 又叫做写锁。当用户要进行数据的写入时，对数据加上排他锁。排他锁只可以加一个，他和其他的排他锁，共享锁都相斥。

# 14. MySQL 中 InnoDB 引擎的行锁是怎么实现的？
基于索引来完成行锁的。

```sql
select * from user where id = 766 for update;
```
for update可以根据条件来完成行锁锁定，并且id是有索引键的列，如果id不是索引键那么InnoDB将实行表锁。

# 15. 视图常见使用场景有哪些？
1、重用SQL语句；

2、简化复杂的SQL操作。

3、使用表的组成部分而不是整个表；

4、保护数据；

5、更改数据格式和表示。视图可返回与底层表的表示和格式不同的数据。

# 16. count(1)、count(*) 与 count(列名) 有什么区别？
执行效果上来说：

count(*)包括了所有的列，相当于行数，在统计结果的时候，不会忽略列值为NULL。

count(1)包括了忽略所有列，用1代表代码行，在统计结果的时候，不会忽略列值为NULL。

count(列名)只包括列名那一列，在统计结果的时候，会忽略列值为空（这里的空不是只空字符串或者0，而是表示null）的计数，即某个字段值为NULL时，不统计。

执行效率上来说：

列名为主键，count(列名)会比count(1)快；而列名不为主键，count(1)会比count(列名)快。

如果表多个列并且没有主键，则count(1)的执行效率优于count(*) ，反之如果有主键，则select count（主键）的执行效率最优。

如果表只有一个字段，则select count(*)最优。

实例分析：

```shell
mysql> create table counttest(name char(1), age char(2));
Query OK, 0 rows affected (0.03 sec)

mysql> insert into counttest values
-> ('a', '14'),('a', '15'), ('a', '15'),
-> ('b', NULL), ('b', '16'),
-> ('c', '17'),
-> ('d', null),
->('e', '');
Query OK, 8 rows affected (0.01 sec)
Records: 8  Duplicates: 0  Warnings: 0

mysql> select * from counttest;
+------+------+
| name | age  |
+------+------+
| a    | 14   |
| a    | 15   |
| a    | 15   |
| b    | NULL |
| b    | 16   |
| c    | 17   |
| d    | NULL |
| e    |      |
+------+------+
8 rows in set (0.00 sec)

mysql> select name, count(name), count(1), count(*), count(age), count(distinct(age))
-> from counttest
-> group by name;
+------+-------------+----------+----------+------------+----------------------+
| name | count(name) | count(1) | count(*) | count(age) | count(distinct(age)) |
+------+-------------+----------+----------+------------+----------------------+
| a    |           3 |        3 |        3 |          3 |                    2 |
| b    |           2 |        2 |        2 |          1 |                    1 |
| c    |           1 |        1 |        1 |          1 |                    1 |
| d    |           1 |        1 |        1 |          0 |                    0 |
| e    |           1 |        1 |        1 |          1 |                    1 |
+------+-------------+----------+----------+------------+----------------------+
5 rows in set (0.00 sec)
```
# 17. 存储过程有哪些优缺点？
优点：

1、存储过程是一个预编译的代码块，执行效率比较高。

2、存储过程在服务器端运行，减少客户端的压力。

3、允许模块化程序设计，只需要创建一次过程，以后在程序中就可以调用该过程任意次，类似方法的复用。

4、一个存储过程替代大量T_SQL语句 ，可以降低网络通信量，提高通信速率。

5、可以一定程度上确保数据安全。

缺点：

1、调试麻烦。

2、可移植性不灵活。

3、重新编译问题。

# 18. 什么是触发器，有什么作用？
触发器是一种特殊类型的存储过程，当使用下面的一种或多种数据修改操作在指定表中对数据进行修改时，触发器会生效：UPDATE、INSERT 或 DELETE。

触发器可以查询其它表，且可以包含复杂的 SQL 语句。它们主要用于强制复杂的业务规则或要求。例如，可以控制是否允许基于顾客的当前帐户状态插入定单。

触发器有助于强制引用完整性，以便在添加、更新或删除表中的行时保留表之间已定义的关系。然而，强制引用完整性的最好方法是在相关表中定义主键和外键约束。如果使用数据库关系图，则可以在表之间创建关系以自动创建外键约束。

# 19. 触发器的使用场景有哪些？
1、可以通过数据库中的相关表实现级联更改。

2、实时监控某张表中的某个字段的更改而需要做出相应的处理。

3、可以用于生成某些业务的编号。

4、注意不要滥用，否则会造成数据库及应用程序的维护困难。

# 20. MySQL 中都有哪些触发器？
MySQL数据库中有六种触发器：

Before Insert

After Insert

Before Update

After Update

Before Delete

After Delete