# 1. MongoDB 中 ObjectID 由哪些部分组成？
ObjectID一共有四部分组成：时间戳、客户端ID、客户进程ID、三个字节的增量计数器。

_id是一个12字节长的十六进制数，它保证了每一个文档的唯一性。在插入文档时，需要提供_id。如果不提供，那么MongoDB就会为每一文档提供一个唯一的id。_id的头4个字节代表的是当前的时间戳，接着的后3个字节表示的是机器id号，接着的2个字节表示MongoDB服务器进程id，最后的3个字节代表递增值。

# 2. MongoDB 中什么是索引？
索引用于高效的执行查询，没有索引MongoDB将扫描查询整个集合中的所有文档这种扫描效率很低，需要处理大量数据。

索引是一种特殊的数据结构，将一小块数据集保存为容易遍历的形式。

索引能够存储某种特殊字段或字段集的值，并按照索引指定的方式将字段值进行排序。

# 3. MongoDB 中如何添加索引？
使用db.collection.createIndex()在集合中创建一个索引：

```shell
>db.collectionName.createIndex({columnName:1})
```
# 4. MongoDB 中用什么方法可以格式化输出结果？
使用pretty()方法可以格式化显示结果：

```shell
>db.collectionName.find().pretty()
```
# 5. MongoDB 中如何使用 AND 或 OR 条件循环查询集合中的文档？
在find()方法中，如果传入多个键，并用逗号（,）分隔它们，那么MongoDB会把它看成是AND条件。

```shell
>db.mycol.find({key1:value1, key2:value2}).pretty()
```
若基于OR条件来查询文档，可以使用关键字$or。

```shell
>db.mycol.find({$or: [ {key1: value1}, {key2:value2} ] }).pretty()
```
# 6. MongoDB 中如何更新数据？
update()与save()方法都能用于更新集合中的文档。

update()方法更新已有文档中的值，而save()方法则是用传入该方法的文档来替换已有文档。

# 7. MongoDB 中如何删除文档？
MongoDB利用remove()方法清除集合中的文档。它有2个可选参数：

deletion criteria：（可选）删除文档的标准。

justOne：（可选）如果设为true或1，则只删除一个文档。

```shell
>db.collectionName.remove({key:value})
```
