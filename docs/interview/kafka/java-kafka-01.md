# 1. 什么是 Apache Kafka？
Kafka是分布式发布-订阅消息系统，它最初是由LinkedIn公司开发的，之后成为Apache项目的一部分，Kafka是一个分布式，可划分的，冗余备份的持久性的日志服务，它主要用于处理流式数据。

Apach Kafka是一款分布式流处理框架，用于实时构建流处理应用。它有一个核心的功能广为人知，即作为企业级的消息引擎被广泛使用。

# 2. Kafka 的分区策略有哪些？
ProducerRecord内部数据结构：

>-- Topic （名字） -- PartitionID ( 可选) -- Key[( 可选 ) -- Value

提供三种构造函数形参：

```java
ProducerRecord(topic, partition, key, value)
ProducerRecord(topic, key, value)
ProducerRecord(topic, value)
```

第一种分区策略：指定分区号，直接将数据发送到指定的分区中。

第二种分区策略：没有指定分区号，指定数据的key值，通过key取上hashCode进行分区。

第三种分区策略：没有指定分区号，也没有指定key值，直接轮循进行分区。

第四种分区策略：自定义分区。

# 3. Kafka 消费者如何取消订阅？
在KafkaConsumer类中使用unsubscribe()方法来取消主题的订阅。该方法可以取消如下的订阅方法：

subscribe(Collection);方式实现的订阅 subscribe(Pattern);方式实现的订阅 assign() 方式实现的订阅

取消订阅使用代码如下：

```java
consumer.unsubscribe();
```
将subscribe(Collection)或assign()中的集合参数设置为空集合，也可以实现取消订阅，以下三种方式都可以取消订阅：

```java
consumer.unsubscribe();
consumer.subscribe(new ArrayList<String>());
consumer.assign(new ArrayList<TopicPartition>());
```
# 4. Kafka 中有哪些核心的组件？
1）replication（副本）、partition（分区）

一个topic可以有多个副本，副本的数量决定了有多少个broker存放写入的数据；副本是以partition为单位的，存放副本即是备份若干个partition，但是只有一个partition被选为Leader用于读写；partition（分区）数量设置最好大于consumer数量（保证每个消费者都有一个partition）。

2）producer（生产者）

kafka中的producer可以直接发送消息到Leader partition；producer可以决定将消息推送到哪些partition；可以使用批处理（Batch）推送消息，提高效率；一个重要的参数acks（0、-1、1）。

3）consumer（消费者）

消费者分组，同一个group的consumer不能同时消费同一个partition，对于同一个group的consumer，kafka就相当于一个队列消息服务，各个consumer均衡的消费相应partition中的数据。当消费者数大于分区数时，存在leader consumer和follower consumer，leader consumer处理所有的读写请求，当leader consumer挂掉时，follower consumer会成为新的leader consumer。

# 5. Kafka 有几种数据保留的策略？
Kafka有两种数据保存策略：按照过期时间保留和按照存储的消息大小保留。

# 6. Kafka 在什么情况下回导致运行变慢？
以下情况可导致Kafka运行变慢：

1、CPU性能瓶颈；

2、磁盘读写瓶颈；

3、网络瓶颈。

# 7. Kafka 集群使用时需要注意什么？
Kafka集群使用时需要注意以下事项：

1、Kafka集群的数量不是越多越好，最好不要超过7个，因为节点越多，消息复制所需要的时间就越长，整个群组的吞吐量就越低；

2、Kafka集群数量最好是单数，因为超过一半故障集群就不能使用了，设置为单数容错率更高。

# 8. Kafka 中位移 offset 有什么作用？
Kafka中每个主题分区下的每条消息都被赋予了一个唯一的ID数值，用于标识它在分区中的位置。这个ID数值，就被称为位移，或者叫偏移量。一旦消息被写入到分区日志，它的位移值将不能被修改。

# 9. Kafka 如何设置接收的消息大小？
Kafka同时设置Broker端参数和Consumer端参数。

Broker端参数：message.max.bytes、max.message.bytes(主题级别)和replica.fetch.max.bytes。

Consumer端参数：fetch.message.max.bytes。

需要注意的是Broker端的最后一个参数比较容易遗漏。必须调整Follower副本能够接收的最大消息的大小，否则，副本同步就会失败。

# 10. 监控 Kafka 的框架都有哪些？
Kafka Manager：应该算是最有名的专属Kafka监控框架了，是独立的监控系统。

Kafka Monitor：LinkedIn开源的免费框架，支持对集群进行系统测试，并实时监控测试结果。

CruiseControl：是LinkedIn公司开源的监控框架，用于实时监测资源使用率，以及提供常用运维操作等。无UI界面，只提供REST API。

JMX 监控：由于Kafka提供的监控指标都是基于JMX的，因此，市面上任何能够集成JMX的框架都可以使用，比如Zabbix和Prometheus。

已有大数据平台自己的监控体系：像Cloudera提供的CDH大数据平台等。

JMXTool：社区提供的命令行工具，能够实时监控JMX指标。此工具属于面试时加分项，知道的比较很少，而且会给人一种你对Kafka工具非常熟悉的感觉。关注Java精选公众号，面试题持续更新。如果暂时不了解它的用法，可以在命令行以无参数方式执行一下kafka-run-class.sh kafka.tools.JmxTool，学习下它的用法。

# 11. Kafka 分区 Leader 选举策略有几种？
Kafka有4种分区Leader选举策略。

OfflinePartition Leader选举：每当有分区上线时，就需要执行Leader选举。所谓的分区上线，可能是创建了新分区，也可能是之前的下线分区重新上线。这是最常见的分区Leader选举场景。

ReassignPartition Leader选举：当手动运行kafka-reassign-partitions命令，或者是调用Admin的alterPartitionReassignments方法执行分区副本重分配时，可能触发此类选举。假设原来的 AR 是[1，2，3]，Leader是1，当执行副本重分配后，副本集合AR被设置成[4，5，6]，显然，Leader 必须要变更，此时会发生Reassign Partition Leader选举。

PreferredReplicaPartition Leader选举：当手动运行kafka-preferred-replica- election命令，或自动触发了Preferred Leader选举时，该类策略被激活。所谓的Preferred Leader，指的是AR中的第一个副本。比如AR是[3，2，1]，那么，Preferred Leader就是 3。

ControlledShutdownPartition Leader选举：当Broker正常关闭时，该Broker上的所有Leader副本都会下线，因此，需要为受影响的分区执行相应的Leader选举。

# 12. Kafka 对比传统技术有什么优势？
Apache Kafka与传统的消息传递技术相比优势之处在于：

快速：单一的Kafka代理可以处理成千上万的客户端，每秒处理数兆字节的读写操作。

可伸缩：在一组机器上对数据进行分区和简化，以支持更大的数据。

持久：消息是持久性的，并在集群中进行复制，以防止数据丢失。

设计：它提供了容错保证和持久性。

# 13. Kafka 中如何提高远程用户的吞吐量？
如果用户位于与broker不同的数据中心，则可能需要调优套接口缓冲区大小，以对长网络延迟进行摊销。

# 14. Kafka 中如何减少 ISR 扰动？broker何时离开 ISR？
ISR是一组与leaders完全同步的消息副本，也就是说ISR中包含了所有提交的消息。

ISR应该总是包含所有的副本，直到出现真正的故障。

如果一个副本从leader中脱离出来，将会从ISR中删除。

# 15. Kafka为什么需要复制？
Kafka的信息复制确保了任何已发布的消息不会丢失，并且可以在机器错误、程序错误或更常见些的软件升级中使用。

# 16. Kafka 中副本在 ISR 中停留很长时间表明什么？
如果一个副本在ISR中保留了很长一段时间，那么它就表明，跟踪器无法像在leader收集数据那样快速地获取数据。

# 17. Kafka 中可能在生产后发生消息偏移吗？
在大多数队列系统中，作为生产者的类无法做到这一点，它的作用是触发并忘记消息。broker将完成剩下的工作，比如使用id进行适当的元数据处理、偏移量等。

作为消息的用户，可以从Kafka broker中获得补偿。如果注意SimpleConsumer类，会发现它会获取包括偏移量作为列表的MultiFetchResponse对象。此外，当对Kafka消息进行迭代时，会拥有包括偏移量和消息发送的MessageAndOffset对象。

# 18. Kafka 是如何保证数据不丢失的？
对于Kafka的Broker而言，Kafka的复制机制和分区的多副本架构是Kafka可靠性保证的核心。把消息写入多个副本可以使Kafka在发生崩溃时仍能保证消息的持久性。

主要包括三个方面：

1、Topic 副本因子个数：replication.factor >= 3

2、同步副本列表(ISR)：min.insync.replicas = 2

3、禁用unclean选举：unclean.leader.election.enable=false

逐步分析这三个配置：

副本因子

Kafka的topic是可以分区的，并且可以为分区配置多个副本，该配置可以通过replication.factor参数实现。Kafka中的分区副本包括两种类型：领导者副本（Leader Replica）和追随者副本（Follower Replica)，每个分区在创建时都要选举一个副本作为领导者副本，其余的副本自动变为追随者副本。在 Kafka 中，追随者副本是不对外提供服务的，也就是说，任何一个追随者副本都不能响应消费者和生产者的读写请求。所有的请求都必须由领导者副本来处理。换句话说，所有的读写请求都必须发往领导者副本所在的 Broker，由该 Broker 负责处理。追随者副本不处理客户端请求，它唯一的任务就是从领导者副本异步拉取消息，并写入到自己的提交日志中，从而实现与领导者副本的同步。

