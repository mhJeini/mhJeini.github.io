# 1. 什么是 Zookeeper？
ZooKeeper由雅虎研究院开发，ZooKeeper是一个分布式的，开放源码的分布式应用程序协调服务，是Google的Chubby一个开源的实现，后来托管到Apache，是Hadoop和Hbase的重要组件。

ZooKeeper是一个经典的分布式数据一致性解决方案，致力于为分布式应用提供一个高性能、高可用，且具有严格顺序访问控制能力的分布式协调服务。

ZooKeeper的目标就是封装好复杂易出错的关键服务，将简单易用的接口和性能高效、功能稳定的系统提供给用户。

ZooKeeper包含一个简单的原语集，提供Java和C的接口。

ZooKeeper代码版本中，提供了分布式独享锁、选举、队列的接口，代码在$zookeeper_home\src\recipes。其中分布锁和队列有Java和C两个版本，选举只有Java版本。

于2010年11月正式成为Apache的顶级项目。它是一个为分布式应用提供一致性服务的软件，提供的功能包括：配置维护、域名服务、分布式同步、组服务等。

分布式应用程序可以基于ZooKeeper实现数据发布与订阅、负载均衡、命名服务、分布式协调与通知、集群管理、Leader选举、分布式锁、分布式队列等功能。

# 2. Zookeeper 怎么保证主从节点的状态同步？
Zookeeper的核心是原子广播机制，这个机制保证了各个server之间的同步。实现这个机制的协议叫做Zab协议。

Zab协议有两种模式，分别是恢复模式和广播模式。

**恢复模式**

当服务启动或者在领导者崩溃后，Zab就进入了恢复模式，当领导者被选举出来，且大多数server完成了和leader的状态同步以后，恢复模式就结束了。

状态同步保证了leader和server具有相同的系统状态。

**广播模式**

一旦leader已经和多数的follower进行了状态同步后，它就可以开始广播消息了，即进入广播状态。

此时当一个server加入ZooKeeper服务中，它会在恢复模式下启动，发现leader，并和leader进行状态同步。

待到同步结束，它也参与消息广播。

ZooKeeper服务一直维持在Broadcast状态，直到leader崩溃了或者leader失去了大部分的followers支持。

# 3. Zookeeper 是如何保证事务的顺序一致性的？
Zookeeper采用了全局递增的事务Id来标识，所有的proposal（提议）都在被提出的时候加上了zxid。

zxid实际上是一个64位的数字，高32位是epoch用来标识leader的周期。

如果有新的leader产生出来，epoch会自增，低32位用来递增计数。

当新产生proposal的时候，会依据数据库的两阶段过程，首先会向其他的server发出事务执行请求，如果超过半数的机器都能执行并且能够成功，那么就会开始执行。

# 4. Zookeeper 有哪几种部署模式？
部署模式：单机模式、伪集群模式、集群模式。

# 5. Zookeeper 集群最少要几台服务器，什么规则？
集群最少要3台服务器，集群规则为2N+1台，N>0，即3台。

# 6. Zookeeper 有哪些典型应用场景？
Zookeeper是一个典型的发布/订阅模式的分布式数据管理与协调框架，开发者可以使用它来进行分布式数据的发布和订阅。

通过对Zookeeper中丰富的数据节点进行交叉使用，配合Watcher事件通知机制，可以非常方便的构建一系列分布式应用中年都会涉及的核心功能。应用场景如下：

1）数据发布/订阅

2）负载均衡

3）命名服务

4）分布式协调/通知

5）集群管理

6）Master选举

7）分布式锁

8）分布式队列

# 7. Paxos 和 ZAB 算法有什么区别和联系？
**相同点**

两者都存在一个类似于Leader进程的角色，由其负责协调多个Follower进程的运行；

Leader进程都会等待超过半数的Follower做出正确的反馈后，才会将一个提案进行提交；

ZAB协议中每个Proposal中都包含一个epoch值来代表当前的Leader周期，而Paxos中名字为Ballot。

**不同点**

Paxos是用来构建分布式一致性状态机系统，而ZAB用来构建高可用的分布式数据主备系统（Zookeeper）。

# 8. Zookeeper 中 Java 客户端都有哪些？
Java客户端：

1）zk自带的zkclient

2）Apache开源的Curator

# 9. Zookeeper 集群支持动态添加服务器吗？
动态添加服务器等同于水平扩容，而Zookeeper在这方面的表现并不是太好。两种方式：

**1）全部重启**

关闭所有Zookeeper服务，修改配置之后启动。不影响之前客户端的会话。

**2）逐一重启**

