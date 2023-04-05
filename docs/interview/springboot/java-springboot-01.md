# 1. 什么是 Spring Boot 框架？
Spring Boot是由Pivotal团队提供的全新框架，其设计目的是用来简化新Spring应用的初始搭建以及开发过程。

Spring Boot框架使用了特定的方式来进行配置，从而使开发人员不再需要定义样板化的配置。通过这种方式，Spring Boot致力于在蓬勃发展的快速应用开发领域（rapid application development）成为领导者。

2014年4月发布第一个版本的全新开源的Spring Boot轻量级框架。它基于Spring4.0设计，不仅继承了Spring框架原有的优秀特性，而且还通过简化配置来进一步简化了Spring应用的整个搭建和开发过程。

另外Spring Boot通过集成大量的框架使得依赖包的版本冲突，以及引用的不稳定性等问题得到了很好的解决。

# 2. Spring Boot 框架的优缺点？
**Spring Boot优点**

1）创建独立的Spring应用程序

Spring Boot以jar包的形式独立运行，使用java -jar xx.jar命令运行项目或在项目的主程序中运行main方法。

2）Spring Boot内嵌入Tomcat，Jetty或者Undertow，无序部署WAR包文件

Spring项目部署时需要在服务器上部署tomcat，然后把项目打成war包放到tomcat中webapps目录。

Spring Boot项目不需要单独下载Tomcat等传统服务器，内嵌容器，使得可以执行运行项目的主程序main函数，让项目快速运行，另外，也降低对运行环境的基本要求，环境变量中有JDK即可。

3）Spring Boot允许通过maven工具根据需要获取starter

Spring Boot提供了一系列的starter pom用来简化我们的Maven依赖，通过这些starter项目就能以Java Application的形式运行Spring Boot项目，而无需其他服务器配置。

starter pom：

