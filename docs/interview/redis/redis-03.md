# 1. Redis 中如何解决 overcommit_memory is set to 0 告警问题？
TODO

# 2. 为什么 Redis 集群的最大槽数是 16384 个？
在redis节点发送心跳包时需要把所有的槽放到这个心跳包中，以便让节点知道当前集群信息，即16384=16k，在发送心跳包时使用char进行bitmap压缩后是2k（2*8 (8bit)*1024(1k)=16K），也就是说使用2k的空间创建了16k的槽数。

虽然使用CRC16算法最多可以分配65535（2^16-1）个槽位，即65535=65k，压缩后就是8k（8*8 (8 bit)*1024(1k)=65K），也就是说需要8k的心跳包，并且一般情况下一个redis集群不会有超过1000个master节点，所以16k的槽位是个比较合适的选择。

# 3. Redis 中如何实现分布式锁？
TODO

# 4. Redis 中分布式锁有什么缺陷性问题？
Redis分布式锁不能解决超时的问题，分布式锁有一个超时时间，程序的执行如果超出了锁的超时时间就会出现问题。

# 5. Redis 有哪几种集群模式？
目前很多项目中使用了Redis，多实例Redis服务比单一实例要复杂很多，这里面涉及到定位、容错、扩容等技术问题。常用sharding技术来对此进行管理，其集群模式主要有以下几种方式：

1、主从复制（Master-Slave Replication）

2、哨兵模式（Sentinel Replication）

3、Redis Cluster集群模式（服务端sharding）

4、Jedis sharding集群（客户端sharding）

5、利用中间件代理

# 6. 描述一下 Redis集群主从复制的工作原理？
TODO

# 7. Redis 集群主从复制有什么优缺点？
主从复制缺点

Redis不具备自动容错和恢复功能，主机从机的宕机都会导致前端部分读写请求失败，需要等待机器重启或者手动切换前端的IP才能恢复。

主机宕机，宕机前有部分数据未能及时同步到从机，切换IP后还会引入数据不一致的问题，降低了系统的可用性。

Redis的主从复制采用全量复制，复制过程中主机会fork出一个子进程对内存做一份快照，并将子进程的内存快照保存为文件发送给从机，这一过程需要确保主机有足够多的空余内存。若快照文件较大，对集群的服务能力会产生较大的影响，而且复制过程是在从机新加入集群或者从机和主机网络断开重连时都会进行，也就是网络波动都会造成主机和从机间的一次全量的数据复制，这对实际的系统运营造成了不小的麻烦。

Redis较难支持在线扩容，在集群容量达到上限时在线扩容会变得很复杂。为避免这一问题，在系统上线时必须确保有足够的空间，这对资源造成了很大的浪费。

其实redis的主从模式很简单，在实际的生产环境中是很少使用的，也不建议在实际的生产环境中使用主从模式来提供系统的高可用性，之所以不建议使用都是由它的缺点造成的，在数据量非常大的情况，或者对系统的高可用性要求很高的情况下，主从模式也是不稳定的。

# 8. 描述一下 Redis 集群 Sentinel（哨兵）进程的工作方式？
TODO

# 9. Redis 集群哨兵模式有什么优缺点？
哨兵模式优点

1）哨兵集群模式是基于主从模式的，所有主从的优点，哨兵模式同样具有。

2）主从可以切换，故障可以转移，系统可用性更好。

3）哨兵模式是主从模式的升级，系统更健壮，可用性更高。

哨兵模式缺点

1）Redis较难支持在线扩容，在集群容量达到上限时在线扩容会变得很复杂。为避免这一问题，在系统上线时必须确保有足够的空间，这对资源造成了很大的浪费。

2）哨兵模式其配置相对比较复杂。

# 10. Redis 集群 Sentinel（哨兵）进程有什么作用？
**1）监控(Monitoring): ** 哨兵(sentinel) 会不断地检查你的Master和Slave是否运作正常。

2）提醒(Notification)： 当被监控的某个Redis节点出现问题时, 哨兵(sentinel) 可以通过 API 向管理员或者其他应用程序发送通知。

3）自动故障迁移(Automatic failover)： 当一个Master不能正常工作时，哨兵(sentinel) 会开始一次自动故障迁移操作，它会将失效Master的其中一个Slave升级为新的Master, 并让失效Master的其他Slave改为复制新的Master；当客户端试图连接失效的Master时，集群也会向客户端返回新Master的地址，使得集群可以使用现在的Master替换失效Master。Master和Slave服务器切换后，Master的redis.conf、Slave的redis.conf和sentinel.conf的配置文件的内容都会发生相应的改变，即，Master主服务器的redis.conf配置文件中会多一行slaveof的配置，sentinel.conf的监控目标会随之调换。

