# 1. Kafka 中如何保证消息的有序性？
Kafka的主题是分区有序的，如果一个主题有多个分区，那么Kafka会按照key将其发送到对应的分区中，所以，对于给定的key与其对应的record在分区内是有序的。

Kafka可以保证同一个分区里的消息是有序的，即生产者按照一定的顺序发送消息，Broker就会按照这个顺序将他们写入对应的分区中，同理，消费者也会按照这个顺序来消费他们。

在一些场景下，消息的顺序是非常重要的。比如，先存钱再取钱与先取钱再存钱是截然不同的两种结果。

上面的问题中提到一个参数max.in.flight.requests.per.connections=1，该参数的作用是在重试次数大于等于1时，保证数据写入的顺序。如果该参数不为1，那么当第一个批次写入失败时，第二个批次写入成功，Broker会重试写入第一个批次，如果此时第一个批次重试写入成功，那么这两个批次消息的顺序就反过来了。

一般来说，如果对消息的顺序有要求，那么在为了保障数据不丢失，需要先设置发送重试次数retries>0，同时需要把max.in.flight.requests.per.connections参数设为1，这样在生产者尝试发送第一批消息时，就不会有其他的消息发送给broker，虽然会影响吞吐量，但是可以保证消息的顺序。

除此之外，还可以使用单分区的Topic，但是会严重影响吞吐量。

# 2. Kafka 中如何查看消费者组是否存在滞后消费？
使用kafka-consumer-groups.sh命令进行查看。

```shell
$ bin/kafka-consumer-groups.sh --bootstrap-server cdh02:9092 --describe --group my-group
## 会显示下面的一些指标信息
TOPIC PARTITION CURRENT-OFFSET LOG-END-OFFSET   LAG          CONSUMER-ID HOST CLIENT-ID
主题   分区       当前offset      LEO           滞后消息数       消费者id     主机   客户端id
```
一般情况下，如果运行良好，CURRENT-OFFSET的值会和LOG-END-OFFSET的值非常接近。通过这个命令可以查看哪个分区的消费出现了滞后消费的情况。

# 3. Kafka 中能否手动删除消息吗？
Kafka不需要用户手动删除消息。它本身提供了留存策略，能够自动删除过期消息。当然，它是支持手动删除消息的。

对于设置了Key且参数cleanup.policy=compact的主题而言，可以构造一条<Key，null>的消息发送给Broker，依靠Log Cleaner组件提供的功能删除掉该Key的消息。

对于普通主题而言，可以使用kafka-delete-records命令，或编写程序调用Admin.deleteRecords方法来删除消息。

这两种方法殊途同归，底层都是调用Admin的deleteRecords方法，通过将分区Log Start Offset值抬高的方式间接删除消息。

# 4. Kafka 中使用零拷贝（Zero Copy）有哪些场景？
Kafka中体现零拷贝使用场景的地方有两处：基于mmap的索引和日志文件读写所用的TransportLayer。

基于mmap的索引

索引都是基于MappedByteBuffer的，也就是让用户态和内核态共享内核态 的数据缓冲区，此时，数据不需要复制到用户态空间。不过，mmap 虽然避免了不必要的 拷贝，但不一定就能保证很高的性能。在不同的操作系统下，mmap 的创建和销毁成本可 能是不一样的。很高的创建和销毁开销会抵消 零拷贝 带来的性能优势。由于这种不确 定性，在Kafka中，只有索引应用了mmap，最核心的日志并未使用mmap机制。

日志文件读写所用的TransportLayer

TransportLayer是Kafka传输层的接口。它的某个实现类使用了FileChannel的transferTo方法。该方法底层使用sendfile实现了零拷贝。对Kafka而言，如果I/O通道使用普通的PLAINTEXT，那么，Kafka就可以利用零拷贝特性，直接将页缓存中的数据发送到网卡的 Buffer 中，避免中间的多次拷贝。相反，如果I/O通道启用了SSL，那么，Kafka便无法利用零拷贝特性了。

# 5. Kafka 中如何获取 topic 主题的列表？
获取topic主题的列表命令：

```shell
bin/kafka-topics.sh --list --zookeeper localhost:2181
```
# 6. Kafka 中生产者和消费者的命令行是什么？
生产者在主题上发布消息：

