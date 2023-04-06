# 1. 超键、候选键、主键、外键分别是什么？
超键：在关系模式中，能唯一知标识元组的属性集称为超键。

候选键：是最小超键，即没有冗余元素的超键。

主键：数据库表中对储存数据对象予以唯一和完整标识的数据列或属性的组合。一个数据列只能有一个主键，且主键的取值不能缺失，即不能为空值（Null）。

外键：在一个表中存在的另一个表的主键称此表的外键。

# 2. MySQL 中 SQL 约束有哪几种？
NOT NULL: 约束字段的内容一定不能为NULL。

UNIQUE: 约束字段唯一性，一个表允许有多个 Unique 约束。

PRIMARY KEY: 约束字段唯一，不可重复，一个表只允许存在一个。

FOREIGN KEY: 用于预防破坏表之间连接的动作，也能防止非法数据插入外键。

CHECK: 用于控制字段的值范围。

# 3. MySQL 中 varchar(50) 中 50 是什么涵义？
50表示的含义是字段最多存放50个字符。

例如：varchar(50)和varchar(200)存储 "jay"字符串所占空间是一样的，后者在排序时会消耗更多内存。

# 4. MySQL 中 drop、delete 与 truncate 有什么区别？
| -	    | delete 	              | truncate 	       | drop                       |
|-------|-----------------------|------------------|----------------------------|
| 类型	   | DML	                  | DDL	             | DDL                        |
| 回滚	   | 可回滚	                  | 不可回滚	            | 不可回滚                       |
| 删除内容	 | 表结构还在，删除表的全部或者一部分数据行	 | 表结构还在，删除表中的所有数据	 | 从数据库中删除表，所有的数据行，索引和权限也会被删除 |
| 删除速度	 | 删除速度慢，逐行删除	           | 删除速度快	           | 删除速度最快                     |

# 5. MySQL 中 UNION 和 UNION ALL 有什么区别？
Union： 对两个结果集进行并集操作，不包括重复行，同时进行默认规则的排序； 

Union All： 对两个结果集进行并集操作，包括重复行，不进行排序；

UNION的效率高于UNION ALL。

# 6. MySQL 中主键使用自增 ID 还是 UUID？
如果是单机的话，选择自增ID；如果是分布式系统，优先考虑UUID，但是建议公司自身有一套分布式唯一ID生产方案。

自增ID：数据存储空间小，查询效率高。但是如果数据量过大,会超出自增长的值范围，多库合并，也有可能有问题。

UUiD：适合大量数据的插入和更新操作，但是它无序的，插入数据效率慢，占用空间大。

# 7. MySQL 中 CPU占用率飙升，如何处理？
排查过程：

使用top命令观察，确定是mysqld导致还是其他原因。

如果是mysqld导致的可以使用show processlist命令查看session情况，确定是不是有消耗资源的sql语句正在运行。

找出消耗高的sql语句，看看执行计划是否准确，索引是否缺失，数据量是否太大。

处理：

kill掉这些线程，同时观察cpu使用率是否下降，进行相应的调整，比如说加索引、改 sql、改内存参数，之后重新执行这些SQL语句。

其他情况：

可能是每个sql消耗资源并不多，但是突然之间，有大量的session连进来导致cpu飙升，这种情况就需要跟应用一起来分析为何连接数会激增，再做出相应的调整，比如说限制连接数等。

# 8. MySQL 中 DATETIME 和 timestamp 有什么区别？
DATETIME的日期范围是1001——9999年；TIMESTAMP的时间范围是1970——2038年。

DATETIME存储时间与时区无关；TIMESTAMP存储时间与时区有关，显示的值也依赖于时区。

DATETIME的存储空间为8字节；TIMESTAMP的存储空间为4字节。

DATETIME的默认值为null；TIMESTAMP的字段默认不为空（not null），默认值为当前时间（CURRENT_TIMESTAMP）。

