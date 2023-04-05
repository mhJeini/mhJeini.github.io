# 1. Spring 中自动装配有那些局限性？
自动装配的局限性

重写：仍需用<constructor-arg>和<property>配置来定义依赖，意味着总要重写自动装配。

基本数据类型：不能自动装配简单的属性，例如基本数据类型、String字符串、和类。

模糊特性：自动装配不如显式装配精确，如果有可能，建议使用显式装配。

# 2. Spring 管理事务默认回滚的异常有哪些？
Spring的事务管理默认只对出现运行期异常（java.lang.RuntimeException及其子类）、Error进行回滚。

假设一个方法抛出Exception或者Checked异常，Spring事务管理默认不进行回滚。

# 3. Spring 中事务如何指定回滚的异常？
在@Transaction注解中定义noRollbackFor和RollbackFor参数指定某种异常是否回滚。

```java
@Transaction(noRollbackFor=RuntimeException.class)
@Transaction(RollbackFor=Exception.class)
```

# 4. 什么是Spring IOC 容器？
Spring IOC 负责创建对象，管理对象（通过依赖注入（DI），装配对象，配置对象，并且管理这些对象的整个生命周期。

# 5. Spring 中 IOC的优点是什么？
IOC 或 依赖注入把应用的代码量降到最低。它使应用容易测试，单元测试不再需要单例和JNDI查找机制。

最小的代价和最小的侵入性使松散耦合得以实现。IOC容器支持加载服务时的饿汉式初始化和懒加载。

# 6. Spring 中 IOC 和 DI 有什么区别？
欢迎大家关注微信公众号： Java精选 ，专注分享前沿资讯，BATJ 大厂面试题解读，架构技术干货，微服务、高可用等架构设计，10年开发老兵帮你少走弯路，欢迎各领域程序员交流学习！

此类面试题只能在微信小程序： Java精选面试题 ，查阅全部内容，感谢支持！

# 7. Bean 工厂和 Application contexts 有什么区别？
Application contexts提供一种方法处理文本消息，一个通常的做法是加载文件资源（比如镜像），它们可以向注册为监听器的bean发布事件。

另外，在容器或容器内的对象上执行的那些不得不由bean工厂以程序化方式处理的操作，可以在Application contexts中以声明的方式处理。

Application contexts实现了MessageSource接口，该接口的实现以可插拔的方式提供获取本地化消息的方法。

# 8. 什么是Spring beans？
Spring beans是那些形成Spring应用的主干的java对象。它们被Spring IOC容器初始化，装配，和管理。这些beans通过容器中配置的元数据创建。比如，以XML文件中<bean/> 的形式定义。

Spring 框架定义的beans都是单件beans。

在bean tag中有个属性“singleton”，如果它被赋为TRUE，bean就是单件，否则就是一个prototype bean。默认是TRUE，所以所有在Spring框架中的beans缺省都是单件。

# 9. 一个 Spring Bean 定义包含什么？
一个Spring Bean的定义包含容器必知的所有配置元数据，包括如何创建一个bean，它的生命周期详情及它的依赖。

# 10. Spring 中如何定义类的作用域?
当定义一个<bean>在Spring里，我们还能给这个bean声明一个作用域。

它可以通过bean定义中的scope属性来定义。

例如当Spring要在需要的时候每次生产一个新的bean实例，bean的scope属性被指定为prototype。

另一方面，一个bean每次使用的时候必须返回同一个实例，这个bean的scope 属性 必须设为 singleton。

# 11. Spring 中内部 bean 是什么？
当一个bean仅被用作另一个bean的属性时，它能被声明为一个内部bean，为了定义inner bean，在Spring的基于XML的配置元数据中，可以在<property/>或<constructor-arg/> 元素内使用<bean/> 元素，内部bean通常是匿名的，它们的Scope一般是prototype。

# 12. Spring 中如何注入一个 java 集合？
Spring提供以下几种集合的配置元素：

<list>类型用于注入一列值，允许有相同的值。

<set>类型用于注入一组值，不允许有相同的值。

<map>类型用于注入一组键值对，键和值都可以为任意类型。

<props>类型用于注入一组键值对，键和值都只能为String类型。

# 13. Spring 中什么是 bean 装配？
装配或bean装配是指在Spring容器中把bean组装到一起，前提是容器需要知道bean的依赖关系，如何通过依赖注入来把它们装配到一起。

# 14. Spring 中什么是 bean 的自动装配？
Spring容器能够自动装配相互合作的bean，这意味着容器不需要<constructor-arg>和<property>配置，能通过Bean工厂自动处理bean之间的协作。

# 15. FileSystemResource 和 ClassPathResource 有何区别？
TODO

# 16. 什么是基于注解的容器配置？
相对于XML文件，注解型的配置依赖于通过字节码元数据装配组件，而非尖括号的声明。

开发者通过在相应的类，方法或属性上使用注解的方式，直接组件类中进行配置，而不是使用xml表述bean的装配关系。

# 17. Spring 中如何开启注解装配？
注解装配在默认情况下是不开启的，为了使用注解装配，必须在Spring配置文件中配置<context:annotation-config/>元素。

# 18. Spring 中如何更有效地使用 JDBC？
使用SpringJDBC框架，资源管理和错误处理的代价都会被减轻。

开发者只需写statements和queries从数据存取数据，JDBC也可以在Spring框架提供的模板类的帮助下更有效地被使用，这个模板就是JdbcTemplate。

JdbcTemplate类提供了很多便利的方法解决诸如把数据库数据转变成基本数据类型或对象，执行写好的或可调用的数据库操作语句，提供自定义的数据错误处理。

# 19. Spring 中支持那些 ORM？
Spring支持以下ORM：

Hibernate

iBatis

JPA（Java Persistence API）

TopLink

JDO（Java Data Objects）

OJB

# 20. 什么是 Spring AOP？
Spring AOP(Aspect Oriented Programming，面向切面编程)是OOPs(面向对象编程)的补充，它也提供了模块化。

在面向对象编程中关键的单元是对象，AOP的关键单元是切面，或者说关注点（可以简单地理解为你程序中的独立模块）。

一些切面可能有集中的代码，但是有些可能被分散或者混杂在一起，例如日志或者事务。这些分散的切面被称为横切关注点。

一个横切关注点是一个可以影响到整个应用的关注点，而且应该被尽量地集中到代码的一个地方，例如事务管理、权限、日志、安全等。

AOP可以使用简单可插拔的配置，在实际逻辑执行之前、之后或周围动态添加横切关注点。这让代码在当下和将来都变得易于维护。

如果使用XML来使用切面的话，要添加或删除关注点，不用重新编译完整的源代码，只需修改配置文件就可以。

Spring AOP通过两种方式来使用。但是最广泛使用的方式是Spring AspectJ注解风格(Spring AspectJ Annotation Style)。

1）使用AspectJ 注解风格。

2）使用Spring XML 配置风格。