```shell
bin/kafka-console-producer.sh --broker-list 192.168.43.49:9092 --topic Hello-Kafka
```
注意这里的IP是server.properties中的 listeners的配置。接下来每个新行就是 输入一条新消息。

消费者接受消息：

```shell
bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic Hello-Kafka --from-beginning
```
# 7. Kafka 中 consumer 是推还是拉？
Kafka最初考虑的问题是，customer应该从brokes拉取消息还是brokers将消息推送到consumer，也就是pull还push。在这方面，Kafka遵循了一种大部分消息系统共同的传统的设计：producer将消息推送到broker，consumer从broker拉取消息。

一些消息系统比如Scribe和Apache Flume采用了push模式，将消息推送到下游的consumer。这样做有好处也有坏处：由broker决定消息推送的速率，对于不同消费速率的consumer就不太好处理了。消息系统都致力于让consumer以最大的速率最快速的消费消息，但不幸的是，push模式下，当broker推送的速率远大于consumer消费的速率时，consumer恐怕就要崩溃了。最终Kafka还是选取了传统的pull模式。

Pull模式的另外一个好处是consumer可以自主决定是否批量的从broker拉取数据。Push模式必须在不知道下游consumer消费能力和消费策略的情况下决定是立即推送每条消息还是缓存之后批量推送。如果为了避免consumer崩溃而采用较低的推送速率，将可能导致一次只推送较少的消息而造成浪费。Pull模式下，consumer就可以根据自己的消费能力去决定这些策略。

Pull有个缺点是，如果broker没有可供消费的消息，将导致consumer不断在循环中轮询，直到新消息到t达。为了避免这点，Kafka有个参数可以让consumer阻塞知道新消息到达(当然也可以阻塞知道消息的数量达到某个特定的量这样就可以批量发送)。

# 8. 说说 Kafka 维护消费状态跟踪的方法？
大部分消息系统在broker端的维护消息被消费的记录：一个消息被分发到consumer后broker就马上进行标记或者等待customer的通知后进行标记。这样也可以在消息在消费后立马就删除以减少空间占用。

但是这样会不会有什么问题呢？如果一条消息发送出去之后就立即被标记为消费过的，一旦consumer处理消息时失败了（比如程序崩溃）消息就丢失了。为了解决这个问题，很多消息系统提供了另外一个个功能：当消息被发送出去之后仅仅被标记为已发送状态，当接到consumer已经消费成功的通知后才标记为已被消费的状态。这虽然解决了消息丢失的问题，但产生了新问题，首先如果consumer处理消息成功了但是向broker发送响应时失败了，这条消息将被消费两次。第二个问题时，broker必须维护每条消息的状态，并且每次都要先锁住消息然后更改状态然后释放锁。这样麻烦又来了，且不说要维护大量的状态数据，比如如果消息发送出去但没有收到消费成功的通知，这条消息将一直处于被锁定的状态，Kafka采用了不同的策略。Topic被分成了若干分区，每个分区在同一时间只被一个consumer消费。这意味着每个分区被消费的消息在日志中的位置仅仅是一个简单的整数：offset。这样就很容易标记每个分区消费状态就很容易了，仅仅需要一个整数而已。这样消费状态的跟踪就很简单了。

这带来了另外一个好处：consumer可以把offset调成一个较老的值，去重新消费老的消息。这对传统的消息系统来说看起来有些不可思议，但确实是非常有用的，谁规定了一条消息只能被消费一次呢。

# 9. Zookeeper 对于 Kafka 的作用是什么？
Zookeeper是一个开放源码的、高性能的协调服务，它用于Kafka的分布式应用。

Zookeeper主要用于在集群中不同节点之间进行通信。

在Kafka中，它被用于提交偏移量，因此如果节点在任何情况下都失败了，它都可以从之前提交的偏移量中获取。

除此之外，它还执行其他活动，如:leader检测、分布式同步、配置管理、识别新节点何时离开或连接、集群、节点实时状态等等。

# 10. Kafka 判断一个节点是否还活着有那两个条件？
1）节点必须可以维护和ZooKeeper的连接，Zookeeper通过心跳机制检查每个节点的连接；