# 11. 描述一下 Redis Cluster 集群模式的工作方式？
TODO

# 12. Redis 中 Jedis sharding 集群模式有哪些特点？
1）采用一致性哈希算法，将key和节点name同时hashing，然后进行映射匹配，采用的算法是MURMUR_HASH。采用一致性哈希而不是采用简单类似哈希求模映射的主要原因是当增加或减少节点时，不会产生由于重新匹配造成的rehashing。一致性哈希只影响相邻节点key分配，影响量小。

2）为了避免一致性哈希只影响相邻节点造成节点分配压力，ShardedJedis会对每个Redis节点根据名字(没有，Jedis会赋予缺省名字)会虚拟化出160个虚拟节点进行散列。根据权重weight，也可虚拟化出160倍数的虚拟节点。用虚拟节点做映射匹配，可以在增加或减少Redis节点时，key在各Redis节点移动再分配更均匀，而不是只有相邻节点受影响。

3）ShardedJedis支持keyTagPattern模式，即抽取key的一部分keyTag做sharding，这样通过合理命名key，可以将一组相关联的key放入同一个Redis节点，这在避免跨节点访问相关数据时很重要。

# 13. Redis 相比 Memcached 有哪些优势？
1、Memcached所有的值均是简单的字符串，redis作为其替代者，支持更为丰富的数据类。

2、Redis的速度比Memcached要快很多。

3、Redis可以持久化其数据。

# 14. 描述一下 Redis 单线程模型？
Redis是基于Reactor模式开发了自己的网络事件处理器，这个处理器被称为文件事件处理器。这个事件处理器是单线程的，所以 Redis 才叫做单线程的模型。它采用IO多路复用机制同时监听多个Socket，根据Socket上的事件来选择对应的事件处理器进行处理。

文件事件处理器的结构包含4个部分：

多个Socket

IO多路复用程序

文件事件分派器

事件处理器（连接应答处理器、命令请求处理器、命令回复处理器）

多个 ocket 能会并发产生不同的操作，每个操作对应不同的文件事件，但是IO路复用程序会监听多个 ocket，会将 ocket 生的事件放入队列中排队，文件事件分派器每次从队列中取出一个事件，把该事件交给对应的事件处理器进行处理。

Redis 6.0版本之后将连接应答处理器及命令回复处理器修改为了多线程，只有命令请求处理器仍然是单线程的。

# 15. Redis 中 IO 多路复用（Epoll）原理？
1、执行epoll_create函数会在内核的高速缓存区中建立一颗红黑树以及就绪链表（该链表存储已经就绪的文件描述符）。接着应用程序执行epoll_ctl函数添加文件描述符会在红黑树上增加相应的结点。

2、执行epoll_ctl的add操作时，不仅将文件描述符放到红黑树上，而且也注册了callBack函数。内核在检测到某文件描述符可读/可写时会调用回调函数，该回调函数将文件描述符放在就绪链表中。

3、执行epoll_wait函数只用观察就绪链表中有无数据即可，最后将链表的数据及就绪的数量返回给应用程序，应用程序只需要遍历依次处理即可。这里返回的文件描述符是通过内存映射函数mmap让内核和用户空间共享同一块内存实现传递的，减少了不必要的拷贝。

4、mmap：将用户空间的一段内存区域映射到内核空间，映射成功后，用户对这段内存区域的修改可以直接反映到内核空间。同样，内核空间对这段区域的修改也直接反映用户空间。那么对于内核空间、用户空间两者之间需要大量数据传输等操作的话效率是非常高的。

# 16. 为什么 Redis 的操作是原子性，怎么保证原子性？
对于Redis而言，命令的原子性指的是：一个操作的不可以再分，操作要么执行，要么不执行。

Redis的操作之所以是原子性的，是因为Redis是单线程的。

Redis本身提供的所有API都是原子操作，Redis中的事务其实是要保证批量操作的原子性。

# 17. Redis 的持久化机制是什么？各自的优缺点？
Redis提供两种持久化机制RDB和AOF机制：

1、RDB（Redis DataBase）持久化方式： 是指用数据集快照的方式（半持久化模式）记录redis数据库的所有键值对,在某个时间点将数据写入一个临时文件，持久化结束后，用这个临时文件替换上次持久化的文件，达到数据恢复。

优点：

1）只有一个文件dump.rdb，方便持久化。

2）容灾性好，一个文件可以保存到安全的磁盘。

