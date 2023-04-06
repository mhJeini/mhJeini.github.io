# 1. Spark 是什么？
Spark是一种快速、通用、可扩展的大数据分析引擎。

2009年诞生于加州大学伯克利分校AMPLab。2010年开源，2013年6月成为Apache孵化项目。2014年2月成为Apache顶级项目。

目前，Spark生态系统已经发展成为一个包含多个子项目的集合，其中包含SparkSQL、SparkStreaming、GraphX、MLlib等子项目。

Spark是基于内存计算的大数据并行计算框架。Spark基于内存计算，提高了在大数据环境下数据处理的实时性，同时保证了高容错性和高可伸缩性，允许用户将Spark部署在大量廉价硬件之上，形成集群。

Spark得到了众多大数据公司的支持，这些公司包括Hortonworks、IBM、Intel、Cloudera、MapR、Pivotal、百度、阿里、腾讯、京东、携程、优酷土豆。

当前百度的Spark已应用于凤巢、大搜索、直达号、百度大数据等业务；阿里利用GraphX构建了大规模的图计算和图挖掘系统，实现了很多生产系统的推荐算法；腾讯Spark集群达到8000台的规模，是当前已知的世界上最大的Spark集群。

# 2. Spark 有几种部署模式，各自都有什么特点？
1）本地模式：适用于测试。

2）standalone 模式：使用spark自带的资源调度框架。

3）spark on yarn 模式：最流行的方式，使用yarn集群调度资源。

4）mesos模式：国外使用比较多。

# 3. Spark 中 Driver 功能是什么？
1）一个Spark作业运行时包括一个Driver进程，也是作业的主进程，具有main函数，并且有SparkContext的实例，是程序的入口点。

2）功能：负责向集群申请资源，向master注册信息，负责了作业的调度，，负责作业的解析、生成Stage并调度Task到Executor上。包括DAGScheduler，TaskScheduler。

# 4. 为什么 Spark 比 MapReduce 快？
1、基本原理

1） MapReduce：基于磁盘的大数据批量处理系统

2）Spark：基于RDD(弹性分布式数据集)数据处理，显示将RDD数据存储到磁盘和内存中。

2、模型

1） MapReduce可以处理超大规模的数据，适合日志分析挖掘等较少的迭代的长任务需求，结合了数据的分布式的计算。

2） Spark：适合数据的挖掘，机器学习等多轮迭代式计算任务。

总结来说：

1）基于内存计算，减少低效的磁盘交互；

2）高效的调度算法，基于DAG；

3）容错机制Linage，精华部分就是DAG和Lingae。

# 5. Hadoop 和 Spark 的 shuffle 有什么差异？
1）从high-level的角度来看，两者并没有大的差别。都是将mapper（Spark 里是 ShuffleMapTask）的输出进行partition，不同的partition送到不同的reducer（Spark 里reducer可能是下一个stage里的ShuffleMapTask，也可能是 ResultTask）。Reducer以内存作缓冲区，边shuffle边 aggregate 数据，等到数据 aggregate 好以后进行 reduce()（Spark 里可能是后续的一系列操作）。

2）从low-level的角度来看，两者差别不小。Hadoop MapReduce是sort-based，进入combine()和reduce()的records必须先sort。这样的好处在于combine/reduce()可以处理大规模的数据，因为其输入数据可以通过外排得到（mapper 对每段数据先做排序，reducer的shuffle对排好序的每段数据做归并）。

目前的Spark默认选择的是hash-based，通常使用HashMap来对shuffle来的数据进行aggregate，不会对数据进行提前排序。如果用户需要经过排序的数据，那么需要自己调用类似sortByKey()的操作；如果是Spark 1.1的用户，可以将spark.shuffle.manager设置为sort，则会对数据进行排序。在Spark 1.2中，sort将作为默认的Shuffle实现。

3）从实现角度来看，两者也有不少差别。Hadoop MapReduce将处理流程划分出明显的几个阶段：map()、spill、merge、shuffle、sort、reduce()等。每个阶段各司其职，可以按照过程式的编程思想来逐一实现每个阶段的功能。在Spark中，没有这样功能明确的阶段，只有不同的stage和一系列的transformation()，所以spill、merge、aggregate等操作需要蕴含在transformation()中。

如果将map端划分数据、持久化数据的过程称为shuffle write，而将reducer读入数据、aggregate数据的过程称为shuffle read。那么在Spark中，问题就变为怎么在job的逻辑或者物理执行图中加入shuffle write和shuffle read的处理逻辑？以及两个处理逻辑应该怎么高效实现？Shuffle write由于不要求数据有序，shuffle write的任务很简单：将数据partition好，并持久化。之所以要持久化，一方面是要减少内存存储空间压力，另一方面也是为了fault-tolerance。

