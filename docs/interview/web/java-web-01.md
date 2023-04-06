# 1. 什么是 Servlet？
Servlet是用Java编写的服务器端程序, 其主要功能在于交互式地浏览和修改数据，生成动态Web内容。

狭义的Servlet是指Java语言实现的一个接口，广义的Servlet是指任何实现了这个Servlet接口的类，一般情况下，我们一般将Servlet理解为后者。

# 2. 为什么要使用 Servlet？
Servlet可以很好地替代公共网关接口(Common Gateway Interface，CGI)脚本。通常CGI脚本是用Perl或者C语言编写的，它们总是和特定的服务器平台紧密相关。而Servlet是用Java编写的，所以它们一开始就是平台无关的。这样，Java编写一次就可以在任何平台运行(write once,run anywhere)的承诺就同样可以在服务器上实现了。

Servlet还有一些CGI脚本所不具备的独特优点：

Servlet是持久的。Servlet只需Web服务器加载一次，而且可以在不同请求之间保持服务(例如一次数据库连接)。与之相反，CGI脚本是短暂的、瞬态的。每一次对CGI脚本的请求，都会使Web服务器加载并执行该脚本。一旦这个CGI脚本运行结束，它就会被从内存中清除，然后将结果返回到客户端。CGI脚本的每一次使用，都会造成程序初始化过程(例如连接数据库)的重复执行。

Servlet是与平台无关的。如前所述，Servlet是用Java编写的，它自然也继承了Java的平台无关性。

Servlet是可扩展的。由于Servlet是用Java编写的，它就具备了Java所能带来的所有优点。Java是健壮的、面向对象的编程语言，它很容易扩展以适应你的需求。Servlet自然也具备了这些特征。

Servlet是安全的。从外界调用一个Servlet的惟一方法就是通过Web服务器。这提供了高水平的安全性保障，尤其是在你的Web服务器有防火墙保护的时候。

Setvlet可以在多种多样的客户机上使用。由于Servlet是用Java编写的，所以你可以很方便地在HTML中使用它们，就像你使用applet一样。

# 3. Servlet接口中有哪些方法？
Servlet接口定义了5个方法：

void init(ServletConfig config) throws ServletException

void service(ServletRequest req, ServletResponse resp) throws ServletException, java.io.IOException

void destory()

java.lang.String getServletInfo()

ServletConfig getServletConfig()

# 4. Servlet 是线程安全的吗？
Servlet不是线程安全的。

多线程并发的读写会导致数据不同步的问题。

解决的办法是尽量不要定义name属性，而是要把name变量分别定义在doGet()和doPost()方法内。

虽然使用synchronized(name){}语句块可以解决问题，但是会造成线程的等待，不是很科学的办法。

注意的是多线程的并发的读写Servlet类属性会导致数据不同步。但是如果只是并发地读取属性而不写入，则不存在数据不同步的问题。因此Servlet里的只读属性最好定义为final类型的。

# 5. Servlet 是单例还是多例？
1、servlet是单例的，严格地说是一个ServletMapping对应一个单例实例(如果一个Servlet被映射了两个URL地址，会生成两个实例)。早期的CGI模式是原型式的，例如同时并发2000次请求一个Servlet，如果不是单例的，内存瞬间要创建2000个对象，同时为了线程安全还得阻塞对方线程，其性能非常之差。

2、要维护Servlet线程安全有很多办法，通常是使用同步块(或方法)来保护共享数据，其次可以volatile、Lock一些锁机制，还可以使用ThreadLocal来打通安全通道，另外还有原子操作也是用来保护数据安全，有非常多的选择。

# 6. Servlet 和 JSP 有什么区别？
Servlet是服务器端的程序，动态生成html页面发送到客户端，但是这样程序里会有很多out.println()。

java与html语言混在一起很乱，所以后来sun公司推出了JSP。

其实JSP就是Servlet，每次运行的时候JSP都首先被编译成servlet文件，然后再被编译成.class文件运行。

有了jsp，在MVC项目中servlet不再负责动态生成页面，转而去负责控制程序逻辑的作用，控制jsp与javabean之间的流转。

# 7. 如何实现自定义一个 Servlet？
extends HttpServlet并覆盖doPost或doGet方法。

在web.xml中进行部署。

# 8. 编写 Servlet 需要继承什么类？
编写Servlet需要继承HttpServlet类。

HttpServlet抽象类是继承自GenericServlet，而GenericServlet是实现了Servlet、ServletConfig、Serializable接口。

Servlet接口及其抽象类：

