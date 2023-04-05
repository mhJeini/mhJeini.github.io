# 1. 什么是 Spring MVC 框架？
Spring MVC属于Spring FrameWork的后续产品，已经融合在Spring Web Flow中。

Spring框架提供了构建Web应用程序的全功能MVC模块。

使用Spring可插入MVC架构，从而在使用Spring进行WEB开发时，可以选择使用Spring中的Spring MVC框架或集成其他MVC开发框架，如Struts1（已基本淘汰），Struts2（老项目还在使用或已重构）等。

通过策略接口，Spring框架是高度可配置的且包含多种视图技术，如JavaServer Pages（JSP）技术、Velocity、Tiles、iText和POI等。

Spring MVC 框架并不清楚或限制使用哪种视图，所以不会强迫开发者只使用JSP技术。

Spring MVC分离了控制器、模型对象、过滤器以及处理程序对象的角色，这种分离让它们更容易进行定制。

# 2. Spring MVC 中常用的注解包含哪些？
@EnableWebMvc 在配置类中开启Web MVC的配置支持，如一些ViewResolver或者MessageConverter等，若无此句，重写WebMvcConfigurerAdapter方法（用于对SpringMVC的配置）。

@Controller 声明该类为SpringMVC中的Controller

@RequestMapping 用于映射Web请求，包括访问路径和参数（类或方法上）

@ResponseBody 支持将返回值放在response内，而不是一个页面，通常用户返回json数据（返回值旁或方法上）

@RequestBody 允许request的参数在request体中，而不是在直接连接在地址后面。（放在参数前）

@PathVariable 用于接收路径参数，比如@RequestMapping(“/hello/{name}”)申明的路径，将注解放在参数中前，即可获取该值，通常作为Restful的接口实现方法。

@RestController 该注解为一个组合注解，相当于@Controller和@ResponseBody的组合，注解在类上，意味着，该Controller的所有方法都默认加上了@ResponseBody。

@ControllerAdvice 通过该注解，我们可以将对于控制器的全局配置放置在同一个位置，注解了@Controller的类的方法可使用@ExceptionHandler、@InitBinder、@ModelAttribute注解到方法上，

这对所有注解了 @RequestMapping的控制器内的方法有效。

@ExceptionHandler 用于全局处理控制器里的异常

@InitBinder 用来设置WebDataBinder，WebDataBinder用来自动绑定前台请求参数到Model中。

@ModelAttribute 本来的作用是绑定键值对到Model里，在@ControllerAdvice中是让全局的@RequestMapping都能获得在此处设置的键值对。

# 3. Spring MVC 执行流程是什么？
1）客户端发送请求到前端控制器DispatcherServlet。

2）DispatcherServlet收到请求调用HandlerMapping处理器映射器。

3）处理器映射器找到具体的处理器，根据xml配置、注解进行查找，生成处理器对象及处理器拦截器，并返回给DispatcherServlet。

4）DispatcherServlet调用HandlerAdapter处理器适配器。

5）HandlerAdapter经过适配调用具体的后端控制器Controller。

6）Controller执行完成返回ModelAndView。

7）HandlerAdapter将controller执行结果ModelAndView返回给DispatcherServlet。

8）DispatcherServlet将ModelAndView传给ViewReslover视图解析器。

9）ViewReslover解析后返回具体View。

10）DispatcherServlet根据View进行渲染视图，将模型数据填充至视图中。

11）DispatcherServlet响应客户端。

**组件说明**

DispatcherServlet：作为前端控制器，整个流程控制的中心，控制其它组件执行，统一调度，降低组件之间的耦合性，提高每个组件的扩展性。

HandlerMapping：通过扩展处理器映射器实现不同的映射方式，例如：配置文件方式，实现接口方式，注解方式等。

HandlAdapter：通过扩展处理器适配器，支持更多类型的处理器。

ViewResolver：通过扩展视图解析器，支持更多类型的视图解析，例如：jsp、freemarker、pdf、excel等。

# 4. Spring MVC 如何解决请求中文乱码问题？
解决GET请求中文乱码问题

方式一：每次请求前使用encodeURI对URL进行编码。

方式二：在应用服务器上配置URL编码格式，在Tomcat配置文件server.xml中增加URIEncoding="UTF-8"，然后重启Tomcat即可。

```xml
<ConnectorURIEncoding="UTF-8"
port="8080"  maxHttpHeaderSize="8192"  maxThreads="150"
minSpareThreads="25"  maxSpareThreads="75"connectionTimeout="20000" 		
disableUploadTimeout="true" URIEncoding="UTF-8" />
```

解决POST请求中文乱码问题

方式一：在request解析数据时设置编码格式：

```java
request.setCharacterEncoding("UTF-8");
```

方式二：使用Spring提供的编码过滤器

在web.xml文件中配置字符编码过滤器，增加如下配置：

