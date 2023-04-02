# 一、Redis键（key）
## 1. 常用命令

 1. **` keys *`**    查看当前库所有key

 	![在这里插入图片描述](https://img-blog.csdnimg.cn/251dcdf6f11e42e2861c340883ff1478.png)
 2. **`exists key`** 判断某个key是否存在（1代表有，0代表没有）

 	![在这里插入图片描述](https://img-blog.csdnimg.cn/60a456ef08954adfbad1ea4713e0d6de.png)
 3. **`type key`** 查看你的key是什么类型

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/6391f3ad93b2441983910e1a0cae91bf.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASmllTmlM,size_15,color_FFFFFF,t_70,g_se,x_16)
 4. **`del key`**       删除指定的key数据

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/d98b31f06afe4e3baf906a6489d2f8c0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASmllTmlM,size_15,color_FFFFFF,t_70,g_se,x_16)
 5. **`unlink key`**   根据value选择非阻塞删除（仅将keys从keyspace元数据中删除，真正的删除会在后续异步操作）

	![在这里插入图片描述](https://img-blog.csdnimg.cn/b548730f851f45fca6b6bce0e6d077f1.png)
 6. **`expire key 10`**   10秒钟：为给定的key设置过期时间
 7. **`ttl key`** 查看还有多少秒过期，-1表示永不过期，-2表示已过期

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/c0a31fec5aef45878ff2930e239ff69f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASmllTmlM,size_18,color_FFFFFF,t_70,g_se,x_16)
 8. **`select index`** 命令切换数据库,下标从0开始，共有16个数据库

	![在这里插入图片描述](https://img-blog.csdnimg.cn/d3736d10e31f4a6284f22e678ebc14a0.png)
 9. **`dbsize`** 查看当前数据库的key的数量

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/75aa0038c4f14fad8bf080ff0f31d24e.png)
 10. **`flushdb`** 清空当前库

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/026274f8d6d64f76b5a10529bfcbbc6c.png)
 11. **`flushall`** 通杀全部库


# 二、Redis 字符串（String）
## 1. 概述
>  - String 是 Redis 最基本的类型，可以理解成与 Memcached 一模一样的类型，一个 key 对应一个 value。
>    
>  - String 类型是二进制安全的。意味着 Redis 的 string 可以包含任何数据。比如 jpg 图片或者序列化的对象。
> 
>  - String 类型是 Redis 最基本的数据类型，一个 Redis 中字符串 value 最多可以是 512M。