# 6. Spark 中 RDD、DAG、Stage 如何理解？
DAG Spark中使用DAG对RDD的关系进行建模，描述了RDD的依赖关系，这种关系也被称之为lineage（血缘），RDD的依赖关系使用Dependency维护。DAG在Spark中的对应的实现为DAGScheduler。

RDD RDD是Spark的灵魂，也称为弹性分布式数据集。一个RDD代表一个可以被分区的只读数据集。RDD内部可以有许多分区(partitions)，每个分区又拥有大量的记录(records)。

Rdd的五个特征： 1）dependencies: 建立RDD的依赖关系，主要RDD之间是宽窄依赖的关系，具有窄依赖关系的RDD可以在同一个stage中进行计算。

2）partition: 一个RDD会有若干个分区，分区的大小决定了对这个RDD计算的粒度，每个RDD的分区的计算都在一个单独的任务中进行。

3）preferedlocations: 按照“移动数据不如移动计算”原则，在Spark进行任务调度的时候，优先将任务分配到数据块存储的位置。

4）compute: Spark中的计算都是以分区为基本单位的，compute函数只是对迭代器进行复合，并不保存单次计算的结果。

5）partitioner: 只存在于（K,V）类型的RDD中，非（K,V）类型的partitioner的值就是None。

RDD的算子主要分成2类，action和transformation。这里的算子概念，可以理解成就是对数据集的变换。action会触发真正的作业提交，而transformation算子是不会立即触发作业提交的。每一个transformation方法返回一个新的 RDD。只是某些transformation比较复杂，会包含多个子transformation，因而会生成多个RDD。这就是实际RDD个数比我们想象的多一些 的原因。通常是，当遇到action算子时会触发一个job的提交，然后反推回去看前面的transformation算子，进而形成一张有向无环图。

Stage在DAG中又进行stage的划分，划分的依据是依赖是否是shuffle的，每个stage又可以划分成若干task。接下来的事情就是driver发送task到executor，executor自己的线程池去执行这些task，完成之后将结果返回给driver。action算子是划分不同job的依据。

# 7. Spark 中宽依赖、窄依赖如何理解？
1、窄依赖指的是每一个parent RDD的partition最多被子RDD的一个partition使用（一子一亲）

2、宽依赖指的是多个子RDD的partition会依赖同一个parent RDD的partition（多子一亲） RDD作为数据结构，本质上是一个只读的分区记录集合。一个RDD可以包含多个分区，每个分区就是一个dataset片段。RDD可以相互依赖。

首先，窄依赖可以支持在同一个cluster node上，以pipeline形式执行多条命令（也叫同一个 stage 的操作），例如在执行了map后，紧接着执行filter。相反，宽依赖需要所有的父分区都是可用的，可能还需要调用类似 MapReduce 之类的操作进行跨节点传递。

其次，则是从失败恢复的角度考虑。窄依赖的失败恢复更有效，因为它只需要重新计算丢失的parent partition即可，而且可以并行地在不同节点进行重计算（一台机器太慢就会分配到多个节点进行），相反的是宽依赖牵涉RDD各级的多个parent partition。

# 8. Spark 有什么优越性？
1）更高的性能。因为数据被加载到集群主机的分布式内存中。数据可以被快速的转换迭代，并缓存用以后续的频繁访问需求。在数据全部加载到内存的情况下，Spark可以比Hadoop快100倍，在内存不够存放所有数据的情况下快hadoop10倍。

2）通过建立在Java、Scala、Python、SQL（应对交互式查询）的标准API以方便各行各业使用，同时还含有大量开箱即用的机器学习库。

3）与现有Hadoop 1和2.x(YARN)生态兼容，因此机构可以无缝迁移。

4）方便下载和安装。方便的shell（REPL: Read-Eval-Print-Loop）可以对API进行交互式的学习。

5）借助高等级的架构提高生产力，从而可以讲精力放到计算上。

# 9. Spark 作业提交流程是如何实现的？
1、spark-submit提交代码，执行new SparkContext()，在SparkContext里构造DAGScheduler和TaskScheduler。

2、TaskScheduler会通过后台的一个进程，连接Master，向Master注册Application。

3、Master接收到Application请求后，会使用相应的资源调度算法，在Worker上为这个 Application启动多个Executer。

4、Executor启动后，会自己反向注册到TaskScheduler中。所有Executor都注册到 Driver上之后，SparkContext结束初始化，接下来往下执行我们自己的代码。

5、每执行到一个Action，就会创建一个Job。Job会提交给DAGScheduler。

6、DAGScheduler会将Job划分为多个stage，然后每个stage创建一个TaskSet。

