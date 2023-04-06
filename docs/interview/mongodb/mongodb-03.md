# 1. MongoDB 中如何创建一个新的数据库？
MongoDB用use+数据库名称的方式来创建数据库。use会创建一个新的数据库，如果该数据库存在，则返回这个数据库。

# 2. MongoDB 和 MySQL 之间最基本的区别是什么？
MongoDB和MySQL两者都是免费开源的数据库。MongoDB和MySQL有许多基本差别包括数据的表示（data representation），查询，关系，事务，schema的设计和定义，标准化（normalization），速度和性能。

通过比较MongoDB和MySQL，实际上是在比较关系型和非关系型数据库，主要是数据存储结构不同。

# 3. 为什么需要使用 MongoDB？
架构简单；

没有复杂的连接；

深度查询能力，MongoDB支持动态查询；

容易调试；

容易扩展；

不需要转化/映射应用对象到数据库对象；

使用内部内存作为存储工作区，以便更快的存取数据。

# 4. MongoDB 主要使用在什么场景？
MongoDB的应用已经渗透到各个领域，比如游戏、物流、电商、内容管理、社交、物联网、视频直播等，以下是几个实际的应用案例：

游戏场景：使用MongoDB存储游戏用户信息，用户的装备、积分等直接以内嵌文档的形式存储，方便查询、更新。

物流场景：使用MongoDB存储订单信息，订单状态在运送过程中会不断更新，以MongoDB内嵌数组的形式来存储，一次查询就能将订单所有的变更读取出来。

社交场景：使用MongoDB存储用户信息，以及用户发表的朋友圈信息，通过地理位置索引实现附近的人、地点等功能。

物联网场景：使用MongoDB存储所有接入的智能设备信息，以及设备汇报的日志信息，并对这些信息进行多维度的分析。

视频直播：使用MongoDB存储用户信息、礼物信息等。

# 5. MongoDB 中的命名空间是什么含义？
MongoDB内部有预分配空间的机制，每个预分配的文件都用0进行填充。

数据文件每新分配一次，它的大小都是上一个数据文件大小的2倍，每个数据文件最大2G。

MongoDB每个集合和每个索引都对应一个命名空间，这些命名空间的元数据集中在16M的*.ns文件中，平均每个命名占用约628字节，也即整个数据库的命名空间的上限约为24000。

如果每个集合有一个索引（比如默认的_id索引），那么最多可以创建12000个集合。如果索引数更多，则可创建的集合数就更少了。同时，如果集合数太多，一些操作也会变慢。

要建立更多的集合的话，MongoDB也是支持的，只需要在启动时加上“--nssize”参数，这样对应数据库的命名空间文件就可以变得更大以便保存更多的命名。这个命名空间文件（.ns文件）最大可以为2G。

每个命名空间对应的盘区不一定是连续的。与数据文件增长相同，每个命名空间对应的盘区大小都是随分配次数不断增长的。目的是为了平衡命名空间浪费的空间与保持一个命名空间数据的连续性。

需要注意的一个命名空间freelist，这个命名空间用于记录不再使用的盘区（被删除的Collection或索引）。每当命名空间需要分配新盘区时，会先查看freelist，这个命名空间用于记录不再使用的盘区（被删除的Collection或索引）。每当命名空间需要分配新盘区时，会先查看freelist，这个命名空间用于记录不再使用的盘区（被删除的Collection或索引）。每当命名空间需要分配新盘区时，会先查看freelist是否有大小合适的盘区可以使用，如果有就回收空闲的磁盘空间。

# 6. 哪些语言支持 MongoDB？
C、C++、C#、Java、Node.js、Perl、PHP等。

# 7. MongoDB 中如何查看数据库列表？
MongoDB中查看数据库列表，使用命令：

```shell
show dbs
```

# 8. MongoDB 中的分片是什么含义？
分片是将数据水平切分到不同的物理节点。当应用数据越来越大的时候，数据量也会越来越大。当数据量增长时，单台机器有可能无法存储数据或可接受的读取写入吞吐量。利用分片技术可以添加更多的机器来应对数据量增加以及读写操作的要求。

# 9. MongoDB 中什么是复制？
复制是将数据同步到多个服务器的过程，通过多个数据副本存储到多个服务器上增加数据可用性。复制可以保障数据的安全性，灾难恢复，无需停机维护（如备份、重建索引、压缩），分布式读取数据。

# 10. MongoDB 中如何在集合中插入一个文档？
将数据插入MongoDB集合中，需要使用insert()或save()方法。

```shell
>db.collectionName.insert({"key":"value"})>db.collectionName.save({"key":"value"})
```
# 11. MongoDB 中如何除去一个数据库？
MongoDB的dropDatabase()命令用于删除已有数据库：

```shell
>db.dropDatabase()
```
#12. MongoDB 中如何创建一个集合？
在MongoDB中，创建集合采用db.createCollection(name, options)方法。options是一个用来指定集合配置的文档。

```shell
>db.createCollection("collectionName")db.createCollection() - MongoDB Manual>db.createCollection()
```
# 13. MongoDB 中如何查看已创建的集合？
可以使用show collections查看当前数据库中的所有集合清单：

```shell
>show collections
```
# 14. MongoDB 中如何删除一个集合？
MongoDB利用db.collection.drop()来删除数据库中的集合：

```shell
>db.CollectionName.drop()
```
# 15. 为什么要在 MongoDB 中使用分析器？
数据库分析工具（Database Profiler）会针对正在运行的mongod实例收集数据库命令执行的相关信息。包括增删改查的命令以及配置和管理命令。分析器（profiler）会写入所有收集的数据到system.profile集合，一个capped集合在管理员数据库。分析器默认是关闭的，可以通过per数据库或per实例开启。

# 16. MongoDB 中支持主键外键关系吗？
MongoDB默认不支持主键和外键关系。

可以使用MongoDB本身的API，但是需要硬编码才能实现外键关联，不够直观且难度较大。

# 17. MongoDB 支持哪些数据类型？
String、Integer、Double、Boolean、Object、Object ID、Arrays、Min/Max Keys、Datetime、Code、Regular Expression等。

# 18. MongoDB 中 Code 数据类型有什么用？
Code类型用于在文档中存储JavaScript代码。

# 19. MongoDB 中 Regular Expression 数据类型有什么用？
Regular Expression类型用于在文档中存储正则表达式。

# 20. MongoDB 中 ObjectID 数据类型有什么用？
ObjectID数据类型用于存储文档ID。