## 2. 常用命令
 1. **`set <key> <value>`** 添加键值对

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/1f1f69cc41164209849f59453669ca5b.png)
 2. **`get <key>`** 查询对应键值

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/8fafa4a47f764d82a2f20b09760ddb63.png)

 3. **`append  <key> <value>`** 将给定的 `<value>` 追加到原值的末尾

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/dae03cbe47b14757a9b6be11e11a9c52.png)

 4. **`strlen  <key>`** 获得值的长度

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/6e1d6ca052af49b0b8464378686c1e70.png)
 5. **`setnx  <key> <value>`** 只有在 key 不存在时，设置 key 的值

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/9c0a16b87ee145a19cd97dc3b206a7f9.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASmllTmlM,size_18,color_FFFFFF,t_70,g_se,x_16)
 6. **`incr  <key>`** 将 key 中储存的数字值增1，只能对**数字值**操作，如果为空，新增值为1

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/32cd7cba20554209b5d1bfb11cfb6e6e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASmllTmlM,size_19,color_FFFFFF,t_70,g_se,x_16)
 7. **`decr  <key>`** 将 key 中储存的数字值减1，只能对**数字值**操作，如果为空，新增值为-1

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/068d76b90c634e10948a0ac8ac75f3f7.png)
 8. **`incrby / decrby  <key> <步长>`** 将 key 中储存的数字值增减。自定义步长

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/fdfd47c6c35940caa26a9902467779c4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASmllTmlM,size_14,color_FFFFFF,t_70,g_se,x_16)

 9. **`mset  <key1> <value1> <key2> <value2>  .....`** 同时设置一个或多个 key-value对  

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/e0e0f3ba10eb48acbe97e831c446a59c.png)

 10. **`mget  <key1> <key2> <key3> .....`** 同时获取一个或多个 value  

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/8b34bf2affd84334a4962e2c62429d60.png)

 11. **`msetnx <key1> <value1> <key2> <value2>  ..... `** 同时设置一个或多个 key-value 对，当且仅当所有给定 key都不存在。**原子性，有一个失败则都失败** （1表示成功，0表示失败）

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/ccdb2e3bbc2d4a4ebc0848106e46fa84.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASmllTmlM,size_19,color_FFFFFF,t_70,g_se,x_16)

 12. **`getrange  <key> <起始位置> <结束位置>`** 获得值的范围，类似java中的substring，**前包，后包**（0 -1表示取全部）

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/9d66bf336562482697cdc8c1b5b4ed9d.png)

 13. **`setrange  <key> <起始位置> <value>`** 用 `<value>` 覆写 `<key>` 所储存的字符串值，从<起始位置>开始(**索引从0开始**)。

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/ac70256828dc426ab0172e6c7fc550a9.png)

 14. **`setex  <key> <过期时间> <value>`** 设置键值的同时，设置过期时间，单位秒。

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/3a807fe94d5e40bb9a3065d1ed151a93.png)
 15. **`getset <key> <value>`** 以新换旧，设置了新值同时获得旧值

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/8bb45196ac8141a58f015198130bc5b3.png)

# 三、Redis列表（List）
## 1. 概述
>  - Redis 列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）。
>    
>  - 它的底层实际是个双向链表，对两端的操作性能很高，通过索引下标的操作中间的节点性能会较差。
>   
>  -  一个列表最多可以包含 232 - 1 个元素 (4294967295, 每个列表超过40亿个元素)。
## 2. 常用命令
 1. **`lpush/rpush  <key> <value1> <value2> <value3> ....`** 从左边/右边插入一个或多个值。

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/ae71adad2e5542d5939a4c8b5c1405cf.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASmllTmlM,size_20,color_FFFFFF,t_70,g_se,x_16)
 2. **`lpop/rpop  <key>`** 从左边/右边吐出一个值。值在键在，值光键亡。

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/ccd91593f6a743f3814d6e3f9170e056.png)
 3. **`rpoplpush  <key1> <key2>`** 从 `<key1>` 列表右边吐出一个值，插到<key2>列表左边。

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/54eabf6316cf47d194a64524176a73c2.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASmllTmlM,size_20,color_FFFFFF,t_70,g_se,x_16)
 4. **`lrange <key> <start> <stop>`** 按照索引下标获得元素(从左到右) 0左边第一个，-1右边第一个，（0-1表示获取所有）

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/d5853829f583423eb046796346aceb90.png)
 5. **`lindex <key> <index>`** 按照索引下标获得元素(从左到右)

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/d9c11c76e7e34d77b3373e82b6f03ec0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASmllTmlM,size_16,color_FFFFFF,t_70,g_se,x_16)
 6. **`llen <key>`** 获得列表长度 

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/f8f222d7dd704529a78c7098f487e678.png)
 7. **`linsert <key>  before/after <value> <newvalue> `** 在 `<value>` 的后面插入<newvalue>插入值

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/b8626c2540d548df87e5cb17b18a2dbd.png)
 8. **`lrem <key> <n> <value>`** 从左边删除n个value(从左到右)

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/4a2c31d12ad74196b4c997c78b9fa21c.png)
 9. **`lset <key> <index> <value>`** 将列表key下标为index的值替换成value 

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/bfd4a12648cb4fd0944f1a442f229da8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASmllTmlM,size_16,color_FFFFFF,t_70,g_se,x_16)