在过半存活即可用的原则下，一台机器重启不影响整个集群对外提供服务。这是比较常用的方式，在Zookeeper 3.5版本开始支持动态扩容。

# 10. Zookeeper 和 Nginx 的负载均衡有什么区别？
Zookeeper的负载均衡是可以调控，而Nginx的负载均衡只是能调整权重，其他可控的都需要自己写Lua插件；但是Nginx的吞吐量比Zookeeper大很多，具体按业务应用场景选择用哪种方式。

# 11. Zookeeper 节点宕机如何处理？
Zookeeper本身是集群模式，官方推荐不少于3台服务器配置。Zookeeper自身保证当一个节点宕机时，其他节点会继续提供服务。

假设其中一个Follower宕机，还有2台服务器提供访问，因为Zookeeper上的数据是有多个副本的，并不会丢失数据；

如果是一个Leader宕机，Zookeeper会选举出新的Leader。

Zookeeper集群的机制是只要超过半数的节点正常，集群就可以正常提供服务。只有Zookeeper节点挂得太多，只剩不到一半节点提供服务，才会导致Zookeeper集群失效。

Zookeeper集群节点配置原则

3个节点的cluster可以挂掉1个节点（leader可以得到2节 > 1.5）。

2个节点的cluster需保证不能挂掉任何1个节点（leader可以得到 1节 <=1）。

# 12. Zookeeper 中 Server 都有哪些工作状态？
Zookeeper服务器具有四种状态，分别是LOOKING、FOLLOWING、LEADING、OBSERVING。

LOOKING：寻找Leader状态

当服务器处于该状态时，它会认为当前集群中没有Leader，因此需要进入Leader选举状态，也就是正在进行选举Leader的过程的server所处状态

LEADING：领导者状态

表示当前Zookeeper服务器角色是Leader，已被选举为Leader后的server所处状态。

FOLLOWING：跟随者状态

表示当前Zookeeper服务器角色是Follower，成为Follower后的server所处状态。

OBSERVING：观察者状态

表示当前Zookeeper服务器角色是Observer，在一个很大的集群中没有投票权的server所处状态。

# 13. Zookeeper 常用命令都有哪些？
1、ZooKeeper服务命令

1）启动ZK服务: bin/zkServer.sh start

2）查看ZK服务状态: bin/zkServer.sh status

3）停止ZK服务: bin/zkServer.sh stop

4）重启ZK服务: bin/zkServer.sh restart

5）连接服务器: zkCli.sh -server 127.0.0.1:2181

2、连接ZooKeeper

启动ZooKeeper服务后可以使用命令连接到ZooKeeper服务：
```shell
zkCli.sh -server 127.0.0.1:2181
```

3、ZooKeeper客户端命令

1）ls -- 查看某个目录包含的所有文件：
```shell
[zk: 127.0.0.1:2181(CONNECTED) 1] ls /
```
2）ls2 -- 查看某个目录包含的所有文件，与ls不同的是它查看到time、version等信息：
```shell
[zk: 127.0.0.1:2181(CONNECTED) 1] ls2 /
```
3）create -- 创建znode，并设置初始内容：
```shell
[zk: 127.0.0.1:2181(CONNECTED) 1] create /test "test"
Created /test
```
创建一个新的znode节点“test”以及与它关联的字符串

4）get -- 获取znode的数据：
```shell
[zk: 127.0.0.1:2181(CONNECTED) 1] get /test
```
5）set -- 修改znode内容：
```shell
[zk: 127.0.0.1:2181(CONNECTED) 1] set /test "ricky"
```
6）delete -- 删除znode：
```shell
[zk: 127.0.0.1:2181(CONNECTED) 1] delete /test
```
7）quit -- 退出客户端

8）help -- 帮助命令

# 14. ZooKeeper 客户端如何注册 Watcher 实现？
1）客户端调用getData()、getChildren()、exist()三个接口，传入Watcher对象。

2）标记请求request，封装Watcher到WatchRegistration。

3）封装成Packet对象，发送request至服务端。

4）收到服务端响应后，将Watcher注册到ZKWatcherManager中进行管理。

5）请求返回，完成注册。

# 15. ZooKeeper 服务端如何处理 Watcher 实现？
**服务端接收Watcher并存储**

接收到客户端的请求，判断是否注册Watcher，如果需要注册Watcher就将数据节点路径和ServerCnxn存储到WatcherManager中的WatchTable和watch2Paths。

其中ServerCnxn表示一个客户端和服务端的连接，实现了Watcher的process接口，此时可以看成一个Watcher对象。

**Watcher触发**

假设服务端接收到setData()方法事务请求时触发NodeDataChanged事件。