7、TaskScheduler会把每一个TaskSet里的 Task，提交到Executor上执行。

8、Executor上有线程池，每接收到一个Task，就用TaskRunner封装，然后从线程池里取出一个线程执行这个task。(TaskRunner 将编写的代码，拷贝和反序列化，执行Task，每个Task执行RDD里的一个partition)

# 10. Spark sql 使用过吗？在什么项目中？
离线ETL之类的，结合机器学习等。

# 11. Spark 中 ML 和 MLLib 两个包区别和联系？
1、技术角度上，面向的数据集类型不同

ML的API是面向Dataset的（Dataframe 是 Dataset的子集，也就是Dataset[Row]）， mllib是面对RDD的。

Dataset和RDD有啥不一样呢？

Dataset的底端是RDD。Dataset对RDD进行了更深一层的优化，比如说有sql语言类似的黑魔法，Dataset支持静态类型分析所以在compile time就能报错，各种combinators（map，foreach 等）性能会更好，等等。

2、编程过程上，构建机器学习算法的过程不同

ML提倡使用pipelines，把数据想成水，水从管道的一段流入，从另一端流出。ML是1.4比 Mllib更高抽象的库，它解决如果简洁的设计一个机器学习工作流的问题，而不是具体的某种机器学习算法。未来这两个库会并行发展。

# 12. 为什么要使用 Yarn 部署 Spark？
这是因为Yarn支持动态资源配置。Standalone模式只支持简单的固定资源分配策略，每个任务固定数量的core，各Job按顺序依次分配在资源，资源不够的时候就排队。这种模式比较适合单用户的情况，多用户的情境下，会有可能有些用户的任务得不到资源。

Yarn作为通用的种子资源调度平台，除了Spark提供调度服务之外，还可以为其他系统提供调度，如Hadoop MapReduce、Hive等。

# 13. 说一说 Spark 中 yarn-cluster 和 yarn-client 有什么异同点？
1、cluster模式会在集群的某个节点上为Spark程序启动一个称为Master的进程，然后Driver程序会运行正在这个Master进程内部，由这种进程来启动Driver程序，客户端完成提交的步骤后就可以退出，不需要等待Spark程序运行结束，这是四一职中适合生产环境的运行方式。

2、client模式也有一个Master进程，但是Driver程序不会运行在这个Master进程内部，而是运行在本地，只是通过Master来申请资源，直到运行结束，这种模式非常适合需要交互的计算。显然Driver在client模式下会对本地资源造成一定的压力。

# 14. Spark 中 RDD 是什么？
RDD（Resilient Distributed Dataset）叫做分布式数据集，是Spark中最基本的数据抽象，它代表一个不可变、可分区、里面的元素可并行计算的集合。

RDD中的数据可以存储在内存或者是磁盘，而且RDD中的分区是可以改变的。

# 15. Spark 中 cache 和 persist 有什么区别？
cache： 缓存数据，默认是缓存在内存中，其本质还是调用 persist；

persist： 缓存数据，有丰富的数据缓存策略。数据可以保存在内存也可以保存在磁盘中，使用的时候指定对应的缓存级别即可。

# 16. 如何解决 Spark 中的数据倾斜问题？
当发现数据倾斜的时候，不要急于提高executor的资源，修改参数或是修改程序。

一、数据问题造成的数据倾斜

找出异常的key，如果任务长时间卡在最后最后1个(几个)任务，首先要对key进行抽样分析，判断是哪些key造成的。 选取key，对数据进行抽样，统计出现的次数，根据出现次数大小排序取出前几个。

比如
```java
df.select("key").sample(false,0.1)
.(k=>(k,1)).reduceBykey(+)
.map(k=>(k._2,k._1))
.sortByKey(false).take(10)
```

如果发现多数数据分布都较为平均，而个别数据比其他数据大上若干个数量级，则说明发生了数据倾斜。

经过分析，倾斜的数据主要有以下三种情况:

1）null（空值）或是一些无意义的信息()之类的，大多是这个原因引起。

2）无效数据，大量重复的测试数据或是对结果影响不大的有效数据。

3）有效数据，业务导致的正常数据分布。

解决办法

第1，2种情况，直接对数据进行过滤即可（因为该数据对当前业务不会产生影响）。

第3种情况则需要进行一些特殊操作，常见的有以下几种做法

1）隔离执行，将异常的key过滤出来单独处理，最后与正常数据的处理结果进行union操作。

2）对key先添加随机值，进行操作后，去掉随机值，再进行一次操作。

3）使用reduceByKey 代替 groupByKey（reduceByKey用于对每个key对应的多个value进行merge操作，最重要的是它能够在本地先进行merge操作，并且merge操作可以通过函数自定义）

