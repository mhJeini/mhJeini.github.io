# 1. JSP 中 application 对象有什么作用？
application对象可将信息保存在服务器中，直到服务器关闭，否则application对象中保存的信息会在整个应用中都有效。与session对象相比，application对象生命周期更长，类似于系统的“全局变量”。

服务器启动后就产生了这个Application对象，当客户再所访问的网站的各个页面之间浏览时，这个Application对象都是同一个，直到服务器关闭。但是与Session对象不同的时，所有客户的Application对象都时同一个，即所有客户共享这个内置的Application对象。

setAttribute(String key,Object obj)：将参数Object指定的对象obj添加到Application对象中，并为添加的对象指定一个索引关键字。

getAttribute(String key)：获取Application对象中含有关键字的对象。

# 2. JSP 中 out 对象有什么作用？
out对象用于在Web浏览器内输出信息，并且管理应用服务器上的输出缓冲区。在使用 out 对象输出数据时，可以对数据缓冲区进行操作，及时清除缓冲区中的残余数据，为其他的输出让出缓冲空间。待数据输出完毕后，要及时关闭输出流。

out对象时一个输出流，用来向客户端输出数据。Out对象用于各种数据的输出。其常用方法如下。

out.print()：输出各种类型数据。

out.newLine()：输出一个换行符。

# 3. JSP 中 cookie 对象有什么作用？
Cookie是Web服务器保存在用户硬盘上的一段文本。Cookie允许一个Web站点在用户电脑上保存信息并且随后再取回它。举例来说，一个Web站点可能会为每一个访问者产生一个唯一的ID，然后以Cookie文件的形式保存在每个用户的机器上。

创建一个Cookie对象 调用Cookie对象的构造函数就可以创建Cookie对象。Cookie对象的构造函数有两个字符串参数：Cookie名字和Cookie值。

```java
Cookie c = new Cookie("username","公众号Java精选");
```

将Cookie对象传送到客户端。

JSP中，如果要将封装好的Cookie对象传送到客户端，可使用Response对象的addCookie()方法。

使用Request对象的getCookie()方法，执行时将所有客户端传来的Cookie对象以数组的形式排列，如果要取出符合需要的Cookie对象，就需要循环比较数组内每个对象的关键字。

设置Cookie对象的有效时间，用Cookie对象的setMaxAge()方法便可以设置Cookie对象的有效时间，

```java
Cookie c = newCookie("username","公众号Java精选");
c.setMaxAge(3600);
```

Cookie对象的典型应用时用来统计网站的访问人数。

由于代理服务器、缓存等的使用，唯一能帮助网站精确统计来访人数的方法就是为每个访问者建立一个唯一ID。

# 4. JSP 中 exception 对象有什么作用？
exception对象的作用是显示异常信息，只有在包含 isErrorPage="true"的页面中才可以被使用，在一般的JSP页面中使用该对象将无法编译JSP文件。

excepation对象和Java的所有对象一样，都具有系统提供的继承 结构。

exception对象几乎定义了所有异常情况。在Java程序中，可以使用try/catch关键字来处理异常情况； 如果在JSP页面中出现没有捕获到的异常，就会生成exception对象，并把exception对象传送到在page指令中设定的错误页面中，然后在错误页面中处理相应的exception对象。

# 5. 什么是跨域？
跨域是指浏览器不能执行其他网站的脚本。它是由浏览器的同源策略造成的，是浏览器对JavaScript实施的安全限制。

当一个请求url的协议、域名、端口三者之间任意一个与当前页面url不同即为跨域

| 当前页面url                     | 	被请求页面url                          | 	是否跨域   | 	原因                |
|-----------------------------|------------------------------------|---------|--------------------|
| http://blog.yoodb.com/      | 	http://blog.yoodb.com/index.html  | 	否      | 	同源（协议、域名、端口号相同）   |
| http://blog.yoodb.com/      | 	https://blog.yoodb.com/index.html | 	跨域     | 	协议不同（http/https）  |
| http://blog.yoodb.com/      | 	http://www.baidu.com/             | 	跨域     | 	主域名不同（test/baidu） |
| http://blog.yoodb.com/      | 	http://blog.test.com/             | 	跨域     | 	子域名不同（www/blog）   |
| http://blog.yoodb.com:8080/ | 	http://blog.yoodb.com:7001/       | 	跨域     | 	端口号不同（8080/7001）  |

# 6. 为什么会出现跨域问题？
出现跨域问题是由于浏览器的同源策略限制。 | |

