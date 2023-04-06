# 1. Java 中如何获取 ServletContext 实例？
1、javax.servlet.Filter中直接获取

```java
ServletContext context = config.getServletContext();
```

2、HttpServlet中直接获取

```java
this.getServletContext()
```

3、在其他方法中通过HttpRequest获得

```java
request.getSession().getServletContext();
```

# 2. Java 中 ServletContext 的生命周期？
创建：web应用被加载到服务器或服务器开启。

销毁：web应用被移除或服务器关闭。

# 3. Java 中 ServletContext 应用场景有哪些？
网站计数器、用户在线人数、简单的聊天系统等，如果涉及到不同用户共享数据，数据又不大，又不希望写入数据库中，都可以考虑使用ServletContext。

# 4. 如何配置 Servlet 初始化参数？
在web.xml中该Servlet的定义标记中，比如：

```xml
<servlet>
    <servlet-name>JingXuanServlet</servlet-name>
    <servlet-class>com.jx.servlet.basic.JingXuanServlet</servlet-class>
    <init-param>
        <param-name>user</param-name>
        <param-value>jingxuan</param-value>
    </init-param>
    <init-param>
        <param-name>blog</param-name>
        <param-value>http://blog.yoodb.com</param-value>
    </init-param>
</servlet>
```

两个初始化参数user和blog它们的值分别为jingxuan和http://blog.yoodb.com，这样以后需要修改用户名和博客的地址的时候，不需要修改Servlet代码，只需修改配置文件即可。

# 5. 如何读取 Servlet 初始化参数？
ServletConfig中定义了如下的方法用来读取初始化参数的信息：

```java
public String getInitParameter(String name)
```

参数：初始化参数的名称。

返回：初始化参数的值，如果没有配置，返回null。

# 6. Serlvet 中 init() 方法执行次数是多少？
在Servlet的生命周期中，该方法执行一次。

# 7. Servlet 和 GCI 有什么区别？
1、Servlet是基于Java编写的，处于服务器进程中，他能够通过多线程方式运行service()方法，一个实例可以服务于多个请求，而且一般不会销毁；

2、CGI对每个请求都产生新的进程，服务完成后销毁，所以效率上低于Servlet。

# 8. Java 中有哪些会话跟踪技术作用域？
**page域**

数据在一个页面范围内有效，通过pageContext对象访问

**request域**

数据在一个服务器请求范围内有效，通过request对象访问

**session域**

数据在一次会话范围内容有效，通过session对象访问

**application域**

数据在一个应用服务器范围内有效，通过application对象访问

# 9. JSP 中 7 个动作指令和作用？
jsp:forward-执行页面转向，把请求转发到下一个页面；

jsp:param-用于传递参数，必须与其他支持参数的标签一起使用；

jsp:include-用于动态引入一个JSP页面；

jsp:plugin-用于下载JavaBean或Applet到客户端执行；

jsp:useBean-寻求或者实例化一个JavaBean；

jsp:setProperty-设置JavaBean的属性值；

jsp:getProperty-获取JavaBean的属性值。

# 10. Java 中自定义标签要继承哪个类？
继承TagSupport或者BodyTagSupport。

两者的差别是前者适用于没有主体的标签，而后者适用于有主体的标签。

继承TagSupport，可以实现doStartTag和doEndTag两个方法实现Tag的功能

继承BodyTagSupport，可以实现doAfterBody这个方法。

# 11. JSP 和 HTML之间有什么关系？
1、JSP与Java Servlet一样，是在服务器端执行的，通常返回该客户端的就是一个HTML文本，因此客户端只要有浏览器就能浏览。

2、在大多数Browser/Server结构的Web应用中，浏览器直接通过HTML或者JSP的形式与用户交互，响应用户的请求。

3、JSP在服务器上执行，并将执行结果输出到客户端浏览器，我们可以说基本上与浏览器无关。

# 12. JSP 中 <%…%> 和 <%!…%> 有什么区别？
<%…%>用于在JSP页面中嵌入Java脚本

<%!…%>用于在JSP页面中申明变量或方法，可以在该页面中的<%…%>脚本中调用，声明的变量相当于Servlet中的定义的成员变量。

# 13. JSP 中如何解决中文乱码问题？
1、JSP页面显示乱码。

```html
<%@ page contentType="text/html; charset=gb2312"%>
```

2、表单提交中文时出现乱码。

```html
request.seCharacterEncoding("gb2312")
```

对请求进行统一编码

3、数据库连接出现乱码

在数据库的数据库URL中加上，增加配置如下：

```html
useUnicode=true&characterEncoding=GBK
```

4、通过过滤器完成。

5、在server.xml中的设置编码格式。

# 14. JSP 中隐含对象都有哪些？
JSP隐含对象：没有声明就可以使用的对象。JSP有9个隐含对象。

```shell
request
response
session
application
out
pagecontext
config
page
exception
```

request：HttpServletRequest 的一个实例，代表了客户端的请求信息，主要用于接受通过HTTP协议传送到服务器的数据。（包括头信息、系统信息、请求方式以及请求参数等）作用域为一次请求。

response：HttpServletResponse的一个实例，代表是对客户端的响应，主要是将JSP容器处理过的对象传回到客户端。

session：HttpSession的一个实例，是由服务器自动创建的与用户请求相关的对象，作用域为一次会话（浏览器打开直到关闭称为一次会话）。

application：ServletContext的一个实例（表示当前web应用），开始于服务器启动，直到服务器关闭，作用域为当前web应用。

out：jspWriter的一个实例（用于浏览器输出数据）。