```java
public interface Servlet {

public void init(ServletConfig config) throws ServletException;

public ServletConfig getServletConfig();

public void service(ServletRequest req, ServletResponse res )throws ServletException, IOException;

    public String getServletInfo();

    public void destroy();

}

public abstract class GenericServlet implements Servlet, ServletConfig,java.io.Serializable {}

public abstract class HttpServlet extends GenericServlet {}
```

# 9. doGet 和 doPost 方法的两个参数是什么？
doGet()和doPost()方法的两个参数为HttpServletRequest和HttpServletResponse对象。

HttpServletRequest：封装了与请求相关的信息。

HttpServletResponse：封装了与响应相关的信息。

# 10. 什么情况下调用 doGet() 和 doPost()？
默认情况是调用doGet()方法，JSP页面中的Form表单的method属性设置为post的时候，调用的为doPost()方法；为get的时候，调用deGet()方法。

# 11. 转发（Forward）和重定向（Redirect）有什么区别？
转发是服务器行为，重定向是客户端行为。

**转发（Forword）**

通过RequestDispatcher对象的forward（HttpServletRequest request,HttpServletResponse response）方法实现的。RequestDispatcher可以通过HttpServletRequest 的getRequestDispatcher()方法获得。例如下面的代码就是跳转到login_success.jsp页面。

```java
request.getRequestDispatcher("login_success.jsp").forward(request, response);
```

**重定向（Redirect）**

是利用服务器返回的状态吗来实现的。客户端浏览器请求服务器的时候，服务器会返回一个状态码。服务器通过HttpServletRequestResponse的setStatus(int status)方法设置状态码。如果服务器返回301或者302，则浏览器会到新的网址重新请求该资源。

**区别**

1、从地址栏显示来说

forward是服务器请求资源，服务器直接访问目标地址的URL，把那个URL的响应内容读取过来，然后把这些内容再发给浏览器。浏览器根本不知道服务器发送的内容从哪里来的，所以它的地址栏还是原来的地址。

redirect是服务端根据逻辑，发送一个状态码,告诉浏览器重新去请求那个地址，所以地址栏显示的是新的URL。

2、从数据共享来说

forward：转发页面和转发到的页面可以共享request里面的数据。 redirect：不能共享数据。

3、从运用地方来说

forward：一般用于用户登陆的时候，根据角色转发到相应的模块。 redirect：一般用于用户注销登陆时返回主页面和跳转到其它的网站等。

4、从效率来说

forward：高。 redirect：低。

# 12. Servlet 中如何实现自动刷新（Refresh）？
自动刷新不仅可以实现一段时间之后自动跳转到另一个页面，还可以实现一段时间之后自动刷新本页面。Servlet中通过HttpServletResponse对象设置Header属性实现自动刷新例如：

```java
Response.setHeader("Refresh","1000;URL=http://localhost:8080/servlet/example.htm");
```

其中1000为时间，单位为毫秒。URL指定就是要跳转的页面，如果设置自己的路径，就会实现没过一秒自动刷新本页面一次。

# 13. Java 中 Request 对象都有哪些方法？
| 方法名                              | 说明 |
|----------------------------------| -----------------------------|
| setAttribute(String name,Object) | 设置名字为name的request的参数值 |
| getAttribute(String name)        | 返回由name指定的属性值 |
| getAttributeNames()              | 返回request对象所有属性的名字集合，结果是一个枚举的实例 |
| getCookies()                     | 返回客户端的所有Cookie对象，结果是一个Cookie数组 |
| getCharacterEncoding()           | 返回请求中的字符编码方式 |
| getContentLength()               | 返回请求的Body的长度 getHeader(String name)：获得HTTP协议定义的文件头信息 |
| getHeaders(String name)          | 返回指定名字的request Header的所有值，结果是一个枚举的实例   getHeaderNames()：返回所以request Header的名字，结果是一个枚举的实例 |
| getInputStream()                 | 返回请求的输入流，用于获得请求中的数据 |
| getMethod()                      | 获得客户端向服务器端传送数据的方法 |
| getParameter(String name)        | 获得客户端传送给服务器端的有name指定的参数值 |
| getParameterNames()              | 获得客户端传送给服务器端的所有参数的名字，结果是一个枚举的实例   getParameterValues(String name)：获得有name指定的参数的所有值 |
| getProtocol()                    | 获取客户端向服务器端传送数据所依据的协议名称 |
| getQueryString()                 | 获得查询字符串 |
| getRequestURI()                  | 获取发出请求字符串的客户端地址 |
| getRemoteAddr()                  | 获取客户端的IP地址 |
| getRemoteHost()                  | 获取客户端的名字 |
| getSession([Boolean create])     | 返回和请求相关Session |
| getServerName()                  | 获取服务器的名字 |
| getServletPath()                 | 获取客户端所请求的脚本文件的路径 |
| getServerPort()                  | 获取服务器的端口号 |
| removeAttribute(String name)     | 删除请求中的一个属性 |