# 四、Redis集合（Set）
## 1. 概述

    > - Redis 的 Set 是 String 类型的**无序集合**。集合成员是唯一的，这就意味着集合中不能出现重复的数据。
    > 
    > - 集合对象的编码可以是 intset 或者 hashtable。
    > 
    > - Redis 中集合是通过哈希表实现的，所以添加，删除，**查找的复杂度都是 O(1)**。
    > 
    > - 集合中最大的成员数为 232 - 1 (4294967295, 每个集合可存储40多亿个成员)。
## 2. 常用命令

 1. **`sadd <key> <value1> <value2> ..... `** 将一个或多个 member 元素加入到集合 key 中，已经存在的 member 元素将被忽略

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/8e4184dfb95048398f68efe633df1aa1.png)
 2. **`smembers <key>`** 取出该集合的所有值。

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/e9b4213030c64b3f9c2f6a6ca4029fb0.png)
 3. **`sismember <key> <value>`** 判断集合 `<key>` 是否为含有该 `<value>` 值，有1，没有0

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/a081643db7e74b719ec80fcdda0f5637.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASmllTmlM,size_19,color_FFFFFF,t_70,g_se,x_16)
 4. **`scard <key>`** 返回该集合的元素个数。

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/8948e8828ad4409ab08879c4fcacb9ed.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASmllTmlM,size_19,color_FFFFFF,t_70,g_se,x_16)
 5. **`srem <key> <value1> <value2> .... `** 删除集合中的某个元素。

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/81d74cada147491fa63d4645d2b2f528.png)
 6. **`spop <key>`** 随机从该集合中吐出一个值，**会从集合中删除**

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/57d1f8ebfc9a4b5e97fac92ba7b7ca5d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASmllTmlM,size_16,color_FFFFFF,t_70,g_se,x_16)
 7. **`srandmember <key> <n>`** 随机从该集合中取出n个值。**不会从集合中删除** 。

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/a95dec8b1dd6478aa9a3d720bc9c6097.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASmllTmlM,size_18,color_FFFFFF,t_70,g_se,x_16)
 8. **`smove <source> <destination>`** value把集合中一个值从一个集合移动到另一个集合

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/ba0205eebdde441d8c15fbf6d13b99ed.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASmllTmlM,size_17,color_FFFFFF,t_70,g_se,x_16)
 9. **`sinter <key1> <key2>`** 返回两个集合的交集元素。

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/6d6996b86ee74ca7945f9b6734a56060.png)
 10. **`sunion <key1> <key2>`** 返回两个集合的并集元素。

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/a7c27cbae21343f0bb96ce51cda2523a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASmllTmlM,size_18,color_FFFFFF,t_70,g_se,x_16)
 11. **`sdiff <key1> <key2>`** 返回两个集合的差集元素(**key1中的，不包含key2中的**)

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/67859b115e27491fb48b0bc67c14333a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASmllTmlM,size_17,color_FFFFFF,t_70,g_se,x_16)

# 五、Redis哈希（Hash）
## 1. 概述

> - Redis hash 是一个键值对集合。 
> - Redis hash 是一个 string 类型的 field（字段） 和 value（值）的映射表，hash 特别适合用于存储对象。
> - 类似Java里面的Map<String,Object> Redis 中每个 hash 可以存储232 - 1 键值对（40多亿）。

