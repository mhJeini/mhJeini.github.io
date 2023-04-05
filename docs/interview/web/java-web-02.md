# 1. Servlet 中如何获取 Session 对象？
使用HttpServletRequest对象的getSession方法获取session，通过getCookies获取Cookie。

# 2. Servlet 中过滤器有什么作用？
Servlet监听器对特定的事件进行监听，当产生这些事件的时候，会执行监听器的代码。可以对应用的加载、卸载，对session的初始化、销毁，对session中值变化等事件进行监听。

```java
void doFilter(..) {
// do stuff before servlet gets called

    // invoke the servlet, or any other filters mapped to the target servlet
    chain.doFilter(..);

    // do stuff after the servlet finishes
}
```

# 3. ServletContext 接口包括哪些功能？分别用代码示例。
**1、获取web应用的初始化参数**

使用getInitParameterNames()和getInitParameter(String name)来获得web应用中的初始化参数。

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
	request.setCharacterEncoding("utf-8");
	response.setContentType("text/html;charset=utf-8");
	PrintWriter outPrintWriter = response.getWriter();
	ServletContext context = this.getServletContext();                  //定义一个ServletContext对象
	Enumeration<String> enumeration = context.getInitParameterNames();  //用集合得方式存储配置文件中所有的name
	while(enumeration.hasMoreElements()){                              //判断是否为空
		String nameString = enumeration.nextElement();                 //跨过头部  提取第一个name
		String passString = context.getInitParameter(nameString);    //用getInitParameter(name)提取value
		outPrintWriter.print(nameString+"  "+passString);            //输出
	}
}
```

**2、实现多个servlet的数据共享**

```java
setAttribute(String name,String value) 来设置共享的数据
getAttribute(String name) 来获得共享得数据值
removeAttribute（String name） 删除
getAttributeNames()
```

写数据：ServletSet类

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
	// TODO Auto-generated method stub
	ServletContext context = this.getServletContext();
	context.setAttribute("公众号：Java精选", "123456");
}
```

读数据：ServletGet类

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
	// TODO Auto-generated method stub
	request.setCharacterEncoding("utf-8");
	response.setContentType("text/html;charset=utf-8");
	PrintWriter out = response.getWriter();
	ServletContext context = this.getServletContext();
	String pasString = (String) context.getAttribute("公众号：Java精选"); //要强制类型转换getAttribute的返回值为object
	out.print(pasString);
}
```

3、获取web应用下的资源文件（配置文件或图片）

```java
InputStream getResourceAsInputstream(String path)
String getRealPath(String path)
URL getResource(String path)
```
1）在src路径下创建配置文件创建文件source.properties（项目被发布后资源文件的路径发生改变）

2）获取资源文件定义的InoutStream and load()方法getProperty()方法

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
	response.setContentType("text/html");
	PrintWriter out = response.getWriter();
	ServletContext context = this.getServletContext();
	InputStream in = context.getResourceAsStream("D:/source.properties");
	Properties pros = new Properties();   //定义内容对象
	pros.load(in);     加载InputStream对象
	out.print(pros.getProperty("name"));  //使用getProperty函数输出文件里的哈希值对
	out.print(pros.getProperty("password"));
}
```

# 4. 如何实现 Servlet 单线程模式？
```java
<%@ page isThreadSafe="false"%>
```

# 5. Servlet 的生命周期有哪几个阶段？
Servlet 的生命周期分为三个阶段，分别对应Servlet中的三个接口。

init()初始化。

service()处理客户端的请求，具体业务逻辑。ServletRequest对象用于获得客户端信息，ServletResponse对象用于向客户端返回信息（客户端可以理解为浏览器）

destroy()结束时调用。这个方法只有在servlet的service方法内的所有线程都退出的时候，或在超时的时候才会被调用。

init()和destroy()方法在servlet的生命周期中只调用一次。其中init()方法在首次创建servlet时调用，在处理每个用户的请求时不再调用。init()方法主要用于一次性初始化；destroy()会在销毁时调用一次；service()则会在响应不同请求时多次调用。

web容器加载servlet，生命周期开始。通过调用servlet的init()方法进行servlet的初始化。通过调用service()方法实现根据请求的不同调用不同的do**()方法。结束服务，web容器调用servlet的destroy()方法。

注意的是Servlet是一个接口，实现了servlet的类，是不能直接处理请求的。请求需要通过Servlet容器来发送到Servlet，Servlet是运行在Servlet容器中的。

Servlet容器是Web服务器和servlet进行交互的必不可少的组件。常见Web服务器有Tomcat、jetty、resin等，它们也可以称为应用服务器。