pagecontext：作用是取得任何范围的参数（页面的上下文）作用域为当前JSP =页面 config：ServletConfig 的一个实例（主要作用是取得服务器的配置信息）。

page：page 对象代表JSP本身，只有在JSP页面内才是合法的。

exception：显示异常信息。

# 15. JSP 中内置对象映射表？
| 对象名         | 	类型                               | 	作用域         |
|-------------|-----------------------------------|--------------|
| request     | 	javax.servlet.ServletRequest的子类  | 	Request     |
| response    | 	javax.servlet.ServletResponse的子类 | 	Page        |
| session     | 	javax.servlet.http.HttpSession   | 	Session     |
| application | 	javax.servlet.ServletContext     | 	Application |
| out         | 	javax.servlet.jsp.JspWriter      | 	Page        |
| config      | 	javax.servlet.ServletConfig      | 	Page        |
| page        | 	java.lang.Object                 | 	Page        |
| pageContext | 	javax.servlet.jsp.PageContext    | 	Page        |
| exception   | 	java.langThrowable               | 	Page        |
# 16. JSP 中 config 对象有什么作用？
config对象的主要作用是取得服务器的配置信息。

通过pageConext对象的getServletConfig()方法可以获取一个config对象。

当一个Servlet初始化时，容器把某些信息通过config对象传递给这个Servlet。

开发者可以在web.xml文件中为应用程序环境中的Servlet程序和JSP页面提供初始化参数。

# 17. JSP 中 pageContext 对象有什么作用？
pageContext对象的作用是取得任何范围的参数，通过它可以获取JSP页面的out、request、reponse、session、application等对象。

pageContext对象的创建和初始化都是由容器来完成的，在JSP页面中可以直接使用pageContext对象。

page对象代表JSP本身，只有在JSP页面内才是合法的。page隐含对象本质上包含当前Servlet接口引用的变量，类似于Java编程中的this指针。

# 18. JSP 中 request 对象有什么作用？
request对象代表了客户端的请求信息，主要用于接受通过HTTP协议传送到服务器的数据。（包括头信息、系统信息、请求方式以及请求参数等）。request对象的作用域为一次请求。

当Request对象获取客户提交的汉字字符时，会出现乱码问题，必须进行特殊处理。首先，将获取的字符串用ISO-8859－1进行编码，并将编码存发岛一个字节数组中，然后再将这个数组转化为字符串对象。

**Request常用方法**

| 方法名                              | 	           说明                            | 
|----------------------------------|-------------------------------------------|
| getParameter(String strTextName) | 获取表单提交的信息。                                |
| getProtocol()                    | 获取客户使用的协议。                                |
| getServletPath()                 | 获取客户提交信息的页面。                              |
| getMethod()                      | 获取客户提交信息的方式。                              |
| getHeader()                      | 获取HTTP头文件中的accept,accept-encoding和Host的值。 |
| getRermoteAddr()                 | 获取客户的IP地址。                                |
| getRemoteHost()                  | 获取客户机的名称。                                 |
| getServerName()                  | 获取服务器名称。                                  |
| getServerPort()                  | 获取服务器的端口号。                                |
| getParameterNames()              | 获取客户端提交的所有参数的名字。                          |

# 19. JSP 中 response 对象有什么作用？
response代表的是对客户端的响应，主要是将JSP容器处理过的对象传回到客户端。

response对象也具有作用域，它只在JSP页面内有效。

具有动态响应contentType属性，当一个用户访问一个JSP页面时，如果该页面用page指令设置页面的contentType属性是text/html，那么JSP引擎将按照这个属性值做出反应。

如果要动态改变这换个属性值来响应客户，就需要使用Response对象的setContentType(String s)方法来改变contentType的属性值。

```java
response.setContentType(String s);
```

参数s可取text/html,application/x-msexcel,application/msword等。

在某些情况下，当响应客户时，需要将客户重新引导至另一个页面，可以使用Response的sendRedirect(URL)方法实现客户的重定向。

```java
response.sendRedirect("index.jsp");
```

# 20. JSP 中 session 对象有什么作用？
Session对象是一个JSP内置对象，它在第一个JSP页面被装载时自动创建，完成会话期管理。从一个客户打开浏览器并连接到服务器开始，到客户关闭浏览器离开这个服务器结束，被称为一个会话。当一个客户访问一个服务器时，可能会在这个服务器的几个页面之间切换，服务器应当通过某种办法知道这是一个客户，就需要Session对象。

当一个客户首次访问服务器上的一个JSP页面时，JSP引擎产生一个Session对象，同时分配一个String类型的ID号，JSP引擎同时将这换个ID号发送到客户端，存放在Cookie中，这样Session对象，直到客户关闭浏览器后，服务器端该客户的Session对象才取消，并且和客户的会话对应关系消失。当客户重新打开浏览器再连接到该服务器时，服务器为该客户再创建一个新的Session对象。

session 对象是由服务器自动创建的与用户请求相关的对象。服务器为每个用户都生成一个session对象，用于保存该用户的信息，跟踪用户的操作状态。

session对象内部使用Map类来保存数据，因此保存数据的格式为 “Key/value”。 session对象的value可以使复杂的对象类型，而不仅仅局限于字符串类型。

public String getId()：获取Session对象编号。

public void setAttribute(String key,Object obj)：将参数Object指定的对象obj添加到Session对象中，并为添加的对象指定一个索引关键字。

public Object getAttribute(String key)：获取Session对象中含有关键字的对象。

public Boolean isNew()：判断是否是一个新的客户。