# 14. JSP 内置对象都有什么作用？
1）Request：本质上就是HttpServletRequest，包含用户端请求的信息，就是请求对象

2）Response：本质上就是HttpServletResponse，包含服务器传回客户端的响应信息，就是响应对象

3）Session：是HttpSession，是一个会话对象，主要用于保存状态

4）Application：是servletContext，指的的整个web应用

5）Page：指整个jsp页面，类似this伪对象

6）PageContext：主要用于管理整个jsp页面

7）Exception：异常对象，jsp页面上的异常都会封装在这里面

8）Config：本质上就是servletConfig对象

9）Out：主要用于向客户端输出数据

# 15. Servlet API 中有哪些主要包？
**javax.servlet.; javax.servlet.http.;**

# 16. get 和 post 请求有什么区别？
1、get一般用于从服务器上获取数据，而post一般用来向服务器传送数据；

2、get将表单中数据按照variable=value的形式，添加到action所指向的URL后面，并且两者用"？"连接，变量之间用"&"连接；而post是将表单中的数据放在form的数据体中，按照变量与值对应的方式，传递到action所指定的URL。

3、get是不安全的，因为在传输过程中，数据是被放在请求的URL中，而post的所有操作对用户来说都是不可见的。

4、get传输的数据量小，这主要应为受url长度限制，而post可以传输大量的数据，所有上传文件只能用post提交。

5、get限制form表单的数据集必须为ASCII字符，而post支持整个IS01 0646字符集。

6、get是form表单的默认方法。

# 17. 编写 Servlet 通常需要重写哪两个方法？
doGet()或doPost()方法。

```java
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class JingXuanServlet extends HttpServlet {

	private static final long serialVersionUID = 1L;

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    	// TODO Auto-generated method stub
    	super.doGet(req, resp);
    }
    
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    	// TODO Auto-generated method stub
    	super.doPost(req, resp);
    }

}
```

# 18. Servlet 执行时一般实现哪几个方法？
init()方法在servlet的生命周期中仅执行一次，在服务器装载servlet时执行。缺省的init()方法通常是符合要求的，不过也可以根据需要进行 override，比如管理服务器端资源，一次性装入GIF图像，初始化数据库连接等，缺省的inti()方法设置了servlet的初始化参数，并用它的ServeltConfig对象参数来启动配置，所以覆盖init()方法时，应调用super.init()以确保仍然执行这些任务。

```java
public void init(ServletConfig config)
```

getServletConfig()方法返回一个servletConfig对象，该对象用来返回初始化参数和servletContext。servletContext接口提供有关servlet的环境信息。

```java
public ServletConfig getServletConfig()
```

getServletInfo()方法提供有关servlet的信息，如作者，版本，版权。

```java
public String getServletInfo()
```

service()方法是servlet的核心，在调用service()方法之前，应确保已完成init()方法。对于HttpServlet，每当客户请求一个HttpServlet对象，该对象的service()方法就要被调用，HttpServlet缺省的service()方法的服务功能就是调用与HTTP请求的方法相应的do功能，doPost()和doGet()，所以对于HttpServlet，一般都是重写doPost()和doGet()方法。

```java
public void service(ServletRequest request,ServletResponse response)
```

destroy()方法在servlet的生命周期中也仅执行一次，即在服务器停止卸载servlet时执行，把servlet作为服务器进程的一部分关闭。缺省的destroy()方法通常是符合要求的，但也可以override，比如在卸载servlet时将统计数字保存在文件中，或是关闭数据库连接。

```java
public void destroy()
```

# 19. Servlet 如何获取传递的参数信息？
HttpServletRequest对象的getParameter()方法和getParameterValues()方法。

getParameter()方法用于获取单值表单元素的值，而getParameterValues()方法用于获取多值的情况，典型的复选框。

getParameter()方法返回的是一个字符串，而getParameterValues()方法返回的是字符串数组。

如果参数指定的表单元素不存在，返回null。

# 20. Servlet 中如何返回响应信息？
设置响应内容的类型：response.setContentType(“text/html;charset=gb2312”);

获取输出流对象：PrintWriter out = response.getWriter();

输出信息：通过out的println()方法。
