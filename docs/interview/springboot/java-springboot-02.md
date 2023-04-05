# 1. 什么是 Spring Batch？
Spring Boot Batch提供可重用的函数，这些函数在处理大量记录时非常重要，包括日志/跟踪，事务管理，作业处理统计信息，作业重新启动，跳过和资源管理。

Spring Boot Batch还提供了更先进的技术服务和功能，通过优化和分区技术，可以实现极高批量和高性能批处理作业。简单以及复杂的大批量批处理作业可以高度可扩展的方式利用框架处理重要大量的信息。

# 2. 什么是 CSRF 攻击？
跨站点请求伪造，指攻击者通过跨站请求，以合法的用户的身份进行非法操作。可以这么理解CSRF攻击：攻击者盗用你的身份，以你的名义向第三方网站发送恶意请求。

CRSF能做的事情包括利用你的身份发邮件，发短信，进行交易转账，甚至盗取账号信息。

CSRF攻击专门针对状态改变请求，而不是数据窃取，因为攻击者无法查看对伪造请求的响应。

# 3. Spring Boot 中监视器是什么？
Spring boot actuator是spring启动框架中的重要功能之一。

Spring boot监视器可帮助您访问生产环境中正在运行的应用程序的当前状态。有几个指标必须在生产环境中进行检查和监控。即使一些外部应用程序可能正在使用这些服务来向相关人员触发警报消息。监视器模块公开了一组可直接作为HTTP URL访问的REST端点来检查状态。

# 4. Spring Boot 中如何禁用 Actuator 端点安全性？
默认情况下，所有敏感的HTTP端点都是安全的，只有具有ACTUATOR角色的用户才能访问它们。

安全性是使用标准的HttpServletRequest.isUserInRole方法实施的。可以使用来禁用安全性。只有在执行机构端点在防火墙后访问时，才建议禁用安全性。

# 5. 如何监视所有 Spring Boot 微服务？
Spring Boot提供监视器端点以监控各个微服务的度量。这些端点对于获取有关应用程序的信息（如它们是否已启动）以及它们的组件（如数据库等）是否正常运行很有帮助。

但是，使用监视器的一个主要缺点或困难是，我们必须单独打开应用程序的知识点以了解其状态或健康状况。想象一下涉及50个应用程序的微服务，管理员将不得不击中所有50个应用程序的执行终端。

为了帮助我们处理这种情况，我们将使用位于的开源项目。它建立在Spring Boot Actuator之上，它提供了一个Web UI，使我们能够可视化多个应用程序的度量。

# 6. spring-boot-starter-parent 有什么用？
创建Spring Boot项目默认都是有parent，这个parent就是spring-boot-starter-parent，spring-boot-starter-parent主要有如下作用：

1、定义Java编译版本为1.8。

2、使用UTF-8格式编码。

3、继承自spring-boot-dependencies，这个里边定义了依赖的版本，也正是因为继承了这个依赖，所以在写依赖时才不需要写版本号。

4、执行打包操作的配置。

5、自动化的资源过滤。

6、自动化的插件配置。

7、针对application.properties和application.yml的资源过滤，包括通过profile 定义的不同环境的配置文件，例如application-dev.properties和application-dev.yml。

# 7. Spring Boot jar 和普通 jar 有什么区别？
Spring Boot项目最终打包成的jar是可执行jar，这种jar可以直接通过java-jarxxx.jar命令来运行，这种jar不可以作为普通的jar被其他项目依赖，即使依赖了也无法使用其中的类。

Spring Boot的jar无法被其他项目依赖，主要还是他和普通jar的结构不同。普通的jar包，解压后直接就是包名，包里就是开发的代码，而Spring Boot打包成的可执行jar解压后，在\BOOT-INF\classes目录下才是开发的代码，因此无法被直接引用。

如果非要引用，可以在pom.xml文件中增加配置，将Spring Boot项目打包成两个jar，一个可执行，一个可引用。

# 8. Spring Boot 中如何实现全局异常处理？
Spring提供了一种使用ControllerAdvice处理异常的非常有用的方法。通过实现一个ControlerAdvice类，来处理控制器类抛出的所有异常。

