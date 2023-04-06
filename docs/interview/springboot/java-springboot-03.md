# 1. Spring Boot 中 Actuator 有什么作用？
本质上Actuator通过启用production-ready功能使得SpringBoot应用程序变得更有生命力。这些功能允许对生产环境中的应用程序进行监视和管理。

集成SpringBoot Actuator到项目中非常简单。只需要做的是将spring-boot-starter-actuator starter引入到POM.xml文件当中：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```
SpringBoot Actuaor可以使用HTTP或者JMX endpoints来浏览操作信息。大多数应用程序都是用HTTP，作为endpoint的标识以及使用/actuator前缀作为URL路径。

一些常用的内置endpoints Actuator：

auditevents：查看 audit 事件信息 env：查看 环境变量 health：查看应用程序健康信息 httptrace：展示 HTTP 路径信息 info：展示 arbitrary 应用信息 metrics：展示 metrics 信息 loggers：显示并修改应用程序中日志器的配置 mappings：展示所有 @RequestMapping 路径信息 scheduledtasks：展示应用程序中的定时任务信息 threaddump：执行 Thread Dump

# 2. Spring Boot 有什么外部配置的可能来源？
SpringBoot对外部配置提供了支持，允许在不同环境中运行相同的应用。可以使用properties文件、YAML文件、环境变量、系统参数和命令行选项参数来声明配置属性。

然后可以通过@Value这个通过@ConfigurationProperties绑定的对象的注解或者实现Enviroment来访问这些属性。

以下是最常用的外部配置来源：

命令行属性： 命令行选项参数是以双连字符（例如，=）开头的程序参数，例如–server.port=8080。SpringBoot将所有参数转换为属性并且添加到环境属性当中。

应用属性： 应用属性是指那些从application.properties文件或者其YAML副本中获得的属性。默认情况下，SpringBoot会从当前目录、classpath根目录或者它们自身的config子目录下搜索该文件。

特定profile配置： 特殊概要配置是从application-{profile}.properties文件或者自身的YAML副本。{profile}占位符引用一个在用的profile。这些文件与非特定配置文件位于相同的位置，并且优先于它们。

# 3. Spring Boot 支持松绑定表示什么含义？
SpringBoot中的松绑定适用于配置属性的类型安全绑定。使用松绑定，环境属性的键不需要与属性名完全匹配。这样就可以用驼峰式、短横线式、蛇形式或者下划线分割来命名。

例如，在一个有@ConfigurationProperties声明的bean类中带有一个名为myProp的属性，它可以绑定到以下任何一个参数中，myProp、my-prop、my_prop或者MY_PROP。

# 4. Spring Boot 如何注册一个定制的自动化配置？
为了注册一个自动化配置类，必须在META-INF/spring.factories文件中的EnableAutoConfiguration键下列出它的全限定名：

```properties
org.springframework.boot.autoconfigure.EnableAutoConfiguration=com.baeldung.autoconfigure.CustomAutoConfiguration
```
如果使用Maven构建项目，这个文件需要放置在package阶段被写入完成的resources/META-INF目录中。

# 5. 什么是 Swagger？Spring Boot 如何实现 Swagger？
Swagger广泛用于可视化API，使用Swagger UI为前端开发人员提供在线沙箱。

Swagger是用于生成RESTful Web服务的可视化表示的工具，规范和完整框架实现。它使文档能够以与服务器相同的速度更新。当通过 Swagger正确定义时，消费者可以使用最少量的实现逻辑来理解远程服务并与其进行交互。因此，Swagger消除了调用服务时的猜测。

# 6. 如何使用 Spring Boot 实现分页和排序？
使用Spring Boot实现分页非常简单。使用Spring Data JPA可以实现将可分页的传递给存储库方法。

# 7. 如何使用 Maven 来构建一个 Spring Boot 程序？
就像引入其他库一样，可以在Maven工程中加入SpringBoot依赖。然而，最好是从spring-boot-starter-parent 项目中继承以及声明依赖到Spring Boot starters。这样做可以使得项目可以重用SpringBoot的默认配置。

继承spring-boot-starter-parent项目依赖很简单，只需要在pom.xml中定义一个parent节点：

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.1.1.RELEASE</version>
</parent>
```
可以在Maven central中找到spring-boot-starter-parent的最新版本。