4）使用map join。

案例

如果使用reduceByKey因为数据倾斜造成运行失败的问题。具体操作流程如下:

1）将原始的 key 转化为 key + 随机值(例如Random.nextInt)

2）对数据进行 reduceByKey(func)

3）将key+随机值 转成 key

4）再对数据进行reduceByKey(func)

案例操作流程分析：

假设说有倾斜的Key，给所有的Key加上一个随机数，然后进行reduceByKey操作；此时同一个Key会有不同的随机数前缀，在进行reduceByKey操作的时候原来的一个非常大的倾斜的Key就分而治之变成若干个更小的Key，不过此时结果和原来不一样，怎么解决？进行map操作，目的是把随机数前缀去掉，然后再次进行reduceByKey操作，这样就可以把原本倾斜的Key通过分而治之方案分散开来，最后又进行了全局聚合。

注意1: 如果此时依旧存在问题，建议筛选出倾斜的数据单独处理。最后将这份数据与正常的数据进行union即可。

注意2: 单独处理异常数据时，可以配合使用Map Join解决。

二、Spark使用不当造成的数据倾斜

提高shuffle并行度

1）dataFrame和sparkSql可以设置spark.sql.shuffle.partitions参数控制shuffle的并发度，默认为200。

2）rdd操作可以设置spark.default.parallelism控制并发度，默认参数由不同的Cluster Manager控制。

3）局限性: 只是让每个task执行更少的不同的key。无法解决个别key特别大的情况造成的倾斜，如果某些key的大小非常大，即使一个task单独执行它，也会受到数据倾斜的困扰。

使用map join 代替reduce join

在小表不是特别大（取决于executor大小）的情况下使用，可以使程序避免shuffle的过程，自然也就没有数据倾斜的困扰。这是因为局限性的问题，也就是先将小数据发送到每个executor上，所以数据量不能太大。

# 17. Spark 运行架构的特点是什么？
每个Application获取专属的executor进程，该进程在Application期间一直驻留，并以多线程方式运行tasks。

Spark任务与资源管理器无关，只要能够获取 executor进程，并能保持相互通信就可以了。

提交SparkContext的Client应该靠近Worker节点（运行 Executor 的节点)，最好是在同一个Rack里，因为Spark程序运行过程中SparkContext和Executor之间有大量的信息交换；如果想在远程集群中运行，最好使用 RPC将SparkContext提交给集群，不要远离Worker运行SparkContext。Task采用了数据本地性和推测执行的优化机制。

# 18. Spark 中 map 和 mapPartitions 有什么区别？
map中的func作用的是RDD中每一个元素，而 mapPartitioons中的func作用的对象是RDD的一整个分区。所以func的类型是Iterator<T> => Iterator<T>，其中T是输入RDD的元素类型。

# 19. Spark 中调优方式都有哪些？
资源参数调优

1、num-executors：设置Spark作业总共要用多少个Executor进程来执行

2、executor-memory：设置每个Executor进程的内存

3、executor-cores：设置每个Executor进程的CPU core数量

4、driver-memory：设置Driver进程的内存

5、spark.default.parallelism：设置每个stage的默认task数量

开发调优

1、避免创建重复的RDD

2、尽可能复用同一个RDD

3、对多次使用的RDD进行持久化

4、尽量避免使用shuffle类算子

5、使用map-side预聚合的shuffle操作

6、使用高性能的算子

①使用reduceByKey/aggregateByKey替代groupByKey

②使用mapPartitions替代普通map

③使用foreachPartitions替代foreach

④使用filter之后进行coalesce操作

⑤使用repartitionAndSortWithinPartitions替代repartition与sort类操作

7、广播大变量：在算子函数中使用到外部变量时，默认情况下，Spark会将该变量复制多个副本，通过网络传输到task中，此时每个task都有一个变量副本。如果变量本身比较大的话（比如100M，甚至1G），那么大量的变量副本在网络中传输的性能开销，以及在各个节点的Executor中占用过多内存导致的频繁GC(垃圾回收)，都会极大地影响性能。

8、使用Kryo优化序列化性能

9、优化数据结构：在可能以及合适的情况下，使用占用内存较少的数据结构，但是前提是要保证代码的可维护性。

# 20. Spark 中如何实现获取 TopN？
方法1：

1）按照key对数据进行聚合（groupByKey）

2）将value转换为数组，利用scala的sortBy或者sortWith进行排序（mapValues）

注意：当数据量太大时，会导致OOM

方法2：

1）取出所有的key

2）对key进行迭代，每次取出一个key利用spark的排序算子进行排序

方法3：

1）自定义分区器，按照key进行分区，使不同的key进到不同的分区

2）对每个分区运用spark的排序算子进行排序