同源策略（Sameoriginpolicy）是一种约定，它是浏览器最核心也最基本的安全功能，如果缺少了同源策略，则浏览器的正常功能可能都会受到影响。

可以说Web是构建在同源策略基础之上的，浏览器只是针对同源策略的一种实现。

同源策略会阻止一个域的javascript脚本和另外一个域的内容进行交互。

所谓同源（即指在同一个域）就是两个页面具有相同的协议（protocol），主机（host）和端口号（port）。

# 7. 跨域问题有哪些解决方法？
1、设置document.domain解决无法读取非同源网页的 Cookie问题

2、跨文档通信 API：window.postMessage()

3、JSONP方式

JSONP是服务器与客户端跨源通信的常用方法。最大特点就是简单适用，兼容性好（兼容低版本IE），缺点是只支持get请求，不支持post请求。

网页通过添加一个script元素，向服务器请求JSON数据，服务器收到请求后，将数据放在一个指定名字的回调函数的参数位置传回来。

**1）JQuery ajax**

```html
$.ajax({
    url: 'https://blog.yoodb.com/',
    type: 'get',
    dataType: 'jsonp', // 请求方式为jsonp
    jsonpCallback: "handleCallback",// 自定义回调函数名
    data: {}
});
```

**2）Vue.js**

```html
this.$http.jsonp('https://blog.yoodb.com/', {
    params: {},
    jsonp: 'handleCallback'
    }).then((res) => {
        console.log(res);
})
```

4、CORS跨域资源共享

CORS 是跨域资源分享（Cross-Origin Resource Sharing）的缩写。它是 W3C 标准，属于跨源 AJAX 请求的根本解决方法。

1）普通跨域请求：只需服务器端设置Access-Control-Allow-Origin。

2）带cookie跨域请求：前后端都需要进行设置。

5、服务端设置

服务器端对于CORS的支持，主要是通过设置Access-Control-Allow-Origin来进行的。如果浏览器检测到相应的设置，就可以允许Ajax进行跨域的访问。

1）Java语言

```java
// 允许跨域访问的域名：若有端口需写全（协议+域名+端口），若没有端口末尾不用加'/'
response.setHeader("Access-Control-Allow-Origin", "https://blog.yoodb.com");

// 允许前端带认证cookie：启用此项后，上面的域名不能为'*'，必须指定具体的域名，否则浏览器会提示
response.setHeader("Access-Control-Allow-Credentials", "true");

// 提示OPTIONS预检时，后端需要设置的两个常用自定义头
response.setHeader("Access-Control-Allow-Headers", "Content-Type,X-Requested-With");
```

# 8. Cookie 禁用，Session 还能用吗？
不能。

Session采用的是在服务器端保持状态的方法，而Cookie采用的是在客户端保持状态的方法。

至于为什么禁用Cookie就不能得到Session，这是因为Session是用Session ID来确定当前对话所对应的服务器Session，而Session ID是通过Cookie来传递的，禁用Cookie相当于失去了Session ID，也就得不到Session了。

# 9. 什么是 Token？
Token的引入：Token是在客户端频繁向服务端请求数据，服务端频繁的去数据库查询用户名和密码并进行对比，判断用户名和密码正确与否，并作出相应提示，在这样的背景下，Token便应运而生。

Token的定义：Token是服务端生成的一串字符串，以作客户端进行请求的一个令牌，当第一次登录后，服务器生成一个Token便将此Token返回给客户端，以后客户端只需带上这个Token前来请求数据即可，无需再次带上用户名和密码。

使用Token的目的：Token的目的是为了减轻服务器的压力，减少频繁的查询数据库，使服务器更加健壮。

Token 是在服务端产生的。如果前端使用用户名/密码向服务端请求认证，服务端认证成功，那么在服务端会返回 Token 给前端。前端可以在每次请求的时候带上 Token 证明自己的合法地位。

# 10. session 和 token 有什么区别？
session机制存在服务器压力增大，CSRF跨站伪造请求攻击，扩展性不强等问题；

session存储在服务器端，token存储在客户端；

token提供认证和授权功能，作为身份认证，token安全性比session好；

session这种会话存储方式方式只适用于客户端代码和服务端代码运行在同一台服务器上，token适用于项目级的前后端分离（前后端代码运行在不同的服务器下）。

# 11. 什么是 Web Service？
Web Service是一个SOA（面向服务的编程）的架构，它是不依赖于语言，不依赖于平台，可以实现不同的语言间的相互调用，通过Internet进行基于Http协议的网络应用间的交互。