一般来说，副本设为3可以满足大部分的使用场景，也有可能是5个副本(比如银行)。如果副本因子为N，那么在N-1个broker 失效的情况下，仍然能够从主题读取数据或向主题写入数据。所以，更高的副本因子会带来更高的可用性、可靠性和更少的故障。另一方面，副本因子N需要至少N个broker ，而且会有N个数据副本，也就是说它们会占用N倍的磁盘空间。实际生产环境中一般会在可用性和存储硬件之间作出权衡。

除此之外，副本的分布同样也会影响可用性。默认情况下，Kafka会确保分区的每个副本分布在不同的Broker上，但是如果这些Broker在同一个机架上，一旦机架的交换机发生故障，分区就会不可用。所以建议把Broker分布在不同的机架上，可以使用broker.rack参数配置Broker所在机架的名称。

同步副本列表

In-sync replica(ISR)称之为同步副本，ISR中的副本都是与Leader进行同步的副本，所以不在该列表的follower会被认为与Leader是不同步的。那么，ISR中存在是什么副本呢？首先可以明确的是：Leader副本总是存在于ISR中。而follower副本是否在ISR中，取决于该follower副本是否与Leader副本保持了“同步”。

Kafka的broker端有一个参数replica.lag.time.max.ms, 该参数表示follower副本滞后与Leader副本的最长时间间隔，默认是10秒。这就意味着，只要follower副本落后于leader副本的时间间隔不超过10秒，就可以认为该follower副本与leader副本是同步的，所以哪怕当前follower副本落后于Leader副本几条消息，只要在10秒之内赶上Leader副本，就不会被踢出出局。