# 9. MySQL 中 TEXT 数据类型的最大长度？
TINYTEXT：256 bytes

TEXT：65,535 bytes(64kb)

MEDIUMTEXT：16,777,215 bytes(16MB)

LONGTEXT：4,294,967,295 bytes(4GB)

# 10. 如何监控数据库？慢日志是如何查询的？
监控数据库的工具有很多，例如zabbix、lepus等，比较常用的是zabbix。

# 11. MySQL 中是否支持 emoji 表情存储，若不支持，如何操作？
MySQL中是默认不支持emoji表情存储。

需要更换数据库表中字符集utf8-->utf8mb4。

# 12. MySQL 中如何获取当前日期？
```sql
SELECT CURRENT_DATE();
```

# 13. SQL 连接查询时 on 和 where 有什么区别？
SQL中的连接查询分为3种，cross join（交叉连接）、inner join（内连接）和outer join（外连接）。

在cross join和inner join中，筛选条件放在on后面还是where后面是没区别的，在编写这两种连接查询的时候，只用on不使用where也没有什么问题。

on筛选和where筛选的差别只是针对outer join，也就是平时最常使用的left join和right join。

以左连接为例：

1、用on的时候，只对右表做筛选条件，而左表不受控制

2、用where的时候，对临时表的组合后的结果进行筛选，所以对左右表都是有作用的。

总的来说outer join的执行过程分为4步：

1、先对两个表执行交叉连接(笛卡尔积)

2、应用on筛选器

3、添加外部行

4、应用where筛选器

# 14. MySQL 中如何去掉重复数据记录？
MySQL数据库表中可能存在重复数据，有些情况是允许重复数据存在的，有些情况是不允许的，这个时候需要查找并删除这些重复数据，以下是具体的处理方法。

方法一：防止表中出现重复数据

当表中未添加数据时，可以在表中设置指定的字段为PRIMARY KEY（主键）或者UNIQUE（唯一）索引来保证数据的唯一性。

例如在用户信息表中员工号no不允许重复，需设置员工号no为主键，且默认值不能为NULL。

```sql
CREATE TABLE user
(
no CHAR(12) NOT NULL,
name CHAR(20),
sex CHAR(10),
PRIMARY KEY (no)
);
```

方法二：过滤删除重复值

对于表中原有数据去掉重复的，需要经过重复数据查找、过滤以及删除等步骤。

1. 统计重复数据

```shell
mysql> SELECT COUNT(*) as sex,no
-> FROM user GROUP BY no
-> HAVING repetitions > 1;
```
以上查询语句将返回student表中重复的记录数。

2. 过滤重复数据

如果读取不重复的数据可以在SELECT语句中使用DISTINCT关键字来过滤重复数据。

```shell
mysql> SELECT DISTINCT no FROM user;
```
也可以使用GROUP BY来读取表中不重复的数据。

```shell
mysql> SELECT no FROM user GROUP BY (no);
```
3. 删除重复数据

删除数据表中重复数据，可以使用以下SQL语句：

```shell
mysql> CREATE TABLE tmp SELECT no, name, sex FROM user GROUP BY (no, sex);
mysql> DROP TABLE user;
mysql> ALTER TABLE tmp RENAME TO student;
```
也可以在数据表中添加INDEX（索引）和 PRIMAY KEY（主键）来删除表中的重复记录，方法如下：

```shell
mysql> ALTER IGNORE TABLE user
-> ADD PRIMARY KEY (no);
```
# 15. MySQL 中如何有效的删除一个大表？
1、复制表结构，切勿复制数据。

2、创建硬链接减少mysql DDL时间，加快锁释放。

如果不知道的存储位置，可使用show variables like "datadir";命令查看数据存储位置。

```shell
ln jingxuan.frm jingxuan.frm.bak
ln jingxuan.ibd jingxuan.ibd.bak
```
3、删除指定的表

DROP TABLE "表格名"; 2G的数据量，删除只用了大概3秒左右。

4、修改表名删除的表名