# 6. 说一说 Servlet 容器对 url 匹配过程？
当一个请求发送到servlet容器的时候，容器先会将请求的url减去当前应用上下文的路径作为servlet的映射url，比如我访问的是http://localhost/test/aaa.html，我的应用上下文是test，容器会将http://localhost/test去掉，剩下的/aaa.html部分拿来做servlet的映射匹配。这个映射匹配过程是有顺序的，而且当有一个servlet匹配成功以后，就不会去理会剩下的servlet了（filter不同，后文会提到）。

匹配规则和顺序如下：

1）精确路径匹配。例子：比如servletA 的url-pattern为 /test，servletB的url-pattern为 /* ，这个时候，如果我访问的url为http://localhost/test ，这个时候容器就会先 进行精确路径匹配，发现/test正好被servletA精确匹配，那么就去调用servletA，也不会去理会其他的servlet了。

2）最长路径匹配。例子：servletA的url-pattern为/test/，而servletB的url-pattern为/test/a/，此时访问http://localhost/test/a时，容器会选择路径最长的servlet来匹配，也就是这里的servletB。

3）扩展匹配，如果url最后一段包含扩展，容器将会根据扩展选择合适的servlet。例子：servletA的url-pattern：*.action

4）如果前面三条规则都没有找到一个servlet，容器会根据url选择对应的请求资源。如果应用定义了一个default servlet，则容器会将请求丢给default servlet（什么是default servlet?后面会讲）。

根据这个规则表，就能很清楚的知道servlet的匹配过程，所以定义servlet的时候也要考虑url-pattern的写法，以免出错。

对于filter，不会像servlet那样只匹配一个servlet，因为filter的集合是一个链，所以只会有处理的顺序不同，而不会出现只选择一个filter。Filter的处理顺序和filter-mapping在web.xml中定义的顺序相同。

url-pattern详解

在web.xml文件中，以下语法用于定义映射：

>以”/’开头和以”/”结尾的是用来做路径映射的。 以前缀”.”开头的是用来做扩展映射的。 “/” 是用来定义default servlet映射的。 剩下的都是用来定义详细映射的。比如： /aa/bb/cc.action

所以，为什么定义”/*.action”这样一个看起来很正常的匹配会错?因为这个匹配即属于路径映射，也属于扩展映射，导致容器无法判断。

# 7. jsp/servlet 中如何保证 browser 保存在 cache 中？
JSP文件头部增加如下信息即可：

```html
<%
response.setHeader("Cache-Control","no-store"); //HTTP 1.1
response.setHeader("Pragma","no-cache"); //HTTP 1.0
response.setDateHeader ("Expires", 0); //prevents caching at the proxy server
%>
```

#8. Servlet 中如何获取客户端机器的信息？
Servlet使用getRemoteAddr()和getRemoteHost()来得到客户端的IP地址和host。

```java
public String ServletRequest.getRemoteAddr()
public Stirng ServletRequest.getRemoteHost()
```

实现对客户端配置进行检查并把相关消息发送到客户端的功能：

```java
import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class JingXuanServlet extends HttpServlet {

	private static final long serialVersionUID = 1L;

	public void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {

		res.setContentType("text/plain");
		PrintWriter out = res.getWriter();
		String remoteHost = req.getRemoteHost();
		if (!isHostAllowed(remoteHost)) {
			out.println("拒绝关注“Java精选”公众号，3000+面试题看不到");
		} else {
			out.println("允许关注“Java精选”公众号，3000+面试题免费看");
		}
	}

	private boolean isHostAllowed(String host) {
		return (host.endsWith(".com")) || (host.indexOf('.') == -1);
	}
}
```

# 9. 什么是 Servlet 链？
与UNIX和DOS命令中的管道类似，你也可以将多个servlet以特定顺序链接起来。在servlet链中，一个servlet的输出被当作下一个servlet的输入，而链中最后一个servlet的输出被返回到浏览器。

servlet链接提供了将一个servlet的输出重定向为另一个servlet的输入的能力。这样，你就可以划分工作，从而使用一系列servlet来实现它。另外，你还可以将servlet组织在一起以提供新的功能。

# 10. Java 中 Servlet 主要功能作用是什么？
Servlet通过创建一个框架来扩展服务器的能力，以提供在Web上进行请求和响应服务。

当客户机发送请求至服务器时，服务器可以将请求信息发送给Servlet ，并让Servlet建立起服务器返回给客户机的响应。当启动Web服务器或客户机第一次请求服务时，可以自动装入Servlet。装入后Servlet继续运行直到其它客户机发出请求。

Servlet完成如下功能：

创建并返回一个包含基于客户请求性质的动态内容的完整的HTML页面。

创建可嵌入到现有HTML页面中的一部分HTML页面（HTML片段）。

与其它服务器资源（包括数据库和基于Java的应用程序）进行通信。

用多个客户机处理连接，接收多个客户机的输入，并将结果广播到多个客户机上。例如，Servlet可以是多参与者的游戏服务器。

当允许在单连接方式下传送数据的情况下，在浏览器上打开服务器至 applet的新连接，并将该连 接保持在打开状态。当允许客户机和服务器简单、高效地执行会话的情况下， applet也可以启动客户浏览器和服务器之间的连接。可以通过定制协议或标准（如IIOP）进行通信。

对特殊的处理采用MIME类型过滤数据，例如图像转换和服务器端包括（SSI）。

将定制的处理提供给所有服务器的标准例行程序。例如，Servlet可以修改如何认证用户。

# 11. JavaWeb 中四大域对象及作用范围？
PageContext：作用范围在整个页面（一个页面）。

HttpRequest：作用范围在一次请求。

HttpSession：这是JavaWeb的一种会话机制，作用在整个会话中。

ServletContext：作用范围在整个web应用。

# 12. JSP 中静态包含和动态包含有什么区别？
动态包含

```html
<jsp:include page="jingxuan.jsp" flush="true"/>
```

比如a.jsp动态导入了b.jsp，只有当服务器访问a.jsp中的b.jsp模块时，java才会编译执行b.jsp文件，将其结果动态包含进来。

>1、会将多个jsp页面分别再编写成java文件，编译成class文件。 2、jsp文件中允许有相同的变量名，每个页面互不影响。 3、当java代码比较多优先选用动态导入。 4、效率相对较低，耦合性低。

动态包含用于加载经常变化的、要求显示最新版本内容的数据。

静态包含

```html
＜%@ include file="jingxuan.htm"%＞
```

>1、会将多个jsp页面合成一个jsp页面，再编写成java文件，编译成class文件。 2、jsp文件中不允许有相同的变量名。 3、当java代码比较少或者没有java代码是优先选用静态导入。 4、效率相对较高，耦合性高。

静态包含一般用于加载进页面显示后就再也不变的数据。

# 13. JSP 中动态 include 和静态 include 有什么区别？
include指令用于把另一个页面包含到当前页面中，在转换成servlet时包含进去。

动态include用jsp:include动作实现 <jsp:include page="jingxuan.jsp" flush="true" />它总是会检查所含文件中的变化，适合用于包含动态页面，并且可以带参数。

静态include用include伪码实现,定不会检查所含文件的变化,适用于包含静态页面<%@ include file="jingxuan.htm" %>。

# 14. 什么是 Cookie？
Cookie是客户端保存用户信息的一种机制，用来记录用户的一些信息，它的过期时间可以任意设置，如果不主动清除，很长一段时间都能保留。

# 15. 什么是 Session？
Session是在无状态的HTTP协议下，服务端记录用户状态时用于标识具体用户的机制，它是在服务端保存的用来跟踪用户的状态的数据结构，可以保存在文件、数据库、或者集群中。

# 16. Session 和 Cookie 有什么区别？
Session和Cookie都是一种会话技术。

**Session**

数据存放在服务端，安全（只存放和状态相关的）。

session不仅仅是存放字符串，还可以存放对象。

session是域对象（session本身是不能跨域的，但可以通过相应技术来解决）。

sessionID的传输默认是需要cookie支持的。

**Cookie**

数据存放在客户端，不安全（存放的数据一定是和安全无关紧要的数据）。

cookie只能存放字符串，不能存放对象。

cookie不是域对象（Cookie是支持跨域的）。

# 17. 什么是 B/S 和 C/S？
Browser/Server 浏览器/服务器（瘦客户端）

Custom/Server 客户端/服务器（胖客户端）

# 18. B/S 和 C/S 有什么联系与区别？
**B/S**

B/S模式是指在TCP/IP的支持下，以HTTP为传输协议，客户端通过Browser访问Web服务器以及与之相连的后台数据库的技术及体系结构。它由浏览器、Web服务器、应用服务器和数据库服务器组成。

客户端的浏览器通过URL访问Web服务器，Web服务器请求数据库服务器，并将获得的结果以HTML形式返回客户端浏览器。

**C/S**

C/S在系统机构上和B/S相似，不过需要在客户端安装一个客户端软件，由这个软件对服务器的数据进行读写，就像我们常用的qq，就是这种模式。

# 19. Servlet 接口的层次结构？
Servlet->genericServlet->HttpServlet->自定义servlet

GenericServlet实现Servlet接口，同时为它的子类屏蔽了不常用的方法，子类只需要重写service方法即可。

HttpServlet继承GenericServlet，根据请求类型进行分发处理，GET进入doGet方法，POST进入doPost方法。

开发者自定义的Servlet类只需要继承HttpServlet即可，重新doGet和doPost。

# 20. 什么是 ServletContext？
ServletContext是一个接口，它表示Servlet上下文对象一个工程或者一个模块只会有一个ServletContext对象实例，即意思是无论获取到多少个ServletContext对象其实就是一个对象，只是名字可能不一样。

ServletContext也可以理解成是一块所有客户都可以访问的共享的服务器端空间，类似Session。
