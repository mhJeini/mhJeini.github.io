# 1. 什么是 Dubbo 框架？
Dubbo（读音[ˈdʌbəʊ]）是阿里巴巴公司开源的一个高性能优秀的服务框架，使得应用可通过高性能的 RPC 实现服务的输出和输入功能，可以和Spring框架无缝集成。

Dubbo提供了六大核心能力：面向接口代理的高性能RPC调用，智能容错和负载均衡，服务自动注册和发现，高度可扩展能力，运行期流量调度，可视化的服务治理与运维。

Dubbo是一款高性能、轻量级的开源Java RPC框架，它提供了三大核心能力：面向接口的远程方法调用，智能容错和负载均衡，以及服务自动注册和发现。

核心组件

Remoting: 网络通信框架，实现了 sync-over-async 和request-response 消息机制；

RPC: 一个远程过程调用的抽象，支持负载均衡、容灾和集群功能；

Registry: 服务目录框架用于服务的注册和服务事件发布和订阅。

# 2. Dubbo 支持哪些协议，推荐用哪种？
dubbo协议（推荐使用） 单一TCP长连接和NIO异步通讯，适合大并发小数据量的服务调用，以及服务消费者远大于提供者的情况。

缺点是Hessian二进制序列化，不适合传送大数据包的服务

rmi协议

采用JDK标准的rmi协议实现，传输参数和返回参数对象需要实现Serializable接口。

使用java标准序列化机制，使用阻塞式短连接，传输数据包不限，消费者和提供者个数相当。

多个短连接，TCP协议传输，同步传输，适用常规的远程服务调用和rmi互操作。

缺点是在依赖低版本的Common-Collections包，java反序列化存在安全漏洞，需升级commons-collections3 到3.2.2版本或commons-collections4到4.1版本。

webservice协议

基于WebService的远程调用协议(Apache CXF的frontend-simple和transports-http)实现，提供和原生WebService的互操作。

多个短连接，基于HTTP传输，同步传输，适用系统集成和跨语言调用。

http协议

基于Http表单提交的远程调用协议，使用Spring的HttpInvoke实现，对传输数据包不限，传入参数大小混合，提供者个数多于消费者。

缺点是不支持传文件，只适用于同时给应用程序和浏览器JS调用。

hessian协议

集成Hessian服务，基于底层Http通讯，采用Servlet暴露服务，Dubbo内嵌Jetty作为服务器实现,可与Hession服务互操作。

通讯效率高于WebService和Java自带的序列化。

适用于传输大数据包(可传文件)，提供者比消费者个数多，提供者压力较大。

缺点是参数及返回值需实现Serializable接口，自定义实现List、Map、Number、Date、Calendar等接口

thrift协议

协议：对thrift原生协议的扩展添加了额外的头信息，使用较少，不支持传null值。

memcache协议

基于memcached实现的RPC协议。

redis协议

基于redis实现的RPC协议。

# 3. Dubbo 默认使用什么注册中心，还有别的选择吗？
推荐使用Zookeeper作为注册中心，还有Redis、Multicast、Simple注册中心，但不推荐。

# 4. Dubbo 停止更新了吗？
Dubbo是阿里巴巴内部使用的分布式业务框架，于2012年由阿里巴巴开源。

由于Dubbo在阿里巴巴内部经过广泛的业务验证，在很短时间内，Dubbo就被许多互联网公司所采用，并产生了许多衍生版本，如网易，京东，新浪，当当等等。

由于阿里巴巴内部策略的调整变化，在2014年10月Dubbo停止维护。随后部分互联网公司公开了自行维护的Dubbo版本，比较著名的如当当DubboX，新浪Motan等。

在2017年9月，阿里宣布重启Dubbo项目，并决策在未来对开源进行长期持续的投入。随后Dubbo开始了密集的更新，并将搁置三年以来大量分支上的特性及缺陷快速修正整合。

在2018年2月，阿里巴巴将Dubbo捐献给Apache基金会，Dubbo成为Apache孵化器项目。

# 5. Dubbo 有哪几种集群容错方案，默认是哪种？
Dubbo有六种集群容错方案，默认是Failover Cluster。