## 2. 常用命令

 1. **`hset <key> <field> <value>`** 给`<key>` 集合中的  `<field>` 键赋值 `<value>`

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/cc8c539568e644a085a30976d53f7101.png)
 2. **`hget <key1> <field>`** 从 `<key1>` 集合 `<field>` 取出 value 

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/9b772a675a6a46968d5830b2145c1efb.png)
 3. **`hmset <key1> <field1> <value1> <field2> <value2>...`** 批量设置hash的值

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/a91c5ebec99c4af6ac579dd9065d2b5f.png)
 4. **`hexists <key1> <field>`** 查看哈希表 key 中，给定域 field 是否存在。 

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/74c37d2e56794e28a36aaa62cbb5a757.png)
 5. **`hkeys <key>`** 列出该hash集合的所有field

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/ceb3bef3a7584d09b849b4ebb9f1b24c.png)
 6. **`hvals <key>`** 列出该hash集合的所有value

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/ed9c648e0ec741eab261ab910664fe1d.png)
 7. **`hincrby <key> <field> <increment>`** 为哈希表 key 中的域 field 的值加上增量 1   -1

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/46cdf43cd730452498d6f1b2222545cd.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASmllTmlM,size_18,color_FFFFFF,t_70,g_se,x_16)
 8. **`hsetnx <key> <field> <value>`** 将哈希表 key 中的域 field 的值设置为 value ，当且仅当域 field 不存在 

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/4636c528a49541448777d840b7f7c84c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASmllTmlM,size_20,color_FFFFFF,t_70,g_se,x_16)


# 六、Redis有序集合（sorted set）
## 1. 概述

> - Redis有序集合zset与普通集合set非常相似，是一个**没有重复元素**的字符串集合。
> 
> - 不同之处是有序集合的每个成员都关联了一个**评分（score）**,这个评分（score）被用来按照从最低分到最高分的方式排序集合中的成员。**集合的成员是唯一的，但是评分可以是重复了。**
> 
>  - 因为元素是有序的, 所以你也可以很快的根据评分（score）或者次序（position）来获取一个范围的元素。
>  
> - 访问有序集合的中间元素也是非常快的,因此你能够使用有序集合作为一个没有重复成员的智能列表。

## 2. 常用命令
 1. **`zadd  <key> <score1> <value1> <score2> <value2>... `** 将一个或多个 member 元素及其 score 值加入到有序集 key 当中。
 
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/1392f45f7bc44be6a5e010dd3f8d63b2.png)
 2. **`zrange <key> <start> <stop>  [WITHSCORES]  `** 返回有序集 key 中，下标在<start><stop>之间的元素，带WITHSCORES，可以让分数一起和值返回到结果集。

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/979fc05f99d1443b9d961841ef9fd519.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASmllTmlM,size_20,color_FFFFFF,t_70,g_se,x_16)
 3. **`zrangebyscore key minmax [withscores] [limit offset count]`** 返回有序集 key 中，所有 score 值介于 min 和 max 之间(包括等于 min 或 max )的成员。有序集成员按 score 值递增(从小到大)次序排列。 

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/f7bf061782a64f7e8c1a7d4a1fbd9f17.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASmllTmlM,size_20,color_FFFFFF,t_70,g_se,x_16)
 4. **`zrevrangebyscore key maxmin [withscores] [limit offset count] `** 返回有序集 key 中，所有 score 值介于 min 和 max 之间(包括等于 min 或 max )的成员。有序集成员按 score 值递增(从大到小)次序排列。

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/2f116786d8ed4ca389d38b14d1e40be6.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASmllTmlM,size_20,color_FFFFFF,t_70,g_se,x_16)
 5. **`zincrby <key> <increment> <value> `** 为元素的score加上增量

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/f4124c411d914e98865bf77690b39c23.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASmllTmlM,size_20,color_FFFFFF,t_70,g_se,x_16)
 6. **`zrem  <key> <value> `** 删除该集合下，指定值的元素，下标从1开始

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/a5a948811bee4987affb0a43227d6cd4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASmllTmlM,size_17,color_FFFFFF,t_70,g_se,x_16)
 7. **`zcount <key> <min> <max>`** 统计该集合，分数区间内的元素个数 

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/f1afcd9b32624061abc8bb2b62b5df05.png)
 8. **`zrank <key> <value>`** 返回该值在集合中的排名，从0开始。

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/f668cb9d2ecd4abd99e70b0f84eb8c37.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASmllTmlM,size_20,color_FFFFFF,t_70,g_se,x_16)

>***努力改变自己***