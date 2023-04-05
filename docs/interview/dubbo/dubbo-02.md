# 1. Dubbo 中服务上线如何兼容旧版本？
可以使用版本号（version）过渡，多个不同版本的服务注册到注册中心，版本号不同的服务相互间不引用。类似服务分组的概念。

# 2. Dubbo 支持对结果进行缓存吗？
Dubbo支持对结果进行缓存。

Dubbo 提供了声明式缓存，用于提高数据的访问速度，以减少用户加缓存的工作量。

```xml
<dubbo:reference cache="true" />
```

比普通配置文件增加标签cache="true"即可。

# 3. Dubbo 支持分布式事务吗？
目前暂时不支持，可与通过tcc-transaction框架实现。

TCC-Transaction是开源的TCC补偿性分布式事务框架。

TCC-Transaction通过Dubbo隐式传参的功能，避免自己对业务代码的入侵。

# 4. Dubbo 中使用了哪些设计模式？
Dubbo 中使用了哪些设计模式？

Dubbo框架在初始化和通信过程中使用了多种设计模式，可灵活控制类加载、权限控制等功能。

工厂模式

Provider在export服务时，会调用ServiceConfig的export方法。ServiceConfig中有个字段：

```java
private static final Protocol protocol = ExtensionLoader.getExtensionLoader(Protocol.class).getAdaptiveExtension();
```
Dubbo中有很多这种代码。这也是一种工厂模式，只是实现类的获取采用了JDK SPI的机制。

实现优点是可扩展性强，想要扩展实现，只需要在classpath下增加个文件就可以实现代码零侵入。

另外，像上面的Adaptive实现，可以做到调用时动态决定调用哪个实现，但是由于这种实现采用了动态代理，会造成代码调试比较麻烦，需要分析出实际调用的实现类。

**装饰器模式**

Dubbo在启动和调用阶段都大量使用了装饰器模式。 以Provider提供的调用链为例，具体的调用链代码是在ProtocolFilterWrapper的buildInvokerChain完成的，具体是将注解中含有group=provider的Filter实现，按照order排序，最后的调用顺序是：

EchoFilter -> ClassLoaderFilter -> GenericFilter -> ContextFilter -> ExecuteLimitFilter -> TraceFilter -> TimeoutFilter -> MonitorFilter -> ExceptionFilter

更确切地说，这里是装饰器和责任链模式的混合使用。例如，EchoFilter的作用是判断是否是回声测试请求，是的话直接返回内容，这是一种责任链的体现。

而像ClassLoaderFilter 则只是在主功能上添加了功能，更改当前线程的 ClassLoader，这是典型的装饰器模式。

**观察者模式**

Dubbo的Provider启动时，需要与注册中心交互，先注册自己的服务，再订阅自己的服务，订阅时，采用了观察者模式，开启一个listener。

注册中心会每5秒定时检查是否有服务更新，如果有更新，向该服务的提供者发送一个notify消息，provider 接受到notify消息后，即运行NotifyListener的notify方法，执行监 听器方法。

**动态代理模式**

Dubbo扩展JDK SPI的类ExtensionLoader的Adaptive实现是典型的动态代理实现。

Dubbo需要灵活地控制实现类，即在调用阶段动态地根据参数决定调用哪个实现类，所以采用先生成代理类的方法，能够做到灵活的调用。

生成代理类的代码是ExtensionLoader的createAdaptiveExtensionClassCode方法。

代理类的主要逻辑是获取URL参数中指定参数的值作为获取实现类的key。

# 5. 你了解过 Dubbo 源码吗？
面试时不管看没看过，都要说看过一些，还是多了解了解Dubbo源码吧。

如果要了解Dubbo是必须看源码的，熟悉其原理，需要花点功夫，可以在网上找一些相关的教程，后续“Java精选”公众号上也会分享Dubbo的源码分析。

# 6. Dubbo 中服务暴露的过程？
Dubbo在Spring实例化完bean之后，在刷新容器最后一步发布ContextRefreshEvent事件的时候，通知实现了ApplicationListener的ServiceBean类进行回调onApplicationEvent事件方法，Dubbo会在这个方法中调用ServiceBean父类ServiceConfig的export方法，而该方法真正实现了服务的（异步或者非异步）发布。

# 7. Dubbo 服务降级失败，如何重试？
Dubbo服务降级失败可以通过dubbo:reference中设置mock="return null"。

mock的值也可以修改为true，然后再跟接口同一个路径下实现一个Mock类，命名规则是 “接口名称+Mock” 后缀，最后在Mock类里实现自己的降级逻辑。