# 8. Spring Boot web 应用程序如何部署为 JAR 或 WAR 文件？
通常，将web应用程序打包成WAR文件，然后将它部署到另外的服务器上。这样做使得能够在相同的服务器上处理多个项目。当CPU和内存有限的情况下，这是一种最好的方法来节省资源。

然而，事情发生了转变。现在的计算机硬件相比起来已经很便宜了，并且现在的注意力大多转移到服务器配置上。部署中对服务器配置的一个细小的失误都会导致无可预料的灾难发生。

Spring通过提供插件来解决这个问题，也就是spring-boot-maven-plugin来打包web应用程序到一个额外的JAR文件当中。引入这个插件只需要在pom.xml中添加一个plugin属性：

```xml
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
</plugin>
```
有了这个插件，在执行package步骤后得到一个JAR包。这个JAR包包含所需的所有依赖以及一个嵌入的服务器。因此，不再需要担心去配置一个额外的服务器了。

通过运行一个普通的JAR包来启动应用程序。

注意一点，为了打包成JAR文件，pom.xml中的packgaing属性必须定义为jar：

```xml
<packaging>jar</packaging>
```
如果不定义这个元素，它的默认值也为jar。

如果想构建一个WAR文件，将packaging元素修改为war：

```xml
<packaging>war</packaging>
```
并且将容器依赖从打包文件中移除：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-tomcat</artifactId>
    <scope>provided</scope>