可以看出ISR是一个动态的，所以即便是为分区配置了3个副本，还是会出现同步副本列表中只有一个副本的情况(其他副本由于不能够与leader及时保持同步，被移出ISR列表)。如果这个同步副本变为不可用，我们必须在可用性和一致性之间作出选择(CAP理论)。

根据Kafka 对可靠性保证的定义，消息只有在被写入到所有同步副本之后才被认为是已提交的。但如果这里的“所有副本”只包含一个同步副本，那么在这个副本变为不可用时，数据就会丢失。如果要确保已提交的数据被写入不止一个副本，就需要把最小同步副本数量设置为大一点的值。对于一个包含3 个副本的主题分区，如果min.insync.replicas=2 ，那么至少要存在两个同步副本才能向分区写入数据。

如果进行了上面的配置，此时必须要保证ISR中至少存在两个副本，如果ISR中的副本个数小于2，那么Broker就会停止接受生产者的请求。尝试发送数据的生产者会收到NotEnoughReplicasException异常，消费者仍然可以继续读取已有的数据。

禁用unclean选举

选择一个同步副本列表中的分区作为leader 分区的过程称为clean leader election。注意，这里要与在非同步副本中选一个分区作为leader分区的过程区分开，在非同步副本中选一个分区作为leader的过程称之为unclean leader election。由于ISR是动态调整的，所以会存在ISR列表为空的情况，通常来说，非同步副本落后 Leader 太多，因此，如果选择这些副本作为新 Leader，就可能出现数据的丢失。毕竟，这些副本中保存的消息远远落后于老 Leader 中的消息。在 Kafka 中，选举这种副本的过程可以通过Broker 端参数 unclean.leader.election.enable控制是否允许 Unclean 领导者选举。开启 Unclean 领导者选举可能会造成数据丢失，但好处是，它使得分区 Leader 副本一直存在，不至于停止对外提供服务，因此提升了高可用性。反之，禁止 Unclean Leader 选举的好处在于维护了数据的一致性，避免了消息丢失，但牺牲了高可用性。分布式系统的CAP理论说的就是这种情况。

