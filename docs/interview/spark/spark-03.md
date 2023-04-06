# 1. Spark 集群运算有哪些模式？
Spark有很多种模式，最简单就是单机本地模式，还有单机伪分布式模式，复杂的则运行在集群中，目前能很好的运行在Yarn和Mesos中，当然Spark还有自带的Standalone模式，对于大多数情况Standalone模式就已经足够，如果企业已经有Yarn或者Mesos环境，也是很方便部署的。

standalone（集群模式）：典型的Mater/slave模式，不过也能看出Master是有单点故障的；Spark支持ZooKeeper来实现HA。

on yarn（集群模式）：运行在yarn资源管理器框架之上，由yarn负责资源管理，Spark负责任务调度和计算。

on mesos（集群模式）：运行在mesos资源管理器框架之上，由mesos负责资源管理，Spark负责任务调度和计算。

on cloud（集群模式）：比如AWS的EC2，使用这个模式能很方便的访问Amazon的S3；Spark支持多种分布式存储系统：HDFS和S3。

# 2. 简述一下 Spark 主备切换机制原理？
Master实际上可以配置两个，Spark原生的standalone模式是支持Master主备切换的。当Active Master节点挂掉以后，我们可以将Standby Master切换为Active Master。

Spark Master主备切换可以基于两种机制，一种是基于文件系统的，一种是基于ZooKeeper的。

基于文件系统的主备切换机制，需要在Active Master挂掉之后手动切换到Standby Master上；

而基于Zookeeper的主备切换机制，可以实现自动切换Master。

# 3. Spark 中数据倾斜的产生和解决办法？
数据倾斜以为着某一个或者某几个partition的数据特别大，导致这几个partition上的计算需要耗费相当长的时间。

在spark中同一个应用程序划分成多个stage，这些stage之间是串行执行的，而一个stage里面的多个task是可以并行执行，task数目由partition数目决定，如果一个partition的数目特别大，那么导致这个task执行时间很长，导致接下来的stage无法执行，从而导致整个job执行变慢。

避免数据倾斜，一般是要选用合适的key，或者自己定义相关的partitioner，通过加盐或者哈希值来拆分这些key，从而将这些数据分散到不同的partition去执行。

如下算子会导致shuffle操作，是导致数据倾斜可能发生的关键点所在：groupByKey；reduceByKey；aggregaByKey；join；cogroup。

# 4. RDD 中 reduceBykey 与 groupByKey 哪个性能好，为什么？
reduceByKey： reduceByKey会在结果发送至reducer之前会对每个mapper在本地进行merge，有点类似于在MapReduce中的combiner。这样做的好处在于，在map端进行一次reduce之后，数据量会大幅度减小，从而减小传输，保证reduce端能够更快的进行结果计算。

groupByKey： groupByKey会对每一个RDD中的value值进行聚合形成一个序列(Iterator)，此操作发生在reduce端，所以势必会将所有的数据通过网络进行传输，造成不必要的浪费。同时如果数据量十分大，可能还会造成OutOfMemoryError。

所以在进行大量数据的reduce操作时候建议使用reduceByKey。不仅可以提高速度，还可以防止使用groupByKey造成的内存溢出问题。

# 5. Spark 中 DStream 是什么及基本工作原理？
DStream是spark streaming提供的一种高级抽象，代表了一个持续不断的数据流。

DStream可以通过输入数据源来创建，比如Kafka、flume等，也可以通过其他DStream的高阶函数来创建，比如map、reduce、join和window等。

DStream内部其实不断产生RDD，每个RDD包含了一个时间段的数据。

Spark streaming一定是有一个输入的DStream接收数据，按照时间划分成一个一个的batch，并转化为一个RDD，RDD的数据是分散在各个子节点的partition中。