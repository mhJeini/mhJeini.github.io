# 1. chubby 和 zookeeper 有哪些区别？
chubby是google的，完全实现paxos算法，不开源。

zookeeper是基于chubby的开源实现，使用zab协议，paxos算法的变种。

# 2. ZooKeeper 提供了什么？
1、文件系统

2、通知机制

# 3. Zookeeper 和文件系统有哪些区别？
Zookeeper提供一个多层级的节点命名空间（节点称为znode）。与文件系统不同的是，这些节点都可以设置关联的数据，而文件系统中只有文件节点可以存放数据而目录节点不行。

Zookeeper为了保证高吞吐和低延迟，在内存中维护了这个树状的目录结构，这种特性使得Zookeeper 不能用于存放大量的数据，每个节点的存放数据上限为1M。

# 4. Zookeeper 中什么是 ZAB 协议？
ZAB协议是为分布式协调服务Zookeeper专门设计的一种支持崩溃恢复的原子广播协议。

ZAB协议包括两种基本的模式：崩溃恢复和消息广播。

崩溃恢复：在正常情况下运行非常良好，一旦Leader出现崩溃或者由于网络原因导致Leader服务器失去了与过半Follower的联系，那么就会进入崩溃恢复模式。为了程序的正确运行，整个恢复过程后需要选举出一个新的Leader,因此需要一个高效可靠的选举方法快速选举出一个Leader。

消息广播：类似一个两阶段提交过程，针对客户端的事务请求， Leader服务器会为其生成对应的事务Proposal,并将其发送给集群中的其余所有机器，再分别收集各自的选票，最后进行事务提交。

当整个zookeeper集群刚刚启动或者Leader服务器宕机、重启或者网络故障导致不存在过半的服务器与Leader服务器保持正常通信时，所有进程（服务器）进入崩溃恢复模式，首先选举产生新的Leader服务器，然后集群中Follower服务器开始与新的Leader服务器进行数据同步，当集群中超过半数机器与该Leader服务器完成数据同步之后，退出恢复模式进入消息广播模式，Leader服务器开始接收客户端的事务请求生成事物提案来进行事务请求处理。

# 5. Zookeeper 中 Watcher 工作机制和特性？
Zookeeper允许客户端向服务端的某个Znode注册一个Watcher监听，当服务端的一些指定事件触发了这个Watcher，服务端会向指定客户端发送一个事件通知来实现分布式的通知功能，然后客户端根据Watcher通知状态和事件类型做出业务上的改变。

**工作机制**

客户端注册watcher 服务端处理watcher 客户端回调watcher

**Watcher特性**

1）一次性

无论是服务端还是客户端，一旦一个Watcher被触发，Zookeeper都会将其从相应的存储中移除。这样的设计有效的减轻了服务端的压力，不然对于更新非常频繁的节点，服务端会不断的向客户端发送事件通知，无论对于网络还是服务端的压力都非常大。

2）客户端串行执行

客户端Watcher回调的过程是一个串行同步的过程。

3）轻量

Watcher通知非常简单，只会告诉客户端发生了事件，而不会说明事件的具体内容。 客户端向服务端注册Watcher的时候，并不会把客户端真实的Watcher对象实体传递到服务端，仅仅是在客户端请求中使用boolean类型属性进行了标记。

watcher event异步发送watcher的通知事件从server发送到client是异步的，这就存在一个问题，不同的客户端和服务器之间通过socket进行通信，由于网络延迟或其他因素导致客户端在不通的时刻监听到事件，由于Zookeeper本身提供了ordering guarantee，即客户端监听事件后，才会感知它所监视znode发生了变化。所以我们使用Zookeeper不能期望能够监控到节点每次的变化。Zookeeper只能保证最终的一致性，而无法保证强一致性。

注册watcher getData、exists、getChildren

触发watcher create、delete、setData

当一个客户端连接到一个新的服务器上时，watch将会被以任意会话事件触发。当与一个服务器失去连接的时候，是无法接收到watch的。而当client重新连接时，如果需要的话，所有先前注册过的watch，都会被重新注册。通常这是完全透明的。只有在一个特殊情况下，watch可能会丢失：对于一个未创建的znode的exist watch，如果在客户端断开连接期间被创建了，并且随后在客户端连接上之前又删除了，这种情况下，这个watch事件可能会被丢失。

# 6. Zookeeper 中 Chroot 特性有什么作用？
Zookeeper3.2.0版本后，添加了Chroot特性，该特性允许每个客户端为自己设置一个命名空间。

如果一个客户端设置了Chroot，那么该客户端对服务器的任何操作，都将会被限制在其自己的命名空间下。

通过设置Chroot，能够将一个客户端应用于Zookeeper服务端的一颗子树相对应，在那些多个应用公用一个Zookeeper进群的场景下，对实现不同应用间的相互隔离非常有帮助。

# 7. Zookeeper 中会话管理使用什么策略和分配原则？
分桶策略：将类似的会话放在同一区块中进行管理，以便于Zookeeper对会话进行不同区块的隔离处理以及同一区块的统一处理。