2）如果节点是个follower,他必须能及时的同步leader的写操作，延时不能太久。

# 11. Kafka 与传统 MQ 消息队列之间有三个关键区别？
1）Kafka持久化日志，这些日志可以被重复读取和无限期保留。

2）Kafka是一个分布式系统：它以集群的方式运行，可以灵活伸缩，在内部通过复制数据提升容错能力和高可用性。

3）Kafka支持实时的流式处理。

# 12. 说一说 Kafka 中 ack 的三种机制？
request.required.acks 有三个值 0 1 -1(all)

0：生产者不会等待 broker 的 ack，这个延迟最低但是存储的保证最弱当 server 挂 掉的时候就会丢数据。

1：服务端会等待 ack 值 leader 副本确认接收到消息后发送 ack 但是如果 leader 挂掉后他不确保是否复制完成新 leader 也会导致数据丢失。

-1(all)：服务端会等所有的 follower 的副本受到数据后才会受到 leader 发出的 ack，这样数据不会丢失。

# 13. Kafka 消费者如何不自动提交偏移量，由应用提交？
将auto.commit.offset设为false，然后在处理一批消息后commitSync()或者异步提交commitAsync()即：

```shell
ConsumerRecords<> records = consumer.poll();
    for(ConsumerRecord<> record : records){
    try{
     consumer.commitSync();
    }
}
```
# 14. Kafka 消费者故障，出现活锁问题如何解决？
出现“活锁”的情况，是它持续的发送心跳，但是没有处理。为了预防消费者在 这种情况下一直持有分区，我们使用max.poll.interval.ms 活跃检测机制。在此基础上，如果调用的poll的频率大于最大间隔，则客户端将主动地离开组，以 便其他消费者接管该分区。发生这种情况时，你会看到offset提交失败（调用commitSync()引发的CommitFailedException）。这是一种安全机制，保障 只有活动成员能够提交 offset。所以要留在组中，你必须持续调用poll。

消费者提供两个配置设置来控制poll循环：

max.poll.interval.ms：增大poll的间隔，可以为消费者提供更多的时间去处理返 回的消息（调用poll(long)返回的消息，通常返回的消息都是一批）。缺点是此值 越大将会延迟组重新平衡。

max.poll.records：此设置限制每次调用poll返回的消息数，这样可以更容易的预测每次poll间隔要处理的最大值。通过调整此值，可以减少poll间隔，减少重新平衡分组。

对于消息处理时间不可预测地的情况，这些选项是不够的。处理这种情况的推荐 方法是将消息处理移到另一个线程中，让消费者继续调用poll。

但是必须注意确 保已提交的offset不超过实际位置。另外，你必须禁用自动提交，并只有在线程 完成处理后才为记录手动提交偏移量（取决于你）。

还要注意，你需要pause暂停分区，不会从poll接收到新消息，让线程处理完之前返回的消息（如果你的处理能力比拉取消息的慢，那创建新线程将导致你机器内存溢出）。

# 15. Kafka 中如何控制消费的位置？
kafka使用seek(TopicPartition, long)指定新的消费位置。

用于查找服务器保留的最早和最新的offset 的特殊的方法也可用。seekToBeginning(Collection)和seekToEnd(Collection)

# 16. Kafka分布式的情况下，如何保证消息的顺序消费？
Kafka分布式的单位是partition，同一个partition用一个write ahead log组织，所以可以保证FIFO的顺序。不同partition之间不能保证顺序。但是绝大多数用户都可以通过message key来定义，因为同一个key的message可以保证只发送到同一个partition。

Kafka中发送1条消息的时候，可以指定(topic,partition,key)3个参数。partiton和key是可选的。如果你指定了partition，那就是所有消息发往同1个partition，就是有序的。并且在消费端，Kafka保证，1个partition只能被1个consumer消费。或者你指定key（比如orderid），具有同1个key的所有消息，会发往同1个partition。

# 17. RabbitMQ 和 Kafka 有什么区别？
1、吞吐量

kafka吞吐量更高： 1）Zero Copy机制，内核copy数据直接copy到网络设备，不必经过内核到用户再到内核的copy，减小了copy次数和上下文切换次数，大大提高了效率。 2）磁盘顺序读写，减少了寻道等待的时间。 3）批量处理机制，服务端批量存储，客户端主动批量pull数据，消息处理效率高。 4）存储具有O(1)的复杂度，读物因为分区和segment，是O(log(n))的复杂度。 5）分区机制，有助于提高吞吐量。