一句话概括：Web Service是一种跨编程语言和跨操作系统平台的远程调用技术。

Web Service实现不同语言间的调用，是依托于一个标准，Web service是需要遵守WSDL（web服务定义语言）/SOAP（简单请求协议）规范的。

Web Service = WSDL+SOAP+UDDI（webservice的注册）Soap是由Soap的part和0个或多个附件组成，一般只有part，在part中有Envelope和Body。

Web Service是通过提供标准的协议和接口，可以让不同的程序集成的一种SOA架构。

# 12. Web Service 的核心组成包括哪些内容？
1、soap：简单对象访问协议，它是轻型协议，用于分散的、分布式计算环境中交换信息。SOAP有助于以独立于平台的方式访问对象、服务和服务器。它借助于XML，提供了HTTP所需的扩展，即http+xml。

2、XML+XSD：Web Service平台中表示数据的格式是XML，XML解决了数据表示的问题，但它没有定义一套标准的数据类型，更没有说怎么去扩展这套数据类型，而XSD就是专门解决这个问题的一套标准。它定义了一套标准的数据类型，并给出了一种语言来扩展这套数据类型。

3、wsdl：基于XML用于描述Web Service及其函数、参数和返回值的文件。

Web Service服务器端通过一个WSDL文件来说明自身对外提供什么服务，该服务包括什么方法、参数、返回值等。WSDL文件保存在Web服务器上，通过一个url地址就可以访问到它。客户端要调用一个Web Service服务之前，首先要知道该服务的WSDL文件的地址。

Web Service服务的WSDL文件地址可以通过两种方式来暴露：

1）注册到UDDI服务器，以便被人查找；

2）直接告诉给客户端调用者。

4、uddi:它是目录服务，通过该服务可以注册和发布web Servcie，以便第三方的调用者统一调用。

# 13. Web Service 中 SEI 指什么？
SEI全称是WebService EndPoint Interface，即Web Service服务器端用来处理请求的接口。

# 14. 如何发布一个 Web Service 服务？
1、定义SEI（接口）使用@webservice（类）和@webMethod（暴露的方法）注解。

2、定义SEI的实现。

3、发布Endpoint.publish（url,new SEI的实现对象）。

# 15. 如何请求一个 Web Service 服务？
1、根据wsdl文档生成客户端代码

使用JDK中wsimport生成Web Service客户端Java类，命令“jdk wsimport -keep wsdl路径”。

使用apache-cxf中cxf生成We bService客户端Java类，命令“cxf wsdl2java wsdl路径”。

2、根据生成的代码调用Web Service

找到wsdl文档中service标签的name属性对应的类，找到这个port标签的name属性，调用该方法即可。

# 16. Web Service 中有哪些常用开发框架？
Apache Axis1、Apache Axis2、Codehaus XFire、Apache CXF、Apache Wink、Jboss RESTEasy、sun JAX-WS（最简单、方便）、阿里巴巴Dubbo等。

# 17. 谈谈 MVC 架构模式中的三个角色？
**Model（模型端）**

Mod封装的是数据源和所有基于对这些数据的操作。在一个组件中，Model往往表示组件的状态和操作这些状态的方法，往往是一系列的公开方法。通过这些公开方法，便可以取得模型端的所有功能。

在这些公开方法中，有些是取值方法，让系统其他部分可以得到模型端的内部状态参数，其他的改值方法则允许外部修改模型端的内部状态。模型端还必须有方法登记视图，以便在模型端的内部状态发生变化时，可以通知视图端。我们可以自己定义一个Subject接口来提供登记和通知视图所需的接口或者继承 Java.util.Observable类，让父类完成这件事。

**多个View（视图端）**

View封装的是对数据源Model的一种显示。一个模型可以由多个视图，并且可以在需要的时候动态地登记上所需的视图。而一个视图理论上也可以同不同的模型关联起来。

在前言里提到了，MVC模式用到了合成模式，这是因为在视图端里，视图可以嵌套，比如说在一个JFrame组件里面，可以有菜单组件，很多按钮组件等。

**多个Controller（控制器端）**

封装的是外界作用于模型的操作。通常，这些操作会转发到模型上，并调用模型中相应的一个或者多个方法（这个方法就是前面在介绍模型的时候说的改值方法）。一般Controller在Model和View之间起到了沟通的作用，处理用户在View上的输入，并转发给Model来更改其状态值。这样 Model和View两者之间可以做到松散耦合，甚至可以彼此不知道对方，而由Controller连接起这两个部分。也在前言里提到，MVC用到了策略模式，这是因为View用一个特定的Controller的实例来实现一个特定的响应策略，更换不同的Controller，可以改变View对用户输入的响应。