```xml
<!--编码过滤器-->                                                                                                                
<filter>                                                                
	<filter-name>encodingFilter</filter-name>                           
	<filter-class>                                                      
		org.springframework.web.filter.CharacterEncodingFilter          
	</filter-class>                                                     
	<!-- 开启异步支持-->                                               
	<async-supported>true</async-supported>                             
	<init-param>                                                        
		<param-name>encoding</param-name>                               
		<param-value>utf-8</param-value>                                
	</init-param>                                                       
</filter>                                                               
<filter-mapping>                                                        
	<filter-name>encodingFilter</filter-name>                           
	<url-pattern>/*</url-pattern>                                       
</filter-mapping>     
```
该过滤器的作用就是强制所有请求和响应设置编码格式：

```java
request.setCharacterEncoding("UTF-8");
response.setCharacterEncoding("UTF-8");
```
# 5. Spring MVC 请求转发和重定向有什么区别？
请求转发是浏览器发出一次请求，获取一次响应，而重定向是浏览器发出2次请求，获取2次请求。

请求转发是浏览器地址栏未发生变化，是第1次发出的请求，而重定向是浏览器地址栏发生变化，是第2次发出的请求。

请求转发称为服务器内跳转，重定向称为服务器外跳转。

请求转发是可以获取到用户提交请求中的数据，而重定向是不可以获取到用户提交请求中的数据，但可以获取到第2次由浏览器自动发出的请求中携带的数据。

请求转发是可以将请求转发到WEB-INF目录下的（内部）资源，而重定向是不可以将请求转发到WEB-INF目录下的资源。

# 6. Spring MVC 中系统是如何分层？
表现层（UI）：数据的展现、操作页面、请求转发。

业务层（服务层）：封装业务处理逻辑。

持久层（数据访问层）：封装数据访问逻辑。

各层之间的关系：MVC是一种表现层的架构，表现层通过接口调用业务层，业务层通过接口调用持久层，当下一层发生改变，不影响上一层的数据。

# 7. 如何开启注解处理器和适配器？
在配置文件中（一般命名为springmvc.xml 文件）通过开启配置：

```xml
<mvc:annotation-driven>
```
来实现注解处理器和适配器的开启。

# 8. Spring MVC 如何设置重定向和转发？
在返回值前面加“forward:”参数，可以实现转发。

```java
"forward:getName.do?name=Java精选"
```
在返回值前面加“redirect:”参数，可以实现重定向。

```java
"redirect:https://blog.yoodb.com"
```

# 9. Spring MVC 中函数的返回值是什么？

Spring MVC的返回值可以有很多类型，如String、ModelAndView等，但事一般使用String比较友好。

# 10. Spring MVC 中 @RequestMapping 注解有什么属性？

RequestMapping是一个用来处理请求地址映射的注解，可用于类或方法上。用于类上，表示类中的所有响应请求的方法都是以该地址作为父路径。

**RequestMapping注解有六个属性**

value

指定请求的实际地址，指定的地址可以是URI Template 模式（后面将会说明）；

method

指定请求的method类型， GET、POST、PUT、DELETE等；

consumes

指定处理请求的提交内容类型（Content-Type），例如application/json, text/html;

produces

指定返回的内容类型，仅当request请求头中的(Accept)类型中包含该指定类型才返回；

params

指定request中必须包含某些参数值是，才让该方法处理。

headers

指定request中必须包含某些指定的header值，才能让该方法处理请求。


# 11. Spring MVC 控制器是单例的吗?

默认情况下是单例模式，在多线程进行访问时存在线程安全的问题。

解决方法可以在控制器中不要写成员变量，这是因为单例模式下定义成员变量是线程不安全的。

通过@Scope(value = ConfigurableBeanFactory.SCOPE_PROTOTYPE)或@Scope("prototype")可以实现多例模式。但是不建议使用同步，因为会影响性能。

使用单例模式是为了性能，无需频繁进行初始化操作，同时也没有必要使用多例模式。

# 12. RequestMethod 可以同时支持POST和GET请求访问吗？


POST请求访问

```java
@RequestMapping(value="/wx/getUserById",method = RequestMethod.POST)
```
GET请求访问

```java
@RequestMapping(value="/wx/getUserById/{userId}",method = RequestMethod.GET)
```
同时支持POST和GET请求访问

```java
@RequestMapping(value="/subscribe/getSubscribeList/{customerCode}/{sid}/{userId}")
```
RequestMethod常用的参数

GET（SELECT）：从服务器查询，在服务器通过请求参数区分查询的方式。    POST（CREATE）：在服务器新建一个资源，调用insert操作。    PUT（UPDATE）：在服务器更新资源，调用update操作。

DELETE（DELETE）：从服务器删除资源，调用delete语句。

# 13. Spring MVC 中文件上传有哪些需要注意事项？
Spring MVC提供了两种上传方式配置

基于commons-fileupload.jar

```java
org.springframework.web.multipart.commons.CommonsMultipartResolver
```
基于servlet3.0+