# 9. Spring Boot 中如何实现定时任务？
定时任务也是一个常见的需求，Spring Boot 中对于定时任务的支持主要还是来自Spring 框架。

Spring Boot中使用定时任务主要有两种不同的方式，一个就是使用Spring中的@Scheduled注解，另一个则是使用第三方框架Quartz。

使用Spring中的@Scheduled的方式主要通过@Scheduled注解来实现。

使用Quartz，则按照Quartz的方式，定义Job和Trigger即可。

# 10. Spring Boot 中如何实现兼容老 Spring 项目？
通过使用@ImportResource注解导入旧配置文件（注解写在启动类），方式如下：

```java
@SpringBootApplication
@ImportResource(locations = {"classpath:spring.xml"})
public class Application{
    public static void main(String[] args) {
     SpringApplication.run(Application.class, args);
    }
}
```
# 11. Spring Boot 中如何解决跨域问题？
跨域可以在前端通过JSONP来解决，但是JSONP只可以发送GET请求，无法发送其他类型的请求。

在RESTful风格的应用中，就显得非常鸡肋，因此推荐在后端通过（CORS，Cross-origin resource sharing）来解决跨域问题。

这种解决方案并非Spring Boot特有的，在传统的SSM框架中，就可以通过CORS来解决跨域问题，只不过之前是在XML文件中配置CORS，现在可以通过实现WebMvcConfigurer接口然后重写addCorsMappings方法解决跨域问题。

```java
@Configuration
public class CorsConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
        .allowedOrigins("*")
        .allowCredentials(true)
        .allowedMethods("GET", "POST", "PUT", "DELETE", "OPTIONS")
        .maxAge(3600);
    }
}
```
项目中前后端分离部署，所以需要解决跨域的问题。使用cookie存放用户登录的信息，在spring拦截器进行权限控制，当权限不符合时，直接返回给用户固定的json结果。

注意：当用户退出登录状态时或者token过期时，由于拦截器和跨域的顺序有问题，出现了跨域的现象。http请求先经过filter，到达servlet后才进行拦截器的处理，如果把cors放在filter中就可以优先于权限拦截器执行。

```java
@Configuration
public class CorsConfig {
    @Bean
    public CorsFilter corsFilter() {
        CorsConfiguration corsConfiguration = new CorsConfiguration();
        corsConfiguration.addAllowedOrigin("*");
        corsConfiguration.addAllowedHeader("*");
        corsConfiguration.addAllowedMethod("*");
        corsConfiguration.setAllowCredentials(true);
        UrlBasedCorsConfigurationSource urlBasedCorsConfigurationSource = new UrlBasedCorsConfigurationSource();
        urlBasedCorsConfigurationSource.registerCorsConfiguration("/**", corsConfiguration);
        return new CorsFilter(urlBasedCorsConfigurationSource);
    }
}
```
# 12. Spring Boot 内嵌容器默认是什么？
Spring Boot默认内嵌容器Tomcat。

Spring Boot的web应用开发必须使用spring-boot-starter-web，其默认嵌入的servlet容器是Tomcat。

```xml
<parent>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-parent</artifactId>
   <version>1.4.3.RELEASE</version>
</parent>
```
```xml
<dependencies>
   <!-- TOMCAT -->
   <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
   </dependency>
</dependencies>
```
# 13. Spring Boot 2.X 有什么新特性？与 1.X 有什么区别？
1、配置变更。

2、JDK版本升级。

3、第三方类库升级。

4、响应式Spring编程支持。

5、HTTP/2支持。

6、配置属性绑定。

7、更多改进与加强。

# 14. Spring、Spring MVC 和 Spring Boot 有什么区别？
Spring

Spring最重要的特征是依赖注入。所有Spring Modules不是DI依赖注入就是IOC控制反转。

合适的场景下使用DI（依赖注入）或者IOC（控制反转）的时候，可以开发松耦合应用。

