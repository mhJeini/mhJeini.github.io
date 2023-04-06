# 1. ZooKeeper 中是否支持禁止某一 IP 访问？
ZK本身不提供这样的功能，它仅仅提供了对单个IP的连接数的限制。

但是可以通过修改iptables等方式来实现对单个ip的限制。

# 2. Zookeeper 中都有哪些默认端口？
Zookeeper的默认端口有3个，如下：

2181：对Client端提供服务的端口。

3888：选举Leader。

2888：集群内的机器通讯使用。（Leader使用此端口）

# 3. Zookeeper 节点存储数据有没有限制？
Zookeeper中每个结点默认的数据量上限是1M，如果需要存入大于1M的数据量，则要修改jute.maxbuffer参数。

jute.maxbuffer：默认值1048575，单位字节，用于配置单个数据节点（ZNode）上可以存储的最大数据大小。

需要注意的是在修改该参数的时候，需要在zookeeper集群的所有服务端以及客户端上设置才能生效。

# 4. Zookeeper 中如何识别请求的先后顺序？
ZooKeeper会给每个更新请求，分配一个全局唯一的递增编号（zxid)，编号的大小体现事务操作的先后顺序。

# 5. Zookeeper 中 Stat 记录有哪些版本相关数据？
version：当前ZNode版本

cversion：当前ZNode子节点版本

aversion：当前ZNode的ACL版本

# 6. Zookeeper 中定义了几种操作权限？
zookeeper中对znode节点的操作权限主要有以下五种，我们可以通过其简写的任意组合来实现对znode节点的不同权限控制。

| 名称	| 简写	| 权限说明|
| -------------	| -------------	| -------------|
| CREATE	| c	| 允许创建当前节点下的字节点|
| DELETE	| d	| 允许删除当前节点下的子节点，仅限下一级|
| READ	| r	| 允许读取节点数据以及显示子节点的列表|
| WRITE	| w	| 允许设置当前节点的数据|
| ADMIN	| a	| 管理员权限，允许设置或读取当前节点的权限列表|
# 7. Zookeeper 中什么情况下导致 ZAB 进入恢复模式并选取新的 Leader?
启动过程或Leader出现网络中断、崩溃退出与重启等异常情况时。

当选举出新的Leader后，同时集群中已有过半的机器与该Leader服务器完成了状态同步之后,ZAB就会退出恢复模式。