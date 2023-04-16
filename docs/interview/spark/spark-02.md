# 1. Spark Streaming 工作流程和 Storm 有什么区别？
Spark Streaming与Storm都可以用于进行实时流计算。但是他们两者的区别是非常大的。

Spark Streaming和Storm的计算模型完全不一样，Spark Streaming是基于RDD的，因此需要将一小段时间内的，比如1秒内的数据，收集起来，作为一个RDD，然后再针对这个batch的数据进行处理。而Storm却可以做到每来一条数据，都可以立即进行处理和计算。因此，Spark Streaming实际上严格意义上来说，只能称作准实时的流计算框架；而Storm是真正意义上的实时计算框架。

Storm支持的一项高级特性，是Spark Streaming暂时不具备的，即Storm支持在分布式流式计算程序（Topology）在运行过程中，可以动态地调整并行度，从而动态提高并发处理能力。而Spark Streaming是无法动态调整并行度的。但是Spark Streaming也有其优点，首先Spark Streaming由于是基于batch进行处理的，因此相较于 Storm 基于单条数据进行处理，具有数倍甚至数十倍的吞吐量。

Spark Streaming由于也身处于Spark生态圈内，因此Spark Streaming可以与Spark Core、Spark SQL，甚至是Spark MLlib、Spark GraphX进行无缝整合。流式处理完的数据，可以立即进行各种map、reduce转换操作，可以立即使用sql进行查询，甚至可以立即使用machine learning或者图计算算法进行处理。这种一站式的大数据处理功能和优势，是Storm无法匹敌的。 因此，综合上述来看，通常在对实时性要求特别高，而且实时数据量不稳定，比如在白天有高峰期的情况下，可以选择使用Storm。但是如果是对实时性要求一般，允许1秒的准实时处理，而且不要求动态调整并行度的话，选择Spark Streaming是更好的选择。

| 对比点	 | Storm	| Spark Streaming|
| ----------------	| ----------------	| ----------------|
| 实时计算模型	| 纯实时，来一条数据，处理一条数据	| 准实时，对一个时间段内的数据收集起来，作为一个RDD，再处理|
| 实时计算延迟度	| 毫秒级	| 秒级|
| 吞吐量	| 低	| 高|
| 事务机制	| 支持完善	| 支持，但不够完善|
| 健壮性 / 容错性	| ZooKeeper，Acker，非常强	| Checkpoint，WAL，一般|
| 动态调整并行度	| 支持	| 不支持|
# 2. 概述一下 Spark 中的常用算子区别？
map：用于遍历RDD，将函数应用于每一个元素，返回新的RDD（transformation算子）

foreach：用于遍历RDD，将函数应用于每一个元素，无返回值（action算子）

mapPatitions：用于遍历操作RDD中的每一个分区，返回生成一个新的RDD（transformation算子）

foreachPatition：用于遍历操作RDD中的每一个分区，无返回值（action算子）

简单总结：一般使用mapPatitions和foreachPatition算子比map和foreach更加高效，推荐使用。

# 3. Spark 技术栈有哪些组件，适合什么应用场景？
1）Spark core：是其它组件的基础，spark的内核，主要包含：有向循环图、RDD、Lingage、Cache、broadcast等，并封装了底层通讯框架，是Spark的基础。

2）SparkStreaming是一个对实时数据流进行高通量、容错处理的流式处理系统，可以对多种数据源（如Kdfka、Flume、Twitter、Zero和TCP 套接字）进行类似Map、Reduce和Join等复杂操作，将流式计算分解成一系列短小的批处理作业。

3）Spark sql：Shark是SparkSQL的前身，Spark SQL的一个重要特点是其能够统一处理关系表和RDD，使得开发人员可以轻松地使用SQL命令进行外部查询，同时进行更复杂的数据分析

4）BlinkDB：是一个用于在海量数据上运行交互式 SQL 查询的大规模并行查询引擎，它允许用户通过权衡数据精度来提升查询响应时间，其数据的精度被控制在允许的误差范围内。

5）MLBase是Spark生态圈的一部分专注于机器学习，让机器学习的门槛更低，让一些可能并不了解机器学习的用户也能方便地使用MLbase。MLBase分为四部分：MLlib，MLI、ML Optimizer和MLRuntime。

6）GraphX是Spark中用于图和图并行计算。