</dependency>
```
执行Maven的package步骤之后，可以得到一个可部署的WAR文件。

# 9. 什么是 WebSocket？
WebSocket是一种计算机通信协议，通过单个 TCP 连接提供全双工通信信道。

1、WebSocket是双向的—使用WebSocket客户端或服务器可以发起消息发送。

2、WebSocket是全双工的—客户端和服务器通信是相互独立的。

3、单个TCP连接—初始连接使用HTTP，然后将此连接升级到基于套接字的连接。然后这个单一连接用于所有未来的通信

4、Light与http相比，WebSocket消息数据交换要轻得多。

# 10. Spring Boot 和 Spring 有什么区别？
Spring框架提供多种特性使得web应用开发变得更简便，包括依赖注入、数据绑定、切面编程、数据存取等等。

随着时间推移，Spring生态变得越来越复杂了，并且应用程序所必须的配置文件也令人觉得可怕。这就是Spirng Boot派上用场的地方了，它使得Spring的配置变得更轻而易举。

实际上Spring是unopinionated（予以配置项多，倾向性弱）的，Spring Boot在平台和库的做法中更opinionated，使得我们更容易上手。

这里有两条SpringBoot带来的好处：

1）根据classpath中的artifacts的自动化配置应用程序；

2）提供非功能性特性例如安全和健康检查给到生产环境中的应用程序。

# 11. 常见的系统架构风格有哪些?各有什么优缺点?
**1、单体架构**

单体架构也称之为单体系统或者是单体应用。就是一种把系统中所有的功能、模块耦合在一个应用中的架构方式。

单体架构特点：打包成一个独立的单元(导成一个唯一的jar包或者是war包)，会一个进程的方式来运行。

单体架构的优点、缺点

**优点：**

项目易于管理

部署简单

**缺点：**

测试成本高

可伸缩性差

可靠性差

迭代困难

跨语言程度差

团队协作难

**2、MVC架构**

MVC架构特点：

MVC是模型(Model)、视图(View)、控制器(Controller)3个单词的缩写。 下面我们从这3个方面来讲解MVC中的三个要素。

Model是指数据模型，是对客观事物的抽象。 如一篇博客文章，我们可能会以一个Post类来表示，那么，这个Post类就是数据对象。 同时，博客文章还有一些业务逻辑，如发布、回收、评论等，这一般表现为类的方法，这也是model的内容和范畴。 对于Model，主要是数据、业务逻辑和业务规则。相对而言，这是MVC中比较稳定的部分，一般成品后不会改变。 开发初期的最重要任务，主要也是实现Model的部分。这一部分写得好，后面就可以改得少，开发起来就快。

View是指视图，也就是呈现给用户的一个界面，是model的具体表现形式，也是收集用户输入的地方。 如你在某个博客上看到的某一篇文章，就是某个Post类的表现形式。 View的目的在于提供与用户交互的界面。换句话说，对于用户而言，只有View是可见的、可操作的。 事实上也是如此，你不会让用户看到Model，更不会让他直接操作Model。 你只会让用户看到你想让他看的内容。 这就是View要做的事，他往往是MVC中变化频繁的部分，也是客户经常要求改来改去的地方。 今天你可能会以一种形式来展示你的博文，明天可能就变成别的表现形式了。

Contorller指的是控制器，主要负责与model和view打交道。 换句话说，model和view之间一般不直接打交道，他们老死不相往来。view中不会对model作任何操作， model不会输出任何用于表现的东西，如HTML代码等。这俩甩手不干了，那总得有人来干吧，只能Controller上了。 Contorller用于决定使用哪些Model，对Model执行什么操作，为视图准备哪些数据，是MVC中沟通的桥梁。

MVC架构优缺点

**优点：**

各施其职，互不干涉。

在MVC模式中，三个层各施其职，所以如果一旦哪一层的需求发生了变化，就只需要更改相应的层中的代码而不会影响到其它层中的代码。

有利于开发中的分工。

在MVC模式中，由于按层把系统分开，那么就能更好的实现开发中的分工。网页设计人员可以进行开发视图层中的JSP，对业务熟悉的开发人员可开发业务层，而其它开发人员可开发控制层。

有利于组件的重用。

分层后更有利于组件的重用。如控制层可独立成一个能用的组件，视图层也可做成通用的操作界面。

**缺点：**

增加了系统结构和实现的复杂性。

视图与控制器间的过于紧密的连接。

视图对模型数据的低效率访问。

**3、面向服务架构(SOA)**

面向服务的架构(SOA)是一个组件模型，它将应用程序拆分成不同功能单元(称为服务)通过这些服务之间定义良好的接口和契约联系起来。接口是采用中立的方式进行定义的，它应该独立于实现服务的硬件平台、操作系统和编程语言。这使得构建在各种各样的系统中的服务可以以一种统一和通用的方式进行交互。

面向服务架构特点：

系统是由多个服务构成

每个服务可以单独独立部署

每个服务之间是松耦合的。服务内部是高内聚的，外部是低耦合的。高内聚就是每个服务只关注完成一个功能。

服务的优点、缺点

**优点：**

测试容易

可伸缩性强

可靠性强

跨语言程度会更加灵活

团队协作容易

系统迭代容易

**缺点：**

运维成本过高，部署数量较多

接口兼容多版本

分布式系统的复杂性

分布式事务

# 12. 什么是 AKF 拆分原则？
业界对于可扩展的系统架构设计有一个朴素的理念,就是：通过加机器就可以解决容量和可用性问题。(如果一台不行那就两台)。

讲个段子：世界上没有什么事是一顿饭不能解决的。如果有，那就两顿。

这一理念在“云计算”概念疯狂流行的今天，得到了广泛的认可!对于一个规模迅速增长的系统而言，容量和性能问题当然是首当其冲的。但是随着时间的向前，系统规模的增长，除了面对性能与容量的问题外，还需要面对功能与模块数量上的增长带来的系统复杂性问题以及业务的变化带来的提供差异化服务问题。而许多系统，在架构设计时并未充分考虑到这些问题，导致系统的重构成为常态，从而影响业务交付能力，还浪费人力财力!对此，《可扩展的艺术》一书提出了一个更加系统的可扩展模型—— AKF可扩展立方 (Scalability Cube) 。这个立方体中沿着三个坐标轴设置分别为：X、Y、Z。

Y轴扩展会将庞大的整体应用拆分为多个服务。每个服务实现一组相关的功能，如订单管理、客户管理等。在工程上常见的方案是 服务化架构(SOA) 。比如对于一个电子商务平台，我们可以拆分成不同的服务

X轴扩展与我们前面朴素理念是一致的，通过绝对平等地复制服务与数据，以解决容量和可用性的问题。其实就是将微服务运行多个实例，做集群加负载均衡的模式。

Z轴扩展通常是指基于请求者或用户独特的需求，进行系统划分，并使得划分出来的子系统是相互隔离但又是完整的。以生产汽车的工厂来举例：福特公司为了发展在中国的业务，或者利用中国的廉价劳动力，在中国建立一个完整的子工厂，与美国工厂一样，负责完整的汽车生产。这就是一种Z轴扩展。