分配原则：每个会话的“下次超时时间点”（ExpirationTime）

计算公式：
```java
ExpirationTime_ = currentTime + sessionTimeout
ExpirationTime = (ExpirationTime_ / ExpirationInrerval + 1) * ExpirationInterval
```
ExpirationInterval是指Zookeeper会话超时检查时间间隔，默认tickTime
# 8. Zookeeper 中都有哪些服务器角色？
**Leader**

Leader服务器是整个ZooKeeper集群工作机制中的核心,其主要工作有以下两个。

1）事务请求的唯一调度和处理者，保证集群事务处理的顺序性。

2）集群内部各服务器的调度者。

**Follower**

从角色名字上可以看出，Follower服务器是ZooKeeper集群状态的跟随者,其主要工作有以下三个。

1）处理客户端的非事务请求，转发事务请求给Leader服务器。

2）参与事务请求Proposal的投票。

3）参与Leader选举投票。

**Observer**

Zookeeper3.3.0版本以后引入的一个全新的服务器角色，在不影响集群事务处理能力的基础上提升集群的非事务处理能力从字面意思看，该服务器充当了一个观察者的角色—其观察ZooKeeper集群的最新状态变化并将这些状态变更同步过来。

1）Observer服务器在工作原理上和Follower基本是一致的，对于非事务请求，都可以进行独立的处理，而对于事务请求，则会转发给Leader服务器进行处理。

2）对比Follower唯一的区别是Observer不参与任何形式的投票，包括事务请求Proposal的投票和Leader选举投票。简单地讲，Observer服务器只提供非事务服务，通常用于在不影响集群事务处理能力的前提下提升集群的非事务处理能力。另外，Observer的请求处理链路和Follower服务器也非常相近。

# 9. Zookeeper 队列有哪些类型？
两种类型的队列： 1、同步队列，当一个队列的成员都聚齐时，这个队列才可用，否则一直等待所有成员到达。 2、队列按照 FIFO 方式进行入队和出队操作。 第一类，在约定目录下创建临时目录节点，监听节点数目是否是我们要求的数目。 第二类，和分布式锁服务中的控制时序场景基本原理一致，入列有编号，出列按编号。在特定的目录下创建PERSISTENT_SEQUENTIAL节点，创建成功时Watcher通知等待的队列，队列删除序列号最小的节点用以消费。此场景下Zookeeper的znode用于消息存储，znode存储的数据就是消息队列中的消息内容，SEQUENTIAL序列号就是消息的编号，按序取出即可。由于创建的节点是持久化的，所以不必担心队列消息的丢失问题。

# 10. Zookeeper 中如何实现通知机制的？
client端会对某个znode建立一个watcher事件，当该znode发生变化时，这些client会收到zk的通知，然后client可以根据znode变化来做出业务上的改变等。

# 11. Zookeeper 中对节点是永久watch监听通知吗？
不是。官方声明：一个Watch事件是一个一次性的触发器，当被设置了Watch的数据发生了改变的时候，则服务器将这个改变发送给设置了Watch的客户端，以便通知它们。

至于为什么不是永久的，简单举个例子，如果服务端变动频繁，而监听的客户端很多情况下，每次变动都要通知到所有的客户端，给网络和服务器造成很大压力。

一般是客户端执行getData(“/节点A”,true)，如果节点A发生了变更或删除，客户端会得到它的watch事件，但是在之后节点A又发生了变更，而客户端又没有设置watch事件，就不再给客户端发送。 在实际应用中，很多情况下，我们的客户端不需要知道服务端的每一次变动，我只要最新的数据即可。

# 12. 说一说 Zookeeper 中数据同步流程？
整个集群完成Leader选举之后，Learner（Follower和Observer的统称）回向Leader服务器进行注册。当Learner服务器想Leader服务器完成注册后，进入数据同步环节。

数据同步流程：（均以消息传递的方式进行）

>1）Learner向Learder注册 2）数据同步 3）同步确认

Zookeeper的数据同步通常分为四类：

>直接差异化同步（DIFF同步） 先回滚再差异化同步（TRUNC+DIFF同步） 仅回滚同步（TRUNC同步） 全量同步（SNAP同步） 在进行数据同步前，Leader服务器会完成数据同步初始化：

peerLastZxid：从learner服务器注册时发送的ACKEPOCH消息中提取lastZxid（该Learner服务器最后处理的ZXID）

minCommittedLog：Leader服务器Proposal缓存队列committedLog中最小ZXID

maxCommittedLog：Leader服务器Proposal缓存队列committedLog中最大ZXID

**直接差异化同步（DIFF同步）**

场景：peerLastZxid介于minCommittedLog和maxCommittedLog之间

**先回滚再差异化同步（TRUNC+DIFF同步）**

场景：当新的Leader服务器发现某个Learner服务器包含了一条自己没有的事务记录，那么就需要让该Learner服务器进行事务回滚--回滚到Leader服务器上存在的，同时也是最接近于peerLastZxid的ZXID

**仅回滚同步（TRUNC同步）**

场景：peerLastZxid 大于 maxCommittedLog