MVC (Model-View-Controller) : 模型利用"观察者"让控制器和视图可以随最新的状态改变而更新。另一方面，视图和控制器则实现了"策略模式"。控制器是视图的行为; 视图内部使用"组合模"式来管理显示组件。

以下的MVC解释图很好的标示了这种模式：

>模型使用观察者模式，以便观察者更新，同时保持两者之间的解耦。 控制器是视图的策略，视图可以使用不同的控制器实现，得到不同的行为。 视图使用组合模式实现用户界面，用户界面通常组合了嵌套的组件，像面板、框架和按钮。 这些模式携手合作，把MVC模式的三层解耦，这样可以保持设计干净又有弹性。

#  18. 如何保存会话状态？有哪些方式、区别？
1、cookie保存在客户端，容易被篡改；

2、session保存在服务端，连接较大的话会造成服务端压力过大，分布式的情况下可以存储子数据库中。

**优点：**

1）简单且高性能；

2）支持分布式与集群；

3）支持服务器断电和重启；

4）支持 tomcat、jetty 等运行容器重启。

**缺点：**

1）需要检查和维护session过期，手动维护cookie；

2）不能有频繁的session数据存取。

3、token多终端或者app应用，推荐使用token

随着互联网技术的发展，分布式web应用的普及，通过session管理用户登录状态成本越来越高，因此慢慢发展成为token的方式做登录身份校验，通过token去取redis中缓存的用户信息。之后jwt的出现，校验方式更加简单便捷化，无需通过redis缓存，而是直接根据token取出保存的用户信息，以及对token可用性校验，单点登录更为简单。

**JWT的token包含三部分数据：**

Header：头部，通常头部有两部分信息：1）声明类型，这里是JWT；2）加密算法，自定义。一般对头部进行base64加密（可解密），得到第一部分数据。

Payload：载荷，就是有效数据，一般包含下面信息：用户身份信息（需注意的是，这里因采用base64加密，可解密，因此不要存放敏感信息）。

注册声明：如token的签发时间，过期时间，签发人等，此部分采用base64加密，得到第二部分数据。

Signature：签名，是整个数据的认证信息。一般根据前两步的数据， 再加上服务的密钥（secret）。（保密性，周期性更换），通过加密算法生成。用于验证整个数据完整和可靠性（保密性，周期性更换），通过加密算法生成。用于验证整个数据完整和可靠性。

# 19. 什么时候用 assert？
assertion(断言)在软件开发中是一种常用的调试方式，很多开发语言中都支持这种机制。一般来说，assertion用于保证程序最基本、关键的正确性。assertion检查通常在开发和测试时开启。为了提高性能，在软件发布后， assertion检查通常是关闭的。在实现中，断言是一个包含布尔表达式的语句，在执行这个语句时假定该表达式为true；如果表达式计算为false，那么系统会报告一个AssertionError。

**断言用于调试目的：**

```java
assert(a > 0); // throws an AssertionError if a <= 0
```

**断言可以有两种形式：**

```java
assert Expression1;
assert Expression1 : Expression2 ;
```

Expression1 应该总是产生一个布尔值。

Expression2 可以是得出一个值的任意表达式；这个值用于生成显示更多调试信息的字符串消息。

断言在默认情况下是禁用的，要在编译时启用断言，需使用source 1.4 标记：

```java
javac -source 1.4 Test.java
```

要在运行时启用断言，可使用-enableassertions 或者-ea 标记。

要在运行时选择禁用断言，可使用-da 或者-disableassertions 标记。

要在系统类中启用断言，可使用-esa或者-dsa标记。还可以在包的基础上启用或者禁用断言。可以在预计正常情况下不会到达的任何位置上放置断言。断言可以用于验证传递给私有方法的参数。不过，断言不应该用于验证传递给公有方法的参数，因为不管是否启用了断言，公有方法都必须检查其参数。不过，既可以在公有方法中，也可以在非公有方法中利用断言测试后置条件。另外，断言不应该以任何方式改变程序的状态。

# 20. Applet 和 Servlet 有什么区别？
Applet是运行在客户端主机的浏览器上的客户端Java程序。而Servlet是运行在web服务器上的服务端的组件。

applet可以使用用户界面类，而Servlet没有用户界面，相反，Servlet是等待客户端的HTTP请求，然后为请求产生响应。