**封装WatchedEvent**

将通知状态（SyncConnected）、事件类型（NodeDataChanged）以及节点路径封装成一个WatchedEvent对象。

**查询Watcher**

从WatchTable中根据节点路径查找Watcher对象，若果没找到说明没有客户端在该数据节点上注册过Watcher对象，反之找到的话就提取并从WatchTable和Watch2Paths中删除对应Watcher。从这里可以看出Watcher在服务端是触发一次就失效。然后调用process()方法来触发Watcher，process()方法主要通过ServerCnxn对应的TCP连接发送Watcher事件通知。

# 16. ZooKeeper 客户端如何回调 Watcher？
ZooKeeper客户端SendThread线程接收事件通知，交由EventThread线程回调Watcher。

ZooKeeper客户端的Watcher机制是一次性的，一旦被触发后这个Watcher就失效了。

# 17. ZooKeeper 中 什么是 ACL 权限控制机制？
什么是ACL？

ACL全称Access Control List即访问控制列表，用于控制资源的访问权限。

ZooKeeper利用ACL策略控制节点的访问权限，如节点数据读写、节点创建、节点删除、读取子节点列表、设置节点权限等。

在传统文件系统中，ACL分为两个维度。一个是属组，一个是权限。属组包含多个权限，一个文件或目录拥有某个组的权限即拥有了组里的所有权限，文件或子目录默认会继承自父目录的ACL。

Zookeeper中znode的ACL是没有继承关系的，每个znode的权限都是独立控制的，只有客户端满足znode设置的权限要求时，才能完成相应的操作。

Zookeeper的ACL，分为三个维度：scheme、id、permission。

通常表示方式：

scheme:id:permission
schema代表授权策略，id代表授权对象，permission代表权限。

ACL（Access Control List）访问控制列表，包括三个方面：

**权限模式（Scheme）**

IP：从IP地址粒度进行权限控制。

Digest：最常用，用类似于username:password的权限标识来进行权限配置，便于区分不同应用来进行权限控制。

World：最开放的权限控制方式，是一种特殊的digest模式，只有一个权限标识“world:anyone”。

Super：超级用户。

**授权对象（Id）**

授权对象指权限赋予的用户或一个指定实体。

**权限（Permission）**

CREATE：数据节点创建权限，允许授权对象在该Znode下创建子节点。

DELETE：子节点删除权限，允许授权对象删除该数据节点的子节点。

READ：数据节点的读取权限，允许授权对象访问该数据节点并读取其数据内容或子节点列表等。

WRITE：数据节点更新权限，允许授权对象对该数据节点进行更新操作。

ADMIN：数据节点管理权限，允许授权对象对该数据节点进行ACL相关设置操作。

# 18. 分布式集群中为什么会有 Master？
Master选举原理是有多个Master，其每次只有一个Master负责主要的工作，剩余的Master作为备份并负责监听工作的Master，一旦负责工作的Master挂掉，其他的Master就会收到监听的事件，从而去抢夺负责工作的权利，其他没有争夺到负责主要工作的Master转而去监听负责工作的新Master。

在分布式环境中，一般采用master-salve模式（主从模式），正常情况下主机提供服务，备机负责监听主机状态，当主机异常时自动切换到备机继续提供服务，也就是只需要集群中的某一台服务器进行执行，其他的服务器共享这个结果，从而减少服务器的重复计算，提高性能。其切换过程中选出下一个Master的过程就是master选举，因此需要进行leader选举。

# 19. ZooKeeper 命名服务是什么？
命名服务是指通过指定的名字来获取资源或者服务的地址。

Zookeeper会在自己的文件系统上（树结构的文件系统）创建一个以路径为名称的节点，它可以指向提供的服务的地址、远程对象等。

简单来说使用Zookeeper做命名服务就是用路径作为名字，路径上的数据就是其名字指向的实体。

# 20. ZooKeeper 支持哪种类型的数据节点？
1）PERSISTENT-持久节点

除非手动删除，否则节点一直存在于Zookeeper上。

2）EPHEMERAL-临时节点

临时节点的生命周期与客户端会话绑定，一旦客户端会话失效（客户端与zookeeper连接断开不一定会话失效），那么这个客户端创建的所有临时节点都会被移除。

3）PERSISTENT_SEQUENTIAL-持久顺序节点

基本特性同持久节点，只是增加了顺序属性，节点名后边会追加一个由父节点维护的自增整型数字。

4）EPHEMERAL_SEQUENTIAL-临时顺序节点

基本特性同临时节点，增加了顺序属性，节点名后边会追加一个由父节点维护的自增整型数字。