**全量同步（SNAP同步）**

场景一：peerLastZxid 小于 minCommittedLog

场景二：Leader服务器上没有Proposal缓存队列且peerLastZxid不等于lastProcessZxid

# 13. 说一说 Zookeeper 工作原理？
Zookeeper的核心是原子广播，这个机制保证了各个Server之间的同步。

实现这个机制的协议叫做Zab协议。

Zab协议有两种模式，它们分别是恢复模式（选主）和广播模式（同步）。

当服务启动或者在领导者崩溃后，Zab就进入了恢复模式，当领导者被选举出来，且大多数Server完成了和 leader的状态同步以后，恢复模式就结束了。

状态同步保证了leader和Server具有相同的系统状态。

为了保证事务的顺序一致性，zookeeper采用了递增的事务id号（zxid）来标识事务。

所有的提议（proposal）都在被提出的时候加上了zxid。

实现中zxid是一个64位的数字，它高32位是epoch用来标识leader关系是否改变，每次一个leader被选出来，它都会有一个新的epoch，标识当前属于那个leader的统治时期。低32位用于递增计数。

# 14. Zookeeper 中数据复制有什么优点？
Zookeeper作为一个集群提供一致的数据服务，自然，它要在所有机器间做数据复制。

数据复制的有点：

1、容错：一个节点出错，不致于让整个系统停止工作，别的节点可以接管它的工作；

2、提高系统的扩展能力 ：把负载分布到多个节点上，或者增加节点来提高系统的负载能力；

3、提高性能：让客户端本地访问就近的节点，提高用户访问速度。

# 15. Zookeeper 中如何选取主 leader 的？
当leader崩溃或者leader失去大多数的follower，这时zk进入恢复模式，恢复模式需要重新选举出一个新的leader，让所有的Server都恢复到一个正确的状态。

Zk的选举算法有两种：一种是基于basic paxos实现的，另外一种是基于fast paxos算法实现的。系统默认的选举算法为fast paxos。

1、Zookeeper选主流程(basic paxos)

1）选举线程由当前Server发起选举的线程担任，其主要功能是对投票结果进行统计，并选出推荐的Server；

2）选举线程首先向所有Server发起一次询问(包括自己)；

3）选举线程收到回复后，验证是否是自己发起的询问(验证zxid是否一致)，然后获取对方的id(myid)，并存储到当前询问对象列表中，最后获取对方提议的leader相关信息(id,zxid)，并将这些信息存储到当次选举的投票记录表中；

4）收到所有Server回复以后，就计算出zxid最大的那个Server，并将这个Server相关信息设置成下一次要投票的Server；

5）线程将当前zxid最大的Server设置为当前Server要推荐的Leader，如果此时获胜的Server获得n/2 + 1的Server票数，设置当前推荐的leader为获胜的Server，将根据获胜的Server相关信息设置自己的状态，否则，继续这个过程，直到leader被选举出来。 通过流程分析我们可以得出：要使Leader获得多数Server的支持，则Server总数必须是奇数2n+1，且存活的Server的数目不得少于n+1. 每个Server启动后都会重复以上流程。在恢复模式下，如果是刚从崩溃状态恢复的或者刚启动的server还会从磁盘快照中恢复数据和会话信息，zk会记录事务日志并定期进行快照，方便在恢复时进行状态恢复。

# 16. ZooKeeper 集群中如何实现服务器之间通信？
Leader服务器会和每一个Follower/Observer服务器都建立TCP连接，同时为每个F/O都创建一个叫做LearnerHandler的实体。

LearnerHandler主要负责Leader和F/O之间的网络通讯，包括数据同步，请求转发和Proposal提议的投票等。

Leader服务器保存了所有F/O的LearnerHandler。

# 17. Zookeeper 中支持自动清理日志？
Zookeeper不支持自动清理日志，需要运维人员手动或编写shell脚本清理日志。

# 18. ZooKeeper 中节点增多时，什么情况导致 PtBalancer 速度变慢？
1）ZK有1MB 的传输限制。 实践中ZNode必须相对较小，而队列包含成千上万的消息，非常的大。

2）如果有很多节点，ZK启动时相当的慢。而使用queue会导致好多ZNode，需要显著增大initLimit和syncLimit。

3）ZNode很大的时候很难清理。Netflix不得不创建了一个专门的程序做这事。

4）当很大量的包含成千上万的子节点的ZNode时， ZK的性能变得不好。

5）ZK的数据库完全放在内存中。大量的Queue意味着会占用很多的内存空间。

尽管如此，Curator还是创建了各种Queue的实现。如果Queue的数据量不太多，数据量不太大的情况下，酌情考虑，还是可以使用。

# 19. ZooKeeper 中支持临时节点创建子节点吗？
ZooKeeper不支持临时节点创建子节点。

# 20. ZooKeeper 中什么情况下删除临时节点？
如果连接断开后，ZK不会马上移除临时数据，只有当SESSIONEXPIRED之后，才会把这个会话建立的临时数据移除。因此，用户需要谨慎设置Session_TimeOut。