将复制的表，表名修改为删除的表名即可。

5、删除物理文件，使用truncate分段删除文件，避免IO hang。

切记物理文件不可直接删除，直接操作会导致磁盘IO和CPU利用率升高，使用truncate来进行删除操作。

```shell
truncate -s 2G jingxuan.ibd.bak
```
# 16. MySQL 中 having 和 where 有什么区别？
having子句和where都是设定条件筛选的语句。

其中having与where的区别主要有以下几方面：

1、having是在分组后对数据进行过滤，而where是在分组前对数据进行过滤；

2、having后面可以使用聚合函数，而where后面不可以使用聚合。

3、在查询过程中执行顺序：from>where>group（含聚合）>having>order>select。

4、聚合语句（sum、min、max、avg、count）要比having子句优先执行，而where子句在查询过程中执行优先级别优先于聚合语句（sum、min、max、avg、count）。

where子句

```sql
select sum(num) as rmb from order where id>10
```
只有先查询出id大于10的记录才能进行聚合语句

having子句

```sql
select reports,count(*)  from employees group by reports having count(*) > 4
```
having条件表达示为聚合语句，having子句查询过程执行优先级别低于聚合语句。如果把上面的having换成where则会出错，统计分组数据时用到聚合语句。

对分组数据再次判断时要用having。如果不用这些关系就不存在使用having。直接使用where就行了。having是用来弥补where在分组数据判断时的不足。所以where执行优先级别要快于聚合语句。

聚合函数

例如SUM、COUNT、MAX、AVG等。这些函数和其它函数的根本区别就是它们一般作用在多条记录上。

having子句可以直接筛选成组后的各组数据，也可以在聚合后对组记录进行筛选，而where子句在聚合前先筛选记录，也就是说作用在group by子句和having子句前。

# 17. 说说对 SQL 语句优化有哪些方法？
1）Where子句中：where表之间的连接必须写在其他Where条件之前，那些可以过滤掉最大数量记录的条件必须写在Where子句的末尾.HAVING最后。

2）用EXISTS替代IN、用NOT EXISTS替代NOT IN。

3） 避免在索引列上使用计算。

4）避免在索引列上使用IS NULL和IS NOT NULL。

5）对查询进行优化，应尽量避免全表扫描，首先应考虑在 where 及 order by 涉及的列上建立索引。

6）应尽量避免在 where 子句中对字段进行 null 值判断，否则将导致引擎放弃使用索引而进行全表扫描。

7）应尽量避免在 where 子句中对字段进行表达式操作，这将导致引擎放弃使用索引而进行全表扫描。

# 18. 如何通俗地理解三个范式？
第一范式：1NF是对属性的原子性约束，要求属性具有原子性，不可再分解；

第二范式：2NF是对记录的惟一性约束，要求记录有惟一标识，即实体的惟一性；

第三范式：3NF是对字段冗余性的约束，即任何字段不能由其他字段派生出来，它要求字段没有冗余。

范式化设计优缺点

优点：可以尽量得减少数据冗余，使得更新快，体积小。

缺点：对于查询需要多个表进行关联，减少写得效率增加读得效率，更难进行索引优化。

反范式化

优点：可以减少表得关联，可以更好得进行索引优化。

缺点：数据冗余以及数据异常，数据得修改需要更多的成本。

# 19. MySQL 调优数据库都有哪些方法？
1、选取最适用的字段属性，尽可能减少定义字段宽度，尽量把字段设置NOTNULL，例如’省份’、’性别’最好适用ENUM。

2、使用连接(JOIN)来代替子查询。

3、适用联合(UNION)来代替手动创建的临时表。

4、事务处理。

5、锁定表、优化事务处理。

6、适用外键，优化锁定表。

7、建立索引。

8、优化查询语句。

# 20. 业务实践中如何优化 MySQL？
1、SQL语句及索引的优化

2、数据库表结构的优化

3、系统配置的优化

4、硬件的优化
