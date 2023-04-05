# 1. MongoDB 是什么？
MongoDB是由C++语言编写的，是一个基于分布式文件存储的开源数据库系统。再高负载的情况下，添加更多的节点，可以保证服务器性能。MongoDB旨在给Web应用提供可扩展的高性能数据存储解决方案。

MongoDB将数据存储为一个文档，数据结构由键值（key=>value）对组成。MongoDB文档类似于JSON对象。字段值可以包含其他文档，数组及文档数组。

![](../../img/java/interview/mongodb/1.png)

# 2. MongoDB 有哪些特点？
MongoDB是一个面向文档存储的数据库，操作起来比较简单和容易。

你可以在MongoDB记录中设置任何属性的索引（如：FirstName="Sameer"，Address="8 Gandhi Road"）来实现更快的排序。

你可以通过本地或者网络创建数据镜像，这使得MongoDB有更强的扩展性。

如果负载的增加（需要更多的存储空间和更强的处理能力），它可以分布在计算机网络中的其他节点上这就是所谓的分片。

MongoDB支持丰富的查询表达式。查询指令使用JSON形式的标记，可轻易查询文档中内嵌的对象及数组。

MongoDB使用update()命令可以实现替换完成的文档（数据）或者一些指定的数据字段。

MongoDB中的Map/Reduce主要是用来对数据进行批量处理和聚合操作。

Map和Reduce。Map函数调用emit(key,value)遍历集合中所有的记录，将key与value传给Reduce函数进行处理。

Map函数和Reduce函数是使用Javascript编写的，并可以通过db.runCommand或mapreduce命令来执行MapReduce操作。

GridFS是MongoDB中的一个内置功能，可以用于存放大量小文件。

MongoDB允许在服务端执行脚本，可以用JavaScript编写某个函数，直接在服务端执行，也可以把函数的定义存储在服务端，下次直接调用即可。

# 3. 什么是非关系型数据库？
NoSQL是非关系型数据库，NoSQL = Not Only SQL。

关系型数据库采用的结构化的数据，NoSQL采用的是键值对的方式存储数据。

非关系型数据库的显著特点是不使用SQL作为查询语言，数据存储不需要特定的表格模式。

# 4. 常见的 NoSQL 数据库都有哪些？
常见的NoSQL数据库包括：MongoDB、Cassandra、CouchDB、Hypertable、Redis、Riak、HBASE、Memcache等。

# 5. 非关系型数据库有哪些类型？
-Key-Value 存储，eg：Amazon S3

图表，eg：Neo4J

文档存储，eg：MongoDB

基于列存储，eg：Cassandra

# 6. MongoDB 和 CouchDB 有什么区别？
MongoDB和CouchDB都是面向文档的数据库。

MongoDB和CouchDB都是开源NoSQL数据库的最典型代表。除了都以文档形式存储外它们没有其他的共同点。

MongoDB和CouchDB在数据模型实现、接口、对象存储以及复制方法等方面有很多不同。

# 7. MongoDB 中什么是集合？
集合就是一组MongoDB文档。它相当于关系型数据库（RDBMS）中的表这种概念。集合位于单独的一个数据库中。一个集合内的多个文档可以有多个不同的字段。一般来说，集合中的文档都有着相同或相关的目的。

# 8. MongoDB 中 namespace 是什么？
MongoDB存储BSON对象在丛集（collection）中。数据库名字和丛集名字以句点连结起来叫做namespace。

# 9. 如果移除对象属性，该属性是否从存储层中删除？
如果用户移除对象的属性，该属性会从存储层中删除。

用户移除对象的属性之后，对象会重新保存，使用re-save()方法。

# 10. MongoDB 中能否使用日志特征进行安全备份？
MongoDB可以使用日志特征进行安全备份。

# 11. MongoDB 中允许空值 null 存在吗？
对于对象成员而言，是的。然而用户不能够添加空值（null）到数据库丛集（collection）因为空值不是对象，然而用户能够添加空对象{}。

# 12. MongoDB 中更新操作立刻 fsync 到磁盘吗？
更新操作不会立刻fsync到磁盘，磁盘写操作默认是延迟执行的。

写操作可能在两三秒（默认在60秒内）后到达磁盘。例如，如果一秒内数据库收到一千个对一个对象递增的操作，仅刷新磁盘一次。

注意：fsync选项在命令行和经过getLastError_old是有效的。

# 13. 为什么 MongoDB 数据文件体积会很大？
MongoDB采用的预分配空间的方式，是为了防止文件系统碎片。

# 14. MongoDB 中启用备份故障恢复需要多久？
从备份数据库声明主数据库宕机到选出一个备份数据库作为新的主数据库将花费10到30秒时间。

这期间在主数据库上的操作将会失败，包括写入和强一致性读取（strong consistent read）操作。

注意的是在slaveOk模式下，第二数据库上可以执行最终一致性查询（eventually consistent query）。

# 15. MongoDB 中支持事务加锁吗？
MongoDB没有使用传统的锁或者复杂的带回滚的事务，因为它设计的宗旨是轻量，快速以及可预计的高性能。可以把它类比成MySQL MylSAM的自动提交模式。通过精简对事务的支持，性能得到了提升，特别是在一个可能会穿过多个服务器的系统中。

MongoDB 4.0中不提供事务支持。若要获得类似的结果，请使用findOneDupdate()。

# 16. MongoDB 中什么是副本集？
MongoDB中副本集由一组MongoDB实例组成，包括一个主节点多个次节点，MongoDB客户端的所有数据都写入主节点（Primary），副节点从主节点同步写入数据，以保持所有复制集内存储相同的数据，提高数据可用性。

# 17. MongoDB 中什么是聚合？
聚合操作能够处理数据记录并返回计算结果。聚合操作能将多个文档中的值组合起来，对成组数据执行各种操作，返回单一的结果。它相当于SQL中的count(*)组合group by。

对于MongoDB中的聚合操作，应该使用aggregate()方法。

```shell
>db.COLLECTION_NAME.aggregate(AGGREGATE_OPERATION)
```
# 18. MongoDB 中如何实现排序？
MongoDB中的文档排序是通过sort()方法来实现的。sort()方法可以通过一些参数来指定要进行排序的字段，并使用1和-1来指定排序方式，其中1表示升序，而-1表示降序。

```shell
>db.connectionName.find({key:value}).sort({columnName:1})
```
# 19. MongoDB 中什么是 master 或 primary？
MongoDB中primary（master）是当前备份集群（replica set）中负责处理所有写入操作的主要节点/成员。在一个备份集群中，当失效备援（failover）事件发生时，一个另外的成员会变成primary（master）。

注：

Primary和Secondary是对于IDE通道而言的，前者是首要的，后者是次要的。

Master和Slave是相对于同一个IDE通道中的顺序而言的，前者是主盘，后者是从盘。

# 20. MongoDB 中什么是 secondary 或 slave？
MongoDB 中Seconday（Slave）从当前的primary上复制相应的操作。它是通过跟踪复制oplog(local.oplog.rs)来实现的。

注：

Primary和Secondary是对于IDE通道而言的，前者是首要的，后者是次要的。

Master和Slave是相对于同一个IDE通道中的顺序而言的，前者是主盘，后者是从盘。