2、可靠性

rabbitmq可靠性更好： 1）确认机制（生产者和exchange，消费者和队列）； 2）支持事务，但会造成阻塞； 3）委托（添加回调来处理发送失败的消息）和备份交换器（将发送失败的消息存下来后面再处理）机制；

3、高可用

1）rabbitmq采用mirror queue，即主从模式，数据是异步同步的，当消息过来，主从全部写完后，回ack，这样保障了数据的一致性。 2）每个分区都可以有一个或多个副本，这些副本保存在不同的broker上，broker信息存储在zookeeper上，当broker不可用会重新选举leader。 kafka支持同步负责消息和异步同步消息（有丢消息的可能），生产者从zk获取leader信息，发消息给leader，follower从leader pull数据然后回ack给leader。

4、负责均衡

1）kafka通过zk和分区机制实现：zk记录broker信息，生产者可以获取到并通过策略完成负载均衡；通过分区，投递消息到不同分区，消费者通过服务组完成均衡消费。 2）需要外部支持。

5、模型

1）rabbitmq： producer，broker遵循AMQP（exchange，bind，queue），consumer； broker为中心，exchange分topic，direct，fanout和header，路由模式适合多种场景； consumer消费位置由broker通过确认机制保存；

2）kafka： producer，broker，consumer，未遵循AMQP； consumer为中心，获取消息模式由consumer自身决定； offset保存在消费者这边，broker无状态； 消息是名义上的永久存储，每个parttition按segment保存自己的消息为文件（可配置清理周期）； consumer可以通过重置offset消费历史消息； 需要绑定zk；

# 18. Kafka follower 如何与 leader 同步数据？
Kafka的复制机制既不是完全的同步复制，也不是单纯的异步复制。完全同步复制要求All Alive Follower都复制完，这条消息才会被认为commit，这种复制方式极大的影响了吞吐率。

而异步复制方式下，Follower异步的从Leader复制数据，数据只要被Leader写入log就被认为已经commit，这种情况下，如果leader挂掉，会丢失数据，kafka使用ISR的方式很好的均衡了确保数据不丢失以及吞吐率。

Follower可以批量的从Leader复制数据，而且Leader充分利用磁盘顺序读以及send file(zero copy)机制，这样极大的提高复制性能，内部批量写磁盘，大幅减少了Follower与Leader的消息量差。

# 19. 为什么要使用 Kafka，为什么要使用消息队列？
缓冲和削峰：上游数据时有突发流量，下游可能扛不住，或者下游没有足够多的机器来保证冗余，kafka在中间可以起到一个缓冲的作用，把消息暂存在kafka中，下游服务就可以按照自己的节奏进行慢慢处理。

解耦和扩展性：项目开始的时候，并不能确定具体需求。消息队列可以作为一个接口层，解耦重要的业务流程。只需要遵守约定，针对数据编程即可获取扩展能力。

冗余：可以采用一对多的方式，一个生产者发布消息，可以被多个订阅topic的服务消费到，供多个毫无关联的业务使用。

健壮性：消息队列可以堆积请求，所以消费端业务即使短时间死掉，也不会影响主要业务的正常进行。

异步通信：很多时候，用户不想也不需要立即处理消息。消息队列提供了异步处理机制，允许用户把一个消息放入队列，但并不立即处理它。想向队列中放入多少消息就放多少，然后在需要的时候再去处理它们。

# 20. Kafka 中 ISR、AR 表示什么意思？ISR 伸缩指什么？
ISR：In-Sync Replicas 副本同步队列。

AR：Assigned Replicas 所有副本。

ISR是由leader维护，follower从leader同步数据有一些延迟（包括延迟时间replica.lag.time.max.ms和延迟条数replica.lag.max.messages两个维度, 当前最新的版本0.10.x中只支持replica.lag.time.max.ms这个维度），任意一个超过阈值都会把follower剔除出ISR, 存入OSR（Outof-Sync Replicas）列表，新加入的follower也会先存放在OSR中。AR=ISR+OSR。