# 4. Spark 中 worker 的主要工作是什么？
管理当前节点内存，CPU的使用情况，接受master发送过来的资源指令，通过executorRunner启动程序分配任务，worker就类似于包工头，管理分配新进程，做计算的服务，相当于process服务，需要注意的是：

1）worker会不会汇报当前信息给master？

worker心跳给master主要只有workid，不会以心跳的方式发送资源信息给master，这样master就知道worker是否存活，只有故障的时候才会发送资源信息；

2）worker不会运行代码，具体运行的是executor，可以运行具体application斜的业务逻辑代码，操作代码的节点，不会去运行代码。

# 5. Spark 中主要包括哪些组件？
1）master：管理集群和节点，不参与计算。

2）worker：计算节点，进程本身不参与计算，和master汇报。

3）Driver：运行程序的main方法，创建spark context对象。

4）spark context：控制整个application的生命周期，包括dagsheduler和task scheduler等组件。

5）client：用户提交程序的入口。

# 6. Spark 中 RDD 弹性表现在哪几点？
1）自动的进行内存和磁盘的存储切换；

2）基于Lingage的高效容错；

3）task如果失败会自动进行特定次数的重试；

4）stage如果失败会自动进行特定次数的重试，而且只会计算失败的分片；

5）checkpoint和persist，数据计算之后持久化缓存；

6）数据调度弹性，DAG TASK调度和资源无关；

7）数据分片的高度弹性

a. 分片很多碎片可以合并成大的

b. par

# 7. Spark 中常规的容错方式有哪几种类型？
1）数据检查点，会发生拷贝，浪费资源。

2）记录数据的更新，每次更新都会记录下来，比较复杂且比较消耗性能。

# 8. Spark 中 RDD 通过 Linage（记录数据更新）的方式为何很高效？
1）lazy记录了数据的来源，RDD是不可变的，且是lazy级别的，且rDD之间构成了链条，lazy是弹性的基石。由于RDD不可变，所以每次操作就产生新的rdd，不存在全局修改的问题，控制难度下降，所有有计算链条将复杂计算链条存储下来，计算的时候从后往前回溯900步是上一个stage的结束，要么就checkpoint。

2）记录原数据，是每次修改都记录，代价很大。如果修改一个集合，代价就很小，官方说rdd是粗粒度的操作，是为了效率，为了简化，每次都是操作数据集合，写或者修改操作，都是基于集合的rdd的写操作是粗粒度的，rdd的读操作既可以是粗粒度的也可以是细粒度，读可以读其中的一条条的记录。

3）简化复杂度，是高效率的一方面，写的粗粒度限制了使用场景，如网络爬虫，现实世界中，大多数写是粗粒度的场景。

# 9. Spark 中 RDD 有哪些不足之处？
1）不支持细粒度的写和更新操作（如网络爬虫），spark写数据是粗粒度的。

所谓粗粒度，就是批量写入数据，为了提高效率。但是读数据是细粒度的也就是说可以一条条的读。

2）不支持增量迭代计算，Flink支持。

# 10. Spark 中列举一些你常用的 action？
collect、reduce、take、count、saveAsTextFile等。

# 11. Spark 为什么要持久化，一般什么场景下要进行 persist 操作？
为什么要进行持久化？

spark所有复杂一点的算法都会有persist身影,spark默认数据放在内存，spark很多内容都是放在内存的，非常适合高速迭代，1000个步骤。

只有第一个输入数据，中间不产生临时数据，但分布式系统风险很高，所以容易出错，就要容错，rdd出错或者分片可以根据血统算出来，如果没有对父rdd进行persist 或者cache的化，就需要重头做。

使用persist场景

1）某个步骤计算非常耗时，需要进行persist持久化

2）计算链条非常长，重新恢复要算很多步骤，很好使，persist

3）checkpoint所在的rdd要持久化persist

lazy级别，框架发现有checnkpoint，checkpoint时单独触发一个job，需要重算一遍，checkpoint前

要持久化，写个rdd.cache或者rdd.persist，将结果保存起来，再写checkpoint操作，这样执行起来会非常快，不需要重新计算rdd链条了。checkpoint之前一定会进行persist。

4）shuffle之后为什么要persist？shuffle要进性网络传输，风险很大，数据丢失重来，恢复代价很大

5）shuffle之前进行persist，框架默认将数据持久化到磁盘，这个是框架自动做的。

# 12. Spark 为什么要进行序列化？
序列化可以减少数据的体积，减少存储空间，高效存储和传输数据，不好的是使用的时候要反序列化，非常消耗CPU。