| 集群容错方案	| 说明                             |
| ---------------	|--------------------------------|
| Failover Cluster	| 失败自动切换，自动重试其它服务器（默认）           |
| Failfast Cluster	| 快速失败，立即报错，只发起一次调用              |
| Failsafe Cluster	| 失败安全，出现异常时，直接忽略                |
| Failback Cluster	| 失败自动恢复，记录失败请求，定时重发             |
| Forking Cluster	| 并行调用多个服务器，只要一个成功即返回            |
| Broadcast Cluster	| 广播逐个调用所有提供者，任意一个报错则报错          |
# 6. Dubbo内置服务容器都有哪些？
Spring Container

自动加载META-INF/spring目录下的所有Spring配置。这个在源码中已经“写死”：

```properties
DEFAULT_SPRING_CONFIG = "classpath*:META-INF/spring/*.xml"
```

因此服务端主配置需放到META-INF/spring/目录下。

Jetty Container

启动一个内嵌Jetty，用于汇报状态，配置在java命令-D参数或dubbo.properties中

```properties
dubbo.jetty.port=8080 ----配置jetty启动端口
dubbo.jetty.directory=/foo/bar ----配置可通过jetty直接访问的目录，用于存放静态文件
dubbo.jetty.page=log,status,system ----配置显示的页面，缺省加载所有页面
```
Log4j Container

自动配置log4j的配置，在多进程启动时，自动给日志文件按进程分目录。 配置在java命令-D参数或者dubbo.properties中

```properties
dubbo.log4j.file=/foo/bar.log ----配置日志文件路径
dubbo.log4j.level=WARN ----配置日志级别
dubbo.log4j.subdirectory=20880 ----配置日志子目录，用于多进程启动，避免冲突
```
# 7. Dubbo 默认使用什么通信框架，还有别的选择吗？
Dubbo默认使用Netty框架，也是推荐的选择。

Dubbo还集成有Mina、Grizzly。

# 8. Dubbo 支持服务多协议吗？
Dubbo允许配置多协议，在不同服务上支持不同协议或同一服务上同时支持多种协议。

# 9. Dubbo 服务之间调用是阻塞的吗？
Dubbo默认是同步等待结果阻塞的，支持异步调用。

Dubbo是基于NIO的非阻塞实现并行调用，客户端不需要启动多线程即可完成并行调用多个远程服务，相对多线程开销较小，异步调用会返回一个Future对象。

# 10. Dubbo 支持服务降级吗？
Dubbo 2.2.0以上版本支持服务降级。

通过dubbo:reference中设置mock="return null"实现服务降级。

mock的值可以修改为true，再跟接口同一个路径下实现一个Mock类，命名规则是 “接口名称+Mock” 后缀。最后在Mock类里实现降级逻辑。

# 11. Dubbo 如何优雅停机？
Dubbo是通过JDK的ShutdownHook来完成优雅停机的，所以如果使用kill -9 PID等强制关闭指令，是不会执行优雅停机的，只有通过kill PID时，才会执行。

# 12. Dubbo 和 Spring Cloud 有哪些区别？
| /	| Dubbo	| Spring Cloud|
| ------------	| ------------	| ------------|
| 服务注册中心	| Zookeeper	Spring | Cloud Netfix Eureka|
| 服务调用方式	| RPC	| REST API|
| 服务监控	| Dubbo-monitor| 	Spring Boot Admin|
| 熔断器	| 不完善| 	Spring Cloud Netflix Hystrix|
| 服务网关	| 无	| Spring Cloud Netflix Zuul|
| 分布式配置	| 无	| Spring Cloud Config|
| 服务跟踪	| 无	| Spring Cloud Sleuth|
| 数据流	| 无	| Spring Cloud Stream|
| 批量任务	| 无	| Spring Cloud Task|
| 信息总线	| 无	| Spring Cloud Bus|
**最大区别**

Dubbo底层是使用Netty这样的NIO框架，是基于TCP协议传输的，配合以Hession序列化完成RPC通信。

SpringCloud是基于Http协议+rest接口调用远程过程的通信，相对来说，Http请求会有更大的报文，占的带宽也会更多。

但是REST相比RPC更为灵活，服务提供方和调用方的依赖只依靠一纸契约，不存在代码级别的强依赖，这在强调快速演化的微服务环境下，显得更为合适，至于注重通信速度还是方便灵活性，具体情况具体考虑。

**背景区别**

Dubbo是来源于阿里团队，Spring Cloud是来源于Spring团队，Spring广泛遍布全球各种企业开发中，可以确保SpringCloud的后续更新维护，Dubbo虽然来自国内顶尖的阿里团队,但是曾经被阿里弃用停更，但是后来阿里又低调重启维护。