# 8. 服务提供者服务失效踢出是什么原理？
服务失效踢出基于Zookeeper的临时节点原理。

# 9. Dubbo 和 Dubbox 有哪些区别？
Dubbox是继Dubbo停止维护后，当当网基于Dubbo做的一个扩展项目，例如增加了Restful调用，更新了开源组件等。

# 10. Dubbo 必须依赖的包有哪些？
Dubbo必须依赖JDK，其他包为可选。

# 11. Dubbo telnet 命令有什么用处？
Dubbo服务发布成功后，可以利用telnet命令进行调试、管理。

Dubbo2.0.5以上版本服务提供端口支持telnet命令。

连接服务

```shell
telnet localhost 20880 //键入回车进入 Dubbo 命令模式。
```

查看服务列表：

```shell
dubbo>ls
com.test.TestService
dubbo>ls com.test.TestService
create
delete
query
```
>ls (list services and methods) ls： 显示服务列表。 ls -l: 显示服务详细信息列表。 ls XxxService：显示服务的方法列表。 ls -l XxxService：显示服务的方法详细信息列表

# 12. Dubbo SPI 和 JDK SPI 有哪些区别？
JDK SPI

JDK标准的SPI会一次性加载所有的扩展实现，如果有的扩展吃实话很耗时，但也没用上，很浪费资源。所以只希望加载某个的实现，就不现实了

DUBBO SPI

1、对Dubbo进行扩展，不需要改动Dubbo的源码。

2、延迟加载，可以一次只加载自己想要加载的扩展实现。

3、增加了对扩展点IOC和AOP的支持，一个扩展点可以直接setter注入其它扩展点。

4、Dubbo的扩展机制能很好的支持第三方IoC容器，默认支持Spring Bean。

# 13. Dubbo 配置文件如何加载到 Spring 中？
Spring容器在启动的时候会读取到Spring默认的一些schema以及Dubbo自定义的schema，每个schema都会对应一个自己的NamespaceHandler，NamespaceHandler里面通过BeanDefinitionParser来解析配置信息并转化为需要加载的bean对象。

# 14. Dubbo 在大数据量情况下使用什么协议？
Dubbo的设计目的是为了满足高并发且数据量小的rpc调用，在大数据量下的性能表现并不好，建议使用rmi或http协议。

# 15. Dubbo 服务读写如何实现容错策略？
读操作建议使用Failover失败自动切换，默认重试两次其他服务器。

写操作建议使用Failfast快速失败，发一次调用失败就立即报错。

# 16. Dubbo 管理控制台有什么功能？
管理控制台主要包含：路由规则，动态配置，服务降级，访问控制，权重调整，负载均衡，等管理功能。

# 17. Dubbo 中有哪些节点角色？
Provider: 暴露服务的服务提供方。

Consumer: 调用远程服务的服务消费方。

Registry: 服务注册与发现的注册中心。

Monitor: 统计服务的调用次调和调用时间的监控中心。

Container: 服务运行容器。

# 18. Dubbo 支持集成 Spring Boot 吗？
Dubbo支持集成Spring Boot。

Apache Dubbo Spring Boot项目可以使用Dubbo作为RPC框架轻松创建Spring Boot应用程序。关注Java精选公众号，源码分析持续更新。

项目地址：

[https://github.com/apache/dubbo-spring-boot-project](https://github.com/apache/dubbo-spring-boot-project)

更重要的是提供：

自动配置功能（例如，注释驱动、自动配置、外部化配置）。

生产就绪功能（例如，安全性、健康检查、外部化配置）。

Apache Dubbo是一个高性能、轻量级、基于java的RPC框架。Dubbo提供了三个关键功能，包括基于接口的远程调用、容错和负载均衡以及自动服务注册和发现。

# 19. 除了 Dubbo，还了解那些分布式框架吗？
Spring cloud、Facebook（脸书）的Thrift、Twitter的Finagle等。

# 20. 简述一下 Dubbo 四种负载均衡策略？
Dubbo实现了常见的集群策略，并提供扩展点予以自行实现。Dubbo四种常见负载均衡策略。

Random LoadBalance

随机模式：随机选取提供者策略，随机转发请求，可以加权

RoundRobin LoadBalance

轮询模式：轮循选取提供者策略，请求平均分布

LeastActive LoadBalance

最少活跃调用数：最少活跃调用策略，可以让慢提供者接收更少的请求

ConstantHash LoadBalance

一致hash：一致性 Hash 策略，相同参数请求总是发到同一提供者，一台机器宕机，可以基于虚拟节点，分摊至其他提供者

缺省时为随机模式（Random LoadBalance）。