# 13. Spark 中 RDD 有几种操作类型？
1）transformation、rdd由一种转为另一种rdd

2）action

3）cronroller、crontroller是控制算子，cache、persist，对性能和效率的有很好的支持三种类型，不要回答只有2中操作。

# 14. 说一说 cogroup rdd 实现原理，在什么场景下使用过 rdd？
cogroup的函数实现:这个实现根据两个要进行合并的两个RDD操作，生成一个CoGroupedRDD的实例，这个RDD的返回结果是把相同的key中两个RDD分别进行合并操作，最后返回的RDD的value是一个Pair的实例。

这个实例包含两个Iterable的值，第一个值表示的是RDD1中相同KEY的值，第二个值表示的是RDD2中相同key的值。

由于做cogroup的操作，需要通过partitioner进行重新分区的操作，因此，执行这个流程时，需要执行一次shuffle的操作(如果要进行合并的两个RDD的都已经是shuffle后的rdd，同时他们对应的partitioner相同时，就不需要执行shuffle。

# 15. Spark 中常见的 join 操作优化有哪些分类？
join常见分为两类：map-side join 和 reduce-side join。

当大表和小表join时，用map-side join能显著提高效率。将多份数据进行关联是数据处理过程中非常普遍的用法，不过在分布式计算系统中，这个问题往往会变的非常麻烦，因为框架提供的join操作一般会将所有数据根据key发送到所有的reduce分区中去，也就是shuffle的过程。造成大量的网络以及磁盘IO消耗，运行效率极其低下，这个过程一般被称为reduce-side-join。

如果其中有张表较小的话，则可以自身实现在 map端实现数据关联，跳过大量数据进行shuffle的过程，运行时间得到大量缩短，根据不同数据可能会有几倍到数十倍的性能提升。

# 16. Spark 中 map 和 flatMap 有什么区别？
map：对RDD每个元素转换，文件中的每一行数据返回一个数组对象。

flatMap：对RDD每个元素转换，然后再扁平化。

将所有的对象合并为一个对象，文件中的所有行数据仅返回一个数组对象，会抛弃值为null的值。

# 17. Spark 中 collect 功能是什么，其底层是如何实现的？
driver通过collect把集群中各个节点的内容收集过来汇总成结果，collect返回结果是Array类型的，collect把各个节点上的数据抓过来，抓过来数据是Array型，collect对Array抓过来的结果进行合并，合并后Array中只有一个元素，是tuple类型（KV类型的）的。

# 18. Spark 如何处理不能被序列化的对象？
将不能序列化的对象封装成Object。

# 19. Spark 程序执行时，为什么默认有时产生很多 task，如何修改 task 个数？
1）因为输入数据有很多task，尤其是有很多小文件的时候，有多少个输入block就会有多少个task启动；

2）spark中有partition的概念，每个partition都会对应一个task，task越多，在处理大规模数据的时候，就会越有效率。不过task并不是越多越好，如果平时测试，或者数据量没有那么大，则没有必要task数量太多。

3）参数可以通过spark_home/conf/spark-default.conf配置文件设置:

spark.sql.shuffle.partitions 50
spark.default.parallelism 10
spark.sql.shuffle.partitions 设置的是RDD1做shuffle处理后生成的结果RDD2的分区数，默认值200。

spark.default.parallelism 是指RDD任务的默认并行度，Spark中所谓的并行度是指RDD中的分区数，即RDD中的Task数。

当初始RDD没有设置分区数（numPartitions或numSlice）时，则分区数采用spark.default.parallelism的取值。

# 20. spark.sql.shuffle.partitions 和 spark.default.parallelism 有什么区别和联系？
spark.default.parallelism只有在处理RDD时有效；而spark.sql.shuffle.partitions则是只对SparkSQL有效。

spark.sql.shuffle.partitions： 设置的是RDD1做shuffle处理后生成的结果RDD2的分区数。

默认值：200

**spark.default.parallelism: ** 设置的是RDD1做shuffle处理/并行处理(窄依赖算子)后生成的结果RDD2的分区数。

默认值：

对于分布式的shuffle算子, 默认值使用了结果RDD2所依赖的所有父RDD中分区数最大的, 作为自己的分区数。

对于并行处理算子（窄依赖的），有父依赖的，结果RDD分区数=父RDD分区数，没有父依赖的看集群配置：

Local mode:给定的core个数

Mesos fine grained mode: 8

Others: max(RDD分区数为总core数, 2)