**定位区别**

Dubbo是SOA时代的产物，它的关注点主要在于服务的调用，流量分发、流量监控和熔断。

Spring Cloud诞生于微服务架构时代，考虑的是微服务治理的方方面面，另外由于依托了Spirng、Spirng Boot 的优势之上，两个框架在开始目标就不一致，Dubbo定位服务治理、Spirng Cloud是一个生态。

因此可以大胆地判断，Dubbo未来会在服务治理方面更为出色，而Spring Cloud在微服务治理上面无人能敌。

**模块区别**

1、Dubbo主要分为服务注册中心，服务提供者，服务消费者，还有管控中心；

2、相比起Dubbo简单的四个模块，SpringCloud则是一个完整的分布式一站式框架，它有着一样的服务注册中心，服务提供者，服务消费者，管控台，断路器，分布式配置服务，消息总线，以及服务追踪等。

**性能区别**

Dubbo每次测试除去网络波动之外，都表现非常稳定。

Spring Cloud在第一次最慢，之后越来越快，连续测试4次以上单次测试性能超过Dubbo。

Spring Cloud-zuul在第一次最慢，之后也表现越来越快，连续4次以上测试，单次性能与dubbo相近，相差不超过0.02ms。

# 13. 说说 Dubbo Monitor 实现原理？
Consumer端在发起调用之前会先进入filter链；provider端在接收到请求时也是先进入filter链，然后才进行真正的业务逻辑处理。默认情况下，在consumer和provider的filter链中都会有Monitorfilter。

1、MonitorFilter向DubboMonitor发送数据

2、DubboMonitor将数据进行聚合后（默认聚合1分钟的统计数据）暂存到


ConcurrentMap<Statistics, AtomicReference> statisticsMap
然后使用一个含有3个线程（线程名字：DubboMonitorSendTimer）的线程池每隔1分钟，调用SimpleMonitorService遍历发送statisticsMap中的统计数据，每发送完毕一个，就重置当前的Statistics的AtomicReference。

3、SimpleMonitorService将这些聚合数据塞入 BlockingQueue queue 中（队列大写为 100000）。

4、SimpleMonitorService使用一个后台线程（线程名为：DubboMonitorAsyncWriteLogThread）将queue中的数据写入文件（该线程以死循环的形式来写）。

5、SimpleMonitorService 还会使用一个含有 1 个线程（线程名字：DubboMonitorTimer）的线程池每隔5分钟，将文件中的统计数据画成图表。

# 14. Dubbo 需要 Web 容器启动吗？
Dubbo不需要Web容器启动。

如果硬要使用Web容器（类似Tomcat应用服务器等），只会增加复杂性，且浪费服务器资源。

# 15. 为什么 Dubbo 不需要 Web 容器启动？
Dubbo服务容器是一个standalone的启动程序，因为后台服务不需要Tomcat或JBoss等Web容器的功能。

如果硬要使用Web容器去加载服务提供方，增加复杂性，也浪费资源。

服务容器只是一个简单的Main方法，并加载一个简单的Spring容器，用于暴露服务。

服务容器的加载内容可以扩展，内置了spring、jetty、log4j等加载，可通过Container扩展点进行扩展，

参见：Container Spring Container自动加载META-INF/spring目录下的所有Spring配置。

# 16. Dubbo 支持哪几种配置方式？
1、Spring配置方式

1）XML配置文件方式

2）properties配置文件方式

3）annotation配置方式

2、Java API配置方式。

# 17. Dubbo 启动时依赖服务不可用会造成什么问题？
Dubbo缺省会在启动时检查依赖的服务是否可用，不可用时会抛出异常，阻止Spring初始化完成，默认check="true"，可以通过check="false"关闭检查。

# 18. Dubbo 推荐使用什么序列化框架，还有别的选择吗？
Dubbo推荐使用Hessian序列化。

序列化框架还有Duddo、FastJson、Java自带序列化。

# 19. Provider 上配置 Consumer 端的属性有哪些？
1）timeout：方法调用超时。

2）retries：失败重试次数，默认重试2次。

3）loadbalance：负载均衡算法，默认随机。

4）actives 消费者端，最大并发调用限制。

# 20. 注册同一服务，如何指定某一服务？
可以配置环境点对点直连，绕过注册中心，将以服务接口为单位，忽略注册中心的提供者列表。
