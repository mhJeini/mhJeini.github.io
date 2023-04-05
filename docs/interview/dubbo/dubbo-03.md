# 1. Dubbo 中服务提供者正常但注册中心不可见如何处理？
1、确认服务提供者是否连接了正确的注册中心，不只是检查配置中的注册中心地址，而且要检查实际的网络连接。

2、查看服务提供者是否非常繁忙，比如压力测试，以至于没有CPU片段向注册中心发送心跳，这种情况减小压力将自动恢复。

# 2. Dubbo 适用于哪些场景？
1、RPC分布式服务，拆分应用进行服务化，提高开发效率，调优性能，节省竞争资源。

2、配置管理，解决服务的地址信息剧增，配置困难的问题。

3、服务依赖，解决服务间依赖关系错踪复杂的问题。

4、服务扩容，解决随着访问量的不断增大，动态扩展服务提供方的机器的问题。

# 3. Dubbo 调用超时问题，如何处理？
dubbo调用服务超时，默认是会重试两次的，但可能两次请求都是成功的。如果没有幂等性处理，就会产生重复数据。比如在发邮件时，可能就会发出多份重复邮件，执行注册请求时，就会插入多条重复的注册数据。

超时解决方法

1、针对核心的服务中心，可以考虑去除dubbo超时重试机制，重新评估设置超时时间。

2、dubbo重试在集群环境下，会把超时的请求发到其他服务。

3、引起超时的原因可能出在消费端，也可能出现在服务端，服务器的网络、内存、CPU、存储空间都可能引起超时问题。

4、超时时间设置过小也会导致超时问题。

全局配置

```xml
<!-- 延迟到Spring初始化完成后，再暴露服务，服务调用超时设置为6秒，超时不重试-->
<dubbo:provider delay="-1" timeout="6000" retries="0"/>
```
注意的是dubbo重试机制是非常不错的QOS保证，它会帮助超时的请求路由转发到其他服务器上，而不是本机尝试，因此dubbo重试机制也在一定程度上保证服务的质量。

# 4. Dubbo 中如何保证服务安全调用？
1、Dubbo和Zookeeper部署在内网，不对外网开放。

2、Zookeeper的注册可以添加用户权限认证。

3、Dubbo通过Token令牌防止用户绕过注册中心直连。

4、在注册中心上管理授权。

5、增加对接口参数校验。

6、提供IP、服务黑白名单，来控制服务所允许的调用方。

# 5. Dubbo 超时设置的优先级是什么？
dubbo支持非常细粒度的超时设置包括：方法级别、接口级别和全局。

如果各个级别同时配置，优先级为：消费端方法级 > 服务端方法级 > 消费端接口级 > 服务端接口级 > 消费端全局 > 服务端全局。

# 6. Dubbo 超时的实现原理是什么？
dubbo默认采用了netty做为网络组件，它属于一种NIO的模式。消费端发起远程请求后，线程不会阻塞等待服务端的返回，而是马上得到一个ResponseFuture，消费端通过不断的轮询机制判断结果是否有返回。

轮询需要特别注要的就是避免死循环，因此为了解决这个问题引入了超时机制，只在一定时间范围内做轮询，如果超时时间就返回超时异常。

# 7. 为什么 Dubbo 不用 JDK SPI，而是要自己实现？
Java SPI在查找扩展实现类的时候遍历SPI的配置文件并且将实现类全部实例化。

假设一个实现类初始化过程比较消耗资源且耗时，但是代码中又用不上它，这就造成了资源的浪费。

因此Dubbo就自己实现了一个SPI，给每个实现类配了个名字，通过名字去文件里面找到对应的实现类全限定名然后加载实例化，按需加载。

# 8. 【腾讯】Dubbo 有几种负载均衡？负载均衡是在服务端还是客户端？
TODO

# 9. Dubbo 中都有哪些核心的配置？
| 配置	                | 配置说明   |
|--------------------|--------|
| dubbo:service	     | 服务配置   |
| dubbo:method	      | 方法配置   |
| dubbo:protocol	    | 协议配置   |
| dubbo:provider	    | 提供方配置  |
| dubbo:consumer	    | 消费方配置  |
| dubbo:application	 | 应用名称   |
| dubbo:reference	   | 引用配置   |
| dubbo:registry	    | 注册中心配置 |
| dubbo:argument	    | 参数配置   |
| dubbo:mudule	      | 模块配置   |
| dubbo:monitor	     | 监控中心配置 |
个人建议最少能说出一下文件中常用的几个配置。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.3.xsd http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">  
    <!--服务名称-->
    <dubbo:application name="hello-world-app"  />  
    <!--注册中心地址配置-->
    <dubbo:registry address="multicast://127.0.0.1:6688" />  
    <!--协议配置-->
    <dubbo:protocol name="dubbo" port="20880" />  
    <!--服务配置-->
    <dubbo:service interface="com.alibaba.dubbo.demo.DemoService" ref="demoServiceLocal" />  
    <!--引用配置-->
    <dubbo:reference id="demoServiceRemote" interface="com.alibaba.dubbo.demo.DemoService" />  
</beans>
```
# 10. Dubbo 中如何解决服务调用链过长的问题？
Dubbo中可以使用Pinpoint和Apache Skywalking实现分布式服务追踪等方式，解决解决服务调用链过长的问题。

# 11. Dubbo 注册中心挂掉，Consumer 和 Provider之间还能通讯吗？
Dubbo注册中心挂掉，Consumer和Provider之间是可以通讯的。

注册中心集群，发生宕机会自动切换；

启动Dubbo时，Consumer会从zookeeper拉取Provider注册的地址、接口等数据，缓存在本地；

Consumer每次调用时，按照本地存储的Provider地址进行调用；

Provider全部宕机，Consumer会无法使用，并无限次重连等待Provider恢复；

无法增加和调用新服务。

# 12. Dubbo 服务接口多种实现，如何注册调用？
使用Dubbo分组，通过不同的实现用不同的组。

服务提供者

```xml
<dubbo:service interface="…" ref="…" group="实现1" />
<dubbo:service interface="…" ref="…" group="实现2" />
```
服务消费者

```xml
<dubbo:reference id="…" interface="…" group="实现1" />
<dubbo:reference id="…" interface="…" group="实现2" />
```