>[https://docs.spring.io/spring-boot/docs/1.4.1.RELEASE/reference/htmlsingle/#using-boot-starter](https://docs.spring.io/spring-boot/docs/1.4.1.RELEASE/reference/htmlsingle/#using-boot-starter)

4）Spring Boot尽可能自动配置Spring框架

Spring Boot提供Spring框架的最大自动化配置，使用大量自动配置，使得开发者对Spring的配置减少。

Spring Boot更多的是采用Java Config的方式，对Spring进行配置。

5）提供生产就绪型功能，如指标、健康检查和外部配置

Spring Boot提供了基于http、ssh、telnet对运行时的项目进行监控；可以引入spring-boot-start-actuator依赖，直接使用REST方式来获取进程的运行期性能参数，从而达到监控的目的，比较方便。

但是Spring Boot只是微框架，没有提供相应的服务发现与注册的配套功能、监控集成方案以及安全管理方案，因此在微服务架构中，还需要Spring Cloud来配合一起使用，可关注微信公众号“Java精选”，后续篇幅会针对Spring Cloud面试题补充说明。

5）绝对没有代码生成，对XML没有要求配置

**Spring Boot缺点**

1）依赖包太多，一个spring Boot项目就需要很多Maven引入所需的jar包

2）缺少服务的注册和发现等解决方案

3）缺少监控集成、安全管理方案

# 3. Spring Boot 核心注解都有哪些？
1）@SpringBootApplication*

用于Spring主类上最最最核心的注解，自动化配置文件，表示这是一个SpringBoot项目，用于开启SpringBoot的各项能力。

相当于@SpringBootConfigryation、@EnableAutoConfiguration、@ComponentScan三个注解的组合。

2）@EnableAutoConfiguration

允许SpringBoot自动配置注解，开启这个注解之后，SpringBoot就能根据当前类路径下的包或者类来配置Spring Bean。

如当前路径下有MyBatis这个Jar包，MyBatisAutoConfiguration 注解就能根据相关参数来配置Mybatis的各个Spring Bean。

3）@Configuration

Spring 3.0添加的一个注解，用来代替applicationContext.xml配置文件，所有这个配置文件里面能做到的事情都可以通过这个注解所在的类来进行注册。

4）@SpringBootConfiguration

@Configuration注解的变体，只是用来修饰Spring Boot的配置而已。

5）@ComponentScan

Spring 3.1添加的一个注解，用来代替配置文件中的component-scan配置，开启组件扫描，自动扫描包路径下的@Component注解进行注册bean实例放到context(容器)中。

6）@Conditional

Spring 4.0添加的一个注解，用来标识一个Spring Bean或者Configuration配置文件，当满足指定条件才开启配置

7）@ConditionalOnBean

组合@Conditional注解，当容器中有指定Bean才开启配置。

8）@ConditionalOnMissingBean

组合@Conditional注解，当容器中没有值当Bean才可开启配置。

9）@ConditionalOnClass

组合@Conditional注解，当容器中有指定Class才可开启配置。

10）@ConditionalOnMissingClass

组合@Conditional注解，当容器中没有指定Class才可开启配置。

11）@ConditionOnWebApplication

组合@Conditional注解，当前项目类型是WEB项目才可开启配置。

**项目有以下三种类型：**

① ANY：任意一个Web项目

② SERVLET： Servlet的Web项目

③ REACTIVE ：基于reactive-base的Web项目

12） @ConditionOnNotWebApplication

组合@Conditional注解，当前项目类型不是WEB项目才可开启配置。

13）@ConditionalOnProperty

组合@Conditional注解，当指定的属性有指定的值时才可开启配置。

14）@ConditionalOnExpression

组合@Conditional注解，当SpEl表达式为true时才可开启配置。

15）@ConditionOnJava

组合@Conditional注解，当运行的Java JVM在指定的版本范围时才开启配置。

16）@ConditionalResource

组合@Conditional注解，当类路径下有指定的资源才开启配置。

17）@ConditionOnJndi

组合@Conditional注解，当指定的JNDI存在时才开启配置。

18）@ConditionalOnCloudPlatform

组合@Conditional注解，当指定的云平台激活时才可开启配置。

19）@ConditiomalOnSingleCandidate

组合@Conditional注解，当制定的Class在容器中只有一个Bean，或者同时有多个但为首选时才开启配置。

20）@ConfigurationProperties

用来加载额外的配置(如.properties文件)，可用在@Configuration注解类或者@Bean注解方法上面。可看一看Spring Boot读取配置文件的几种方式。

21）@EnableConfigurationProperties

一般要配合@ConfigurationProperties注解使用，用来开启@ConfigurationProperties注解配置Bean的支持。

22）@AntoConfigureAfter

用在自动配置类上面，便是该自动配置类需要在另外指定的自动配置类配置完之后。如Mybatis的自动配置类，需要在数据源自动配置类之后。

23）@AutoConfigureBefore

用在自动配置类上面，便是该自动配置类需要在另外指定的自动配置类配置完之前。

24）@Import

Spring 3.0添加注解，用来导入一个或者多个@Configuration注解修饰的配置类。

25）@IMportReSource

Spring 3.0添加注解，用来导入一个或者多个Spring配置文件，这对Spring Boot兼容老项目非常有用，一位内有些配置文件无法通过java config的形式来配置

# 4. Spring Boot 的目录结构是怎样的？
**1、代码层的结构**

根目录：com.springboot

1）工程启动类(ApplicationServer.java)置于com.springboot.build包下

2）实体类(domain)置于com.springboot.domain

3）数据访问层(Dao)置于com.springboot.repository

4）数据服务层(Service)置于com,springboot.service,数据服务的实现接口(serviceImpl)至于com.springboot.service.impl

5）前端控制器(Controller)置于com.springboot.controller

6）工具类(utils)置于com.springboot.utils

7）常量接口类(constant)置于com.springboot.constant

8）配置信息类(config)置于com.springboot.config

9）数据传输类(vo)置于com.springboot.vo

**2、资源文件的结构**

根目录：src/main/resources

1）配置文件(.properties/.json等)置于config文件夹下

2）国际化(i18n)置于i18n文件夹下

3）spring.xml置于META-INF/spring文件夹下

4）页面以及js/css/image等置于static文件夹下的各自文件下

# 5. Spring Boot 需要独立的容器运行吗？
Spring Boot项目可以不需要，内置了Tomcat/Jetty等容器，默认Tomcat。

Spring Boot不需要独立的容器就可以运行，因为在Spring Boot工程发布的jar文件里已经包含了tomcat插件的jar文件。

Spring Boot运行时创建tomcat对象实现web服务功能，另外也可以将Spring Boot编译成war包文件放到tomcat中运行。

# 6. Spring Boot 运行方式有哪几种？
1）直接执行main方法运行，通过IDE工具运行Application这个类的main方法

2）使用Maven插件spring-boot-plugin方式启动，在Spring Boot应用的根目录下运行mvn spring-boot:run

3）使用mvn install生成jar后通过java -jar命令运行

# 7. Spring Boot 自动配置原理是什么？
Spring Boot的启动类中使用了@SpringBootApplication注解，里面的@EnableAutoConfiguration注解是自动配置的核心，注解内部使用@Import(AutoConfigurationImportSelector.class)（class文件用来哪些加载配置类）注解来加载配置类，并不是所有的bean都会被加载，在配置类或bean中使用@Condition来加载满足条件的bean。

@EnableAutoConfiguration给容器导入META-INF/spring.factories中定义的自动配置类，筛选有效的自动配置类。每一个自动配置类结合对应的xxxProperties.java读取配置文件进行自动配置功能

# 8. Spring Boot 热部署有几种方式？
1）spring-boot-devtools

通过Springboot提供的开发者工具spring-boot-devtools来实现，在pom.xml引用其依赖。

然后在Settings→Build→Compiler中将Build project automatically勾选上，最后按ctrl+shift+alt+/ 选择registy，将compiler.automake.allow.when.app.running勾选。

2）Spring Loaded

Spring官方提供的热部署程序，实现修改类文件的热部署

下载Spring Loaded（项目地址[https://github.com/spring-projects/spring-loaded](https://github.com/spring-projects/spring-loaded)）

添加运行时参数：-javaagent:C:/springloaded-1.2.5.RELEASE.jar –noverify

3）JRebel

收费的一个热部署软件，安装插件使用即可。

# 9. Spring Boot 支持哪几种内嵌容器？
Spring Boot支持的内嵌容器有Tomcat（默认）、Jetty、Undertow和Reactor Netty（v2.0+），借助可插拔（SPI）机制的实现，开发者可以轻松进行容器间的切换。

# 10. 什么是 Spring Boot Stater？
Spring Boot在配置上相比Spring要简单许多，其核心在于Spring Boot Stater。

Spring Boot内嵌容器支持Tomcat、Jetty、Undertow等应用服务的starter启动器，在应用启动时被加载，可以快速的处理应用所需要的一些基础环境配置。

starter解决的是依赖管理配置复杂的问题，可以理解成通过pom.xml文件配置很多jar包组合的maven项目，用来简化maven依赖配置，starter可以被继承也可以依赖于别的starter。

比如spring-boot-starter-web包含以下依赖：

```java
org.springframework.boot:spring-boot-starter
org.springframework.boot:spring-boot-starter-tomcat
org.springframework.boot:spring-boot-starter-validation
com.fasterxml.jackson.core:jackson-databind
org.springframework:spring-web
org.springframework:spring-webmvc
```
starter负责配与Sping整合相关的配置依赖等，使用者无需关心框架整合带来的问题。

比如使用Sping和JPA访问数据库，只需要项目包含spring-boot-starter-data-jpa依赖就可以完美执行。

# 11. Spring Boot Stater 有什么命名规范？
1）Spring Boot官方项目命名方式

前缀：spring-boot-starter-，其中是指定类型的应用程序名称。

格式：spring-boot-starter-

举例：spring-boot-starter-web、spring-boot-starter-jdbc

2）自定义命名方式

后缀：*-spring-boot-starter

格式：{模块名}-spring-boot-starter

举例：mybatis-spring-boot-starter

# 12. Spring Boot 启动器都有哪些？
Spring Boot在org.springframework.boot组下提供了以下应用程序启动器：

| 名称 | 	描述 |
|------------------------------------|----------------------------------------------------------------------|
| spring-boot-starter	 | 核心启动器，包括自动配置支持，日志记录和YAML | 
| spring-boot-starter-activemq	 | 使用Apache ActiveMQ的JMS消息传递启动器 | 
| spring-boot-starter-amqp	 | 使用Spring AMQP和Rabbit MQ的启动器 | 
| spring-boot-starter-aop	 | 使用Spring AOP和AspectJ进行面向方面编程的启动器 | 
| spring-boot-starter-artemis	 | 使用Apache Artemis的JMS消息传递启动器 | 
| spring-boot-starter-batch	 | 使用Spring Batch的启动器 | 
| spring-boot-starter-cache	 | 使用Spring Framework的缓存支持的启动器 | 
| spring-boot-starter-data-cassandra	 | 使用Cassandra分布式数据库和Spring Data Cassandra的启动器 | 
| spring-boot-starter-data-cassandra-reactive	 | 使用Cassandra分布式数据库和Spring Data Cassandra Reactive的启动器 | 
| spring-boot-starter-data-couchbase	 | 使用Couchbase面向文档的数据库和Spring Data Couchbase的启动器 | 
| spring-boot-starter-data-couchbase-reactive	 | 使用Couchbase面向文档的数据库和Spring Data Couchbase Reactive的启动器 | 
| spring-boot-starter-data-elasticsearch	 | 使用Elasticsearch搜索和分析引擎以及Spring Data Elasticsearch的启动器 | 
| spring-boot-starter-data-jdbc	 | 使用Spring Data JDBC的启动器 | 
| spring-boot-starter-data-jpa | 	将Spring Data JPA与Hibernate结合使用的启动器 | 
| spring-boot-starter-data-ldap	 | 使用Spring Data LDAP的启动器 | 
| spring-boot-starter-data-mongodb	 | 使用MongoDB面向文档的数据库和Spring Data MongoDB的启动器 | 
| spring-boot-starter-data-mongodb-reactive	 | 使用MongoDB面向文档的数据库和Spring Data MongoDB Reactive的启动器 | 
| spring-boot-starter-data-neo4j	 | 使用Neo4j图形数据库和Spring Data Neo4j的启动器工具 | 
| spring-boot-starter-data-r2dbc	 | 使用Spring Data R2DBC的启动器 | 
| spring-boot-starter-data-redis	 | 使用Redis键值数据存储与Spring Data Redis和Lettuce客户端的启动器 | 
| spring-boot-starter-data-redis-reactive	 | 将Redis键值数据存储与Spring Data Redis Reacting和Lettuce客户端一起使用的启动器 | 
| spring-boot-starter-data-rest	 | 使用Spring Data REST在REST上公开Spring数据存储库的启动器 | 
| spring-boot-starter-freemarker	 | 使用FreeMarker视图构建MVC Web应用程序的启动器 | 
| spring-boot-starter-groovy-templates	 | 使用Groovy模板视图构建MVC Web应用程序的启动器 | 
| spring-boot-starter-hateoas	 | 使用Spring MVC和Spring HATEOAS构建基于超媒体的RESTful Web应用程序的启动器 | 
| spring-boot-starter-integration	 | 使用Spring Integration的启动器 | 
| spring-boot-starter-jdbc	 | 结合使用JDBC和HikariCP连接池的启动器 | 
| spring-boot-starter-jersey	 | 使用JAX-RS和Jersey构建RESTful Web应用程序的启动器。的替代品spring-boot-starter-web | 
| spring-boot-starter-jooq	 | 使用jOOQ访问SQL数据库的启动器。替代spring-boot-starter-data-jpa或spring-boot-starter-jdbc | 
| spring-boot-starter-json	 | 读写JSON启动器 | 
| spring-boot-starter-jta-atomikos	 | 使用Atomikos的JTA交易启动器 | 
| spring-boot-starter-mail	 | 使用Java Mail和Spring Framework的电子邮件发送支持的启动器 | 
| spring-boot-starter-mustache	 | 使用Mustache视图构建Web应用程序的启动器 | 
| spring-boot-starter-oauth2-client	 | 使用Spring Security的OAuth2 / OpenID Connect客户端功能的启动器 | 
| spring-boot-starter-oauth2-resource-server	 | 使用Spring Security的OAuth2资源服务器功能的启动器 | 
| spring-boot-starter-quartz	 | 启动器使用Quartz Scheduler | 
| spring-boot-starter-rsocket	 | 用于构建RSocket客户端和服务器的启动器 | 
| spring-boot-starter-security	 | 使用Spring Security的启动器 | 
| spring-boot-starter-test	 | 用于使用包括JUnit Jupiter，Hamcrest和Mockito在内的库测试Spring Boot应用程序的启动器 | 
| spring-boot-starter-thymeleaf | 	使用Thymeleaf视图构建MVC Web应用程序的启动器 | 
| spring-boot-starter-validation	 | 初学者，可将Java Bean验证与Hibernate Validator结合使用 | 
| spring-boot-starter-web	 | 使用Spring MVC构建Web（包括RESTful）应用程序的启动器。使用Tomcat作为默认的嵌入式容器 | 
| spring-boot-starter-web-services | 	使用Spring Web Services的启动器 | 
| spring-boot-starter-webflux | 	使用Spring Framework的反应式Web支持构建WebFlux应用程序的启动器 | 
| spring-boot-starter-websocket	 | 使用Spring Framework的WebSocket支持构建WebSocket应用程序的启动器 | 
除应用程序启动器外，以下启动程序还可用于添加生产环境上线功能：

| 名称 | 	描述 |
|------------------------------------|----------------------------------------------------------------------|
| spring-boot-starter-actuator	 | 使用Spring Boot Actuator的程序，该启动器提供了生产环境上线功能，可帮助您监视和管理应用程序 |

Spring Boot还包括以下启动程序，如果想排除或替换启动器，可以使用这些启动程序：

| 名称                                 | 	描述                                                                  |
|------------------------------------|----------------------------------------------------------------------|
| spring-boot-starter-jetty	         | 使用Jetty作为嵌入式servlet容器的启动器。替代spring-boot-starter-tomcat               |
| spring-boot-starter-log4j2	        | 使用Log4j2进行日志记录的启动器。替代spring-boot-starter-logging                     |
| spring-boot-starter-logging	       | 使用Logback进行日志记录的启动器。默认记录启动器                                          |
| spring-boot-starter-reactor-netty	 | 启动器，用于将Reactor Netty用作嵌入式反应式HTTP服务器。                                 |
| spring-boot-starter-tomcat	        | 启动器，用于将Tomcat用作嵌入式servlet容器。默认使用的servlet容器启动器spring-boot-starter-web |
| spring-boot-starter-undertow	      | 使用Undertow作为嵌入式servlet容器的启动器。替代spring-boot-starter-tomcat            |
# 13. 什么是 JavaConfig？
Spring JavaConfig是Spring社区的产品，它提供了配置Spring IoC容器的纯Java方法。因此它有助于避免使用XML配置。

使用JavaConfig的优点在于：   1）面向对象的配置。由于配置被定义为JavaConfig中的类，因此用户可以充分利用Java 中的面向对象功能。一个配置类可以继承另一个，重写它的@Bean方法等。

2）减少或消除 XML配置。基于依赖注入原则的外化配置的好处已被证明。但是，许多开发人员不希望在XML和Java之间来回切换。JavaConfig为开发人员提供了一种纯Java方法来配置与 XML 配置概念相似的Spring容器。从技术角度来讲，只使用JavaConfig配置类来配置容器是可行的，但实际上很多人认为将JavaConfig与XML混合匹配是理想的。

3）类型安全和重构友好。JavaConfig提供了一种类型安全的方法来配置Spring容器。由于Java 5.0对泛型的支持，现在可以按类型而不是按名称检索bean，不需要任何强制转换或基于字符串的查找。

# 14. 什么是 YAML？
YAML是一种人类可读的数据序列化语言。

它通常用于配置文件。

与属性文件相比，如果我们想要在配置文件中添加复杂的属性，YAML文件就更加结构化，而且更少混淆。可以看出YAML具有分层配置数据。

# 15. YAML 配置 和 properties 配置有什么区别？
YAML现在可以算是非常流行的一种配置文件格式，无论是前端还是后端，都可以见到YAML配置。

YAML配置和相比传统的properties配置的优势：

>1、配置有序，在一些特殊的场景下，配置有序很关键； 2、支持数组，数组中的元素可以是基本数据类型也可以是对象； 3、简洁

相比properties配置文件，YAML还有一个缺点，就是不支持@PropertySource注解导入自定义的YAML配置。

正常的情况是先加载yml，接下来加载properties文件。如果相同的配置存在于两个文件中，最后会使用properties中的配置。最后读取的优先集最高。

两个配置文件中的端口号不一样会读取properties中的端口号。

# 16. Spring Boot 是否可以使用 XML 配置？
Spring Boot推荐使用Java配置而非XML配置，但是Spring Boot中也可以使用XML配置，通过@ImportResource注解可以引入一个XML配置。

@ImportResource注解用于导入Spring的配置文件，让配置文件里面的内容生效；（就是以前写的springmvc.xml、applicationContext.xml）。

# 17. Spring Boot 中核心配置文件是什么？
Spring Boot有两种类型的配置文件，application（.yml 或者.properties）和bootstrap（.yml 或者.properties）文件。

Spring Boot会自动加载classpath目前下的这两个文件，文件格式为properties或yml格式。

*.properties文件是key=value的形式

*.yml文件是key:value的形式

*.yml加载的属性是有顺序的，但不支持@PropertySource注解来导入配置，一般推荐用yml文件，看下来更加形象。

bootstrap配置文件是系统级别的，用来加载外部配置，如配置中心的配置信息，也可以用来定义系统不会变化的属性.bootstatp 文件的加载先于application文件

application配置文件是应用级别的，是当前应用的配置文件。

# 18. bootstrap.properties 和 application.properties 有何区别？
Spring Boot开发时可能不很少遇到bootstrap.properties配置文件，但是在结合Spring Cloud时，这个配置就会经常遇到，特别是在需要加载一些远程配置文件。

bootstrap（.yml或者.properties）：boostrap由父ApplicationContext加载的，比applicaton优先加载，配置在应用程序上下文的引导阶段生效。一般来说我们在 Spring Cloud Config或者Nacos中会用到它。且 boostrap里面的属性不能被覆盖；

application（.yml或者.properties）：由ApplicatonContext加载，用于spring boot项目的自动化配置。

# 19. 什么是 Spring Profiles？
Spring Profiles允许用户根据配置文件（dev，test，prod等）来注册bean。

因此，当应用程序在开发中运行时，只有某些 bean可以加载，而在PRODUCTION中，某些其他bean可以加载。

假设要求是Swagger文档仅适用于QA环境，并且禁用所有其他文档。这就可以使用配置文件来完成。

Spring Boot使得使用配置文件非常简单。

# 20. Spring Security 和 Shiro 对比有什么优缺点？
由于Spring Boot官方提供了大量的非常方便的开箱即用的Starter，包括Spring Security的Starter，使得在Spring Boot中使用Spring Security变得更加容易，甚至只需要添加一个依赖就可以保护所有的接口，所以如果是Spring Boot项目一般选择Spring Security。当然这只是一个建议的组合，单纯从技术上来说，无论怎么组合，都是没有问题的。

Shiro和Spring Security相比，主要有如下一些特点：

Spring Security是一个重量级的安全管理框架；Shiro则是一个轻量级的安全管理框架。

Spring Security概念复杂，配置繁琐；Shiro概念简单、配置简单。

Spring Security功能强大；Shiro功能简单。