所谓的IOC称之为控制反转，简单来说就是将对象的创建的权力及对象的生命周期的管理过程交由Spring框架来处理，从此在开发过程中不在需要关注对象的创建和生命周期的管理，而是在需要的时候由Spring框架提供，这个由Spring框架管理对象创建和生命周期的机制称之为控制反转。而在创建对象的过程中Spring可以依据配置对象的属性进行设置，这个过程称之为依赖注入，也即DI。

Spring MVC

Spring MVC提供了一种分离式的方法来开发Web应用。通过运用像DispatcherServelet、MoudlAndView和ViewResolver等一些简单的概念，开发Web应用将会变的非常简单。

Spring Boot

Spring和Spring MVC的问题在于需要配置大量的参数。

SpringBoot通过一个自动配置和启动的项来解决这个问题。

# 15. 如何实现 Spring Boot 应用程序的安全性？
为了实现Spring Boot的安全性，可以使用spring-boot-starter-security依赖项，并且必须添加安全配置。

spring-boot-starter-security只需要很少的代码。

配置类必须扩展WebSecurityConfigurerAdapter并覆盖其方法。

# 16. 如何重新加载 Spring Boot 上的更改内容，而无需重启服务？
可以使用DEV工具来实现。通过这种依赖关系，可以节省任何更改，嵌入式tomcat将重新启动。

Spring Boot有一个开发工具（DevTools）模块，它有助于提高开发人员的生产力。

Java开发人员面临的一个主要挑战是将文件更改自动部署到服务器并自动重启服务器。

开发人员可以重新加载Spring Boot上的更改内容，而无需重启服务。消除了每次手动部署更改的需要。

Spring Boot在发布它的第一个版本时没有这个功能。这是开发人员最需要的功能。

DevTools模块完全满足开发人员的需求。该模块将在生产环境中被禁用。它还提供H2数据库控制台以更好地测试应用程序。

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <optional>true</optional>
</dependency>
```
# 17. 如何自定义端口运行 Spring Boot 应用程序？
Spring Boot应用程序自定义端口可以在application.properties中指定端口。

```properties
server.port = 8090
```
# 18. Spring Boot 如何禁用某些自动配置特性？
禁用某些自动配置特性，可以使用@EnableAutoConfiguration注解的exclude属性来指明。

例如，下面的代码段是使DataSourceAutoConfiguration无效：

```java
// other annotations
@EnableAutoConfiguration(exclude = DataSourceAutoConfiguration.class)
public class MyConfiguration { }
```
使用@SpringBootApplication注解

将@EnableAutoConfiguration作为元注解的项，来启用自动化配置，能够使用相同名字的属性来禁用自动化配置：

```java
// other annotations
@SpringBootApplication(exclude = DataSourceAutoConfiguration.class)
public class MyConfiguration { }
```
也可以使用spring.autoconfigure.exclude环境属性来禁用自动化配置。application.properties文件中增加如下配置内容：

```properties
spring.autoconfigure.exclude=org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration
```
# 19. Spring boot 中当 bean 存在时如何置后执行自动配置？
当bean已存在的时候通知自动配置类置后执行，可以使用@ConditionalOnMissingBean注解。这个注解需要注意的属性是：

value：被检查的beans的类型

name：被检查的beans的名字

当将@Bean修饰到方法时，目标类型默认为方法的返回类型：

```java
@Configuration
public class CustomConfiguration {
    @Bean
    @ConditionalOnMissingBean
    public CustomService service() { 
        //
    }
}
```
# 20. Spring Boot 如何编写一个集成测试？
使用Spring应用运行一个集成测试时，需要使用一个ApplicationContext。

为了使开发更简单，SpringBoot为测试提供一个注解@SpringBootTest。这个注解由其classes属性指示的配置类创建一个ApplicationContext。

如果没有配置classes属性，SpringBoot将会搜索主配置类。搜索会从包含测试类的包开始直到找到一个使用@SpringBootApplication或者@SpringBootConfiguration的类为止。

注意如果使用JUnit4，必须使用@RunWith(SpringRunner.class) 来修饰这个测试类。
