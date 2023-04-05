# 1. Spring MVC 控制器注解一般适用什么？可以适用什么替代？
一般用@Controller注解，也可以使用@RestController，@RestController注解相当于@ResponseBody+@Controller，表示是表现层，除此之外，一般不用别的注解代替。

# 2. Spring MVC 中拦截器如何使用？
定义拦截器，实现HandlerInterceptor接口。

接口中提供三个方法

1、preHandle ：进入 Handler方法之前执行，用于身份认证、身份授权，比如身份认证，如果认证通过表示当前用户没有登陆，需要此方法拦截不再向下执行。

2、postHandle：进入Handler方法之后，返回modelAndView之前执行，应用场景从modelAndView出发：将公用的模型数据(比如菜单导航)在这里传到视图，也可以在这里统一指定视图。

3、afterCompletion：执行Handler完成执行此方法，应用场景：统一异常处理，统一日志处理。

拦截器配置

1、针对HandlerMapping配置(不推荐)：springmvc拦截器针对HandlerMapping进行拦截设置，如果在某个HandlerMapping中配置拦截，经过该 HandlerMapping映射成功的handler最终使用该 拦截器。（一般不推荐使用）。

2、类似全局的拦截器：springmvc配置类似全局的拦截器，springmvc框架将配置的类似全局的拦截器注入到每个HandlerMapping中。

# 3. Spring MVC 中如何实现拦截器？
有两种写法，一种是实现HandlerInterceptor接口，另外一种是继承适配器类，接着在接口方法当中，实现处理逻辑；然后在SpringMvc的配置文件中配置拦截器即可。

```xml


<!-- 配置SpringMvc的拦截器 -->
<mvc:interceptors>
<!-- 配置一个拦截器的Bean就可以了 默认是对所有请求都拦截 -->
<bean id="myInterceptor" class="com.yoodb.action.MyHandlerInterceptor"></bean>

    <!-- 只针对部分请求拦截 -->
    <mvc:interceptor>
       <mvc:mapping path="/jingxuan.do" />
       <bean class="com.yoodb.action.MyHandlerInterceptorAdapter" />
    </mvc:interceptor>
</mvc:interceptors>
```
# 4. 说一说 Spring MVC 注解原理？
注解本质是一个继承了Annotation的特殊接口，其具体实现类是JDK动态代理生成的代理类。

通过反射获取注解时，返回的也是Java运行时生成的动态代理对象。

通过代理对象调用自定义注解的方法，会最终调用AnnotationInvocationHandler的invoke方法，该方法会从memberValues这个Map中查询出对应的值，而memberValues的来源是Java常量池。

# 5. Spring MVC 中日期类型参数如何接收？
1、controller控制层

日期类型参数接收时，需要注意Spring MVC在接收日期类型参数时，如不做特殊处理会出现400语法格式错误。

解决办法：定义全局日期处理。

```java
@RequestMapping("/test")
public String test(Date birthday){
System.out.println(birthday);
return "index";
}

```
2、自定义类型转换规则

```java
public class DateConvert implements Converter<String, Date> {

    @Override
    public Date convert(String stringDate) {
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd");
        try {
            return simpleDateFormat.parse(stringDate);
        } catch (ParseException e) {
            e.printStackTrace();
        }
        return null;
    }
}
```
全局日期处理类，其中Convert<T,S>，泛型T：代表客户端提交的参数 String，泛型S：通过convert转换的类型。

3、XML配置文件

首先创建自定义日期转换规则，并创建convertion-Service ，注入dateConvert，最后注册处理器映射器、处理器适配器 ，添加conversion-service属性。

```xml
<mvc:annotation-driven conversion-service="conversionService"/>
<bean id="conversionService" class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
    <property name="converters">
        <set>
            <ref bean="dateConvert"/>
        </set>
    </property>
</bean>
<bean id="dateConvert" class="com.yoodb.DateConvert"/>
```
# 6. 说一说对 RESTful 的理解及项目中的使用？
RESTful一种软件架构风格、设计风格，而不是标准，只是提供了一组设计原则和约束条件。

RESTful主要用于客户端和服务器交互类的软件。

REST指的是一组架构约束条件和原则。

满足这些约束条件和原则的应用程序或设计就是RESTful。

RESTful结构清晰、符合标准、易于理解、扩展方便，所以正得到越来越多网站的采用。

# 7. Spring MVC 常用的注解有哪些？
@RequestMapping：用于处理请求url映射的注解，可用于类或方法上。用于类上，则表示类中的所有响应请求的方法都是以该地址作为父路径。

@RequestBody：注解实现接收http请求的json数据，将json转换为java对象。

@ResponseBody：注解实现将conreoller方法返回对象转化为json对象响应给客户。

# 8. Spring MVC模块的作用是什么？
Spring提供了MVC框架来构建Web应用程序。

Spring可以很容易地与其他MVC框架集成，但是Spring的MVC框架是更好的选择，因为它使用IoC来提供控制器逻辑与业务对象的清晰分离。

使用Spring MVC，可以声明性地将请求参数绑定到业务对象。

# 9. Spring MVC 都有哪些组件？
| 说明        | 组件                            |
|-----------|-------------------------------|
| 前端控制器     | （DispatcherServlet）           |
| 处理器映射器    | （HandlerMapping）              |
| 处理器适配器    | （HandlerAdapter）              |
| 拦截器       | （HandlerInterceptor）          |
| 语言环境处理器   | （LocaleResolver）              |
| 主题解析器     | （ThemeResolver）               |
| 视图解析器     | （ViewResolver）                |
| 文件上传处理器   | （MultipartResolver）           |
| 异常处理器     | （HandlerExceptionResolver）    |
| 数据转换      | （DataBinder）                  |
| 消息转换器     | （HttpMessageConverter）        |
| 请求转视图翻译器  | （RequestToViewNameTranslator） |
| 页面跳转参数管理器 | （FlashMapManager）             |
| 处理程序执行链   | （HandlerExecutionChain）       |

# 10. 简单介绍一下 WebApplicationContext？
WebApplicationContext继承了ApplicationContext并增加了一些WEB应用必备的特有功能，它不同于一般的ApplicationContext ，因为它能处理主题，并找到被关联的servlet。

# 11. Spring 中 BeanFactory 与 ApplicationContext 的作用有哪些？
Spring通过一个配置文件来描述Bean及Bean之间的依赖关系，利用Java的反射功能实例化Bean并建立Bean之间的依赖关系。Sprig的IoC容器在完成这些底层工作的基础上，还提供了Bean实例缓存、生命周期管理、Bean实例代理、事件发布、资源装载等高级服务。

Spring提供了两种类型的IOC容器实现：

>BeanFactory：IOC容器的基本实现 ApplicationContext：提供了更多的高级特性，是BeanFactory的子接口。 Bean工厂（com.springframework.beans.factory.BeanFactory）是Spring框架中最核心的接口，它提供了高级IoC的配置机制。BeanFactory使管理不同类型的Java对象成为可能。而应用上下文（com.springframework.context.ApplicationContext）建立在BeanFactory基础之上，提供了更多面向应用的功能，它提供了国际化支持和框架事件体系，更易于创建实际应用。

一般称BeanFactory为IoC容器，而称ApplicationContext为应用上下文或Spring容器。