```java
org.springframework.web.multipart.support.StandardServletMultipartResolver
```
1、页面form中提交方式enctype="multipart/form-data"的数据时，需要springmvc对multipart类型的数据进行解析。

2、在springmvc.xml中配置multipart类型解析器。

3、方法中使用MultipartFile attach实现单个文件上传或者使用MultipartFile[] attachs实现多个文件上传。

# 14. Spring MVC 和 Struts2 有哪些区别？
Spring MVC的入口是一个servlet即前端控制器（DispatchServlet），而Struts2入口是一个filter过虑器（StrutsPrepareAndExecuteFilter）。

Spring MVC是基于方法开发（一个url对应一个方法），请求参数传递到方法的形参，可以设计为单例或多例（建议单例），Struts2是基于类开发，传递参数是通过类的属性，只能设计为多例。

Struts采用值栈存储请求和响应的数据，通过OGNL存取数据，而Spring MVC通过参数解析器将request请求内容解析，并给方法形参赋值，将数据和视图封装成ModelAndView对象，最后将ModelAndView中的模型数据通过reques域传输到页面。其中Jsp视图解析器默认使用jstl。

# 15. Spring MVC 中 @RequestMapping 注解用在类上有什么作用？
@RequestMapping注解是一个用来处理请求地址映射的注解，可用于类或方法上。

用于类上，则表示类中的所有响应请求的方法都是以该地址作为父路径。

比如

```java
@Controller
@RequestMapping("/test/")
public class TestController{

}
```
启动的是本地服务，默认端口是8080，通过浏览器访问的路径就是如下地址：

```sh
http://localhost:8080/test/
```
# 16. Spring MVC 中 @PathVariable 和 @RequestParam 有哪些区别？
@RequestParam与@PathVariable为spring的注解，都可以用于在Controller层接收前端传递的数据，不过两者的应用场景不同。

@PathVariable主要用于接收http://host:port/path/{参数值}数据。

@PathVariable请求接口时，URL是 http://www.yoodb.com/user/getUserById/2

@RequestParam主要用于接收http://host:port/path?参数名=值数据值。

@RequestParam请求接口时，URL是 http://www.yoodb.com/user/getUserById?userId=1

@PathVariable用法

```java
@RequestMapping(value = "/yoodb/{id}",method = RequestMethod.DELETE)
public Result getUserById(@PathVariable("id")String id) 
```
@RequestParam用法

```java
@RequestMapping(value = "/yoodb",method = RequestMethod.POST)
public Result getUserById(@RequestParam(value="id",required=false,defaultValue="0")String id)
```
注意@RequestParam用法当中的参数

value参数表示接收数据的名称。 required参数表示接收的参数值是否必须，默认为true，既默认参数必须不为空，当传递过来的参数可能为空的时候可以设置required=false。 defaultValue参数表示如果此次参数未空则为其设置一个默认值。微信小程“Java精选面试题”，内涵 3000+ 道面试题。

@PathVariable主要应用场景：不少应用为了实现RestFul风格，采用@PathVariable方式。

@RequestParam应用场景：这种方式应用非常广，比如CSDN、博客园、开源中国等等。

# 17. Spring MVC 中如何进行异常处理？
局部异常处理

局部异常处理是指当类中发生异常时，由方法来处理，该方法的参数类型为Exception，而Exception是所有异常的父类，故由该参数来接收异常对象。

步骤说明

1）在controller类中定义处理异常的方法，添加注解@ExceptionHandler，方法的参数类型为Exception，并通过getMessage()方法获取异常信息，再将异常信息保存，最后跳转到错误页面，使用mv.setViewName("error")，这里通过ModelAndView将异常信息保存到request中。

2）创建error.jsp页面，在page中添加isErrorPage="true"，表示该页面为错误页面，再从request中获取错误信息，并显示到页面。

定义全局异常类

1）创建异常类，添加注解@ControllerAdvice表示当发生异常时由该异常类来处理，全局的异常类处理异常。

在异常类中定义处理异常的方法，该方法的定义与定义局部的异常处理方法一致。

# 18. Spring MVC 如何将 Model 中数据存放到 Session？
在Controller类上增加@SessionAttributes注解，使得在调用该Controller时，将Model中的数据存入Session中，实例代码如下：

```java 
@Controller 
@RequestMapping("/") 
@SessionAttributes("isAdmin") 
public class IndexController extends BasicController { 
    //.... 
}
```


# 19. Spring MVC 如何与 Ajax 相互调用？

通过Jackson框架就可以把Java里面的对象直接转化成Js可以识别的Json对象。具体步骤如下：

1、加入Jackson.jar

2、在配置文件中配置json的映射

3、在接受Ajax方法里面可以直接返回Object、List等，但方法前面要加上@ResponseBody注解。

# 20. Spring MVC 中如何拦截 get 方式请求？

@RequestMapping注解中加上method=RequestMethod.GET参数就可以实现拦截get方式请求。

```java
@RequestMapping("jingxuan/login")
public String login(){

}
```