3）性能最大化，fork子进程来完成写操作，让主进程继续处理命令，所以是IO最大化。（使用单独子进程来进行持久化，主进程不会进行任何IO操作，保证了redis的高性能） 4.相对于数据集大时，比AOF的启动效率更高。

缺点：

1）数据的完整性和一致性不高，因为RDB可能在最后一次备份时宕机了。

2）备份时占用内存，因为Redis 在备份时会独立创建一个子进程，将数据写入到一个临时文件（此时内存中的数据是原来的两倍哦），最后再将临时文件替换之前的备份文件。

2、AOF（Append-only file）持久化方式： 是指所有的命令行记录以redis命令请求协议的格式（完全持久化存储）保存为aof文件。

优点：

1）数据安全，aof持久化可以配置appendfsync属性，有always，每进行一次命令操作就记录到aof文件中一次。

2）通过append模式写文件，即使中途服务器宕机，可以通过redis-check-aof工具解决数据一致性问题。

3）AOF机制的rewrite模式。（AOF文件没被rewrite之前（文件过大时会对命令进行合并重写），可以删除其中的某些命令（比如误操作的flushall））

缺点：

1）AOF文件比RDB文件大，且恢复速度慢。

2）数据集大的时候，比rdb启动效率低。

# 18. 描述一下 Redis 内存淘汰机制（回收策略）？
Redis内存淘汰机制包括：

noeviction: 当内存不足以容纳新写入数据时，新写入操作会报错。（这个有点过于暴力, 不推荐）

allkeys-lru：当内存不足以容纳新写入数据时，在键空间中，移除最近最少使用的key（这个是最常用的）。

allkeys-random：当内存不足以容纳新写入数据时，在键空间中，随机移除某个key。

volatile-lru：当内存不足以容纳新写入数据时，在设置了过期时间的键空间中，移除最近最少使用的key（这个一般不太合适）。

volatile-random：当内存不足以容纳新写入数据时，在设置了过期时间的键空间中，随机移除某个key。

volatile-ttl：当内存不足以容纳新写入数据时，在设置了过期时间的键空间中，有更早过期时间的key优先移除。

Redis默认的过期策略是noeviction

noeviction策略是内存不足以容纳新写入数据时，新写入操作会报错。

```shell

127.0.0.1:6379> config get maxmemory-policy
1) "maxmemory-policy"
2) "noeviction"
   127.0.0.1:6379>
   redis.conf中过期淘汰策略配置如下（翻译）


# MAXMEMORY POLICY: how Redis will select what to remove when maxmemory
# is reached. You can select among five behaviors:
#最大内存策略：当到达最大使用内存时，可以在下面5种行为中选择，Redis如何选择淘汰数据库键

#当内存不足以容纳新写入数据时

# volatile-lru -> remove the key with an expire set using an LRU algorithm
# volatile-lru ：在设置过期时间的键空间中，移除最近最少使用的key。这种情况一般是把redis既当缓存，又做持久化存储的时候才用。

# allkeys-lru -> remove any key according to the LRU algorithm
# allkeys-lru ：移除最近最少使用的key（推荐）

# volatile-random -> remove a random key with an expire set
# volatile-random ：在设置了过期时间的键空间中，随机移除一个键，不推荐

# allkeys-random -> remove a random key, any key
# allkeys-random ：直接在键空间中随机移除一个键，弄啥叻

# volatile-ttl -> remove the key with the nearest expire time (minor TTL)
# volatile-ttl ：在设置了过期时间的键空间中，有更早过期时间的key优先移除 不推荐

# noeviction -> don't expire at all, just return an error on write operations
# noeviction ：不做过键处理，只返回一个写操作错误。不推荐

# Note: with any of the above policies, Redis will return an error on write
# operations, when there are no suitable keys for eviction.
# 上面所有的策略下，在没有合适的淘汰删除的键时，执行写操作时，Redis 会返回一个错误。下面是写入命令：
# At the date of writing these commands are: set setnx setex append
# incr decr rpush lpush rpushx lpushx linsert lset rpoplpush sadd
# sinter sinterstore sunion sunionstore sdiff sdiffstore zadd zincrby
# zunionstore zinterstore hset hsetnx hmset hincrby incrby decrby
# getset mset msetnx exec sort

# 过期策略默认：
# The default is:
# maxmemory-policy noeviction

```
# 19. Redis 集群最大节点个数是多少？
Redis集群最大节点个数是16384个，即哈希槽数是16384个。

Redis集群并没有使用一致性hash，而是引入了哈希槽的概念。

Redis集群有16384个哈希槽，每个key通过CRC16校验后对16384取模来决定放置哪个槽，集群的每个节点负责一部分hash槽。