不幸的是，unclean leader election的选举过程仍可能会造成数据的不一致，因为同步副本并不是完全同步的。由于复制是异步完成的，因此无法保证follower可以获取最新消息。比如Leader分区的最后一条消息的offset是100，此时副本的offset可能不是100，这受到两个参数的影响：

>replica.lag.time.max.ms：同步副本滞后与leader副本的时间 zookeeper.session.timeout.ms：与zookeeper会话超时时间

简而言之，如果我们允许不同步的副本成为leader，那么就要承担丢失数据和出现数据不一致的风险。如果不允许它们成为leader，那么就要接受较低的可用性，因为我们必须等待原先的首领恢复到可用状态。

关于unclean选举，不同的场景有不同的配置方式。对数据质量和数据一致性要求较高的系统会禁用这种unclean的leader选举(比如银行)。如果在可用性要求较高的系统里，比如实时点击流分析系统， 一般不会禁用unclean的leader选举。

# 19. 如何解决 Kafka 数据丢失问题？
从Kafka的生产者与消费者的角度来看待数据丢失的问题。

**Producer**

1、retries=Long.MAX_VALUE

设置retries为一个较大的值。这里的retries同样是Producer的参数，对应前面提到的Producer自动重试。当出现网络的瞬时抖动时，消息发送可能会失败，此时配置了retries>0的Producer能够自动重试消息发送，避免消息丢失。

2、acks=all

设置acks=all。acks是Producer的一个参数，代表了你对“已提交”消息的定义。如果设置成all，则表明所有副本Broker都要接收到消息，该消息才算是“已提交”。这是最高等级的“已提交”定义。

3、max.in.flight.requests.per.connections=1

该参数指定了生产者在收到服务器晌应之前可以发送多少个消息。它的值越高，就会占用越多的内存，不过也会提升吞吐量。把它设为1可以保证消息是按照发送的顺序写入服务器的，即使发生了重试。

4、Producer要使用带有回调通知的API，也就是说不要使用producer.send(msg)，而要使用producer.send(msg,callback)。

5、其他错误处理

使用生产者内置的重试机制，可以在不造成消息丢失的情况下轻松地处理大部分错误，不过仍然需要处理其他类型的错误，例如消息大小错误、序列化错误等等。

**Consumer**

1、禁用自动提交：enable.auto.commit=false

2、消费者处理完消息之后再提交offset

3、配置auto.offset.reset

这个参数指定了在没有偏移量可提交时(比如消费者第l次启动时)或者请求的偏移量在broker上不存在时(比如数据被删了)，消费者会做些什么。

这个参数有两种配置。一种是earliest：消费者会从分区的开始位置读取数据，不管偏移量是否有效，这样会导致消费者读取大量的重复数据，但可以保证最少的数据丢失。一种是latest(默认)，如果选择了这种配置，消费者会从分区的末尾开始读取数据，这样可以减少重复处理消息，但很有可能会错过一些消息。

# 20. Kafka 能否保证永久不丢失数据吗？
Kafka只对“已提交”的消息（committed message）做有限度的持久化保证。所以说，Kafka不能够完全保证数据不丢失，需要做出一些权衡策略。

什么是已提交的消息？

当Kafka的若干个Broker成功地接收到一条消息并写入到日志文件后，它们会告诉生产者程序这条消息已成功提交。此时，这条消息在Kafka看来就正式变为已提交消息了。所以说无论是ack=all，还是ack=1，不论哪种情况，Kafka只对已提交的消息做持久化保证这件事情是不变的。

有限度的持久化保证消息不丢失，也就是说Kafka不可能保证在任何情况下都做到不丢失消息。必须保证Kafka的Broker是可用的，换句话说，假如消息保存在N个Kafka Broker上，那么这个前提条件就是这N个Broker中至少有 1个存活。只要这个条件成立，Kafka就能保证你的这条消息永远不会丢失。

总结的说就是Kafka是能做到不丢失消息的，只不过这些消息必须是已提交的消息，而且还要满足一定的条件。
