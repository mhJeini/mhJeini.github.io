# 1. 为什么 TCP 握手三次，挥手四次？
因为只有在客户端和服务端都没有数据要发送的时候才能断开TCP。而客户端发出FIN报文时只能保证客户端没有数据发了，服务端还有没有数据发客户端是不知道的。

服务端收到客户端的FIN报文后只能先回复客户端一个确认报文来告诉客户端，服务端已经收到FIN报文，但服务端还有一些数据没发完，等这些数据发完了服务端才能给客户端发FIN报文，所以不能一次性将确认报文和FIN报文发给客户端，就是这里多出来了一次。

# 2. 为什么 TCP 握手三次？两次不可以吗？
因为需要考虑连接时丢包的问题，如果只握手2次，第二次握手时如果服务端发给客户端的确认报文段丢失，此时服务端已经准备好了收发数(可以理解服务端已经连接成功)据，而客户端一直没收到服务端的确认报文，所以客户端就不知道服务端是否已经准备好了(可以理解为客户端未连接成功)，这种情况下客户端不会给服务端发数据，也会忽略服务端发过来的数据。

如果是三次握手，即便发生丢包也不会有问题，比如如果第三次握手客户端发的确认ack报文丢失，服务端在一段时间内没有收到确认ack报文的话就会重新进行第二次握手，也就是服务端会重发SYN报文段，客户端收到重发的报文段后会再次给服务端发送确认ack报文。

# 3. 为什么客户端发出第四次挥手确认报文后要等 2MSL才能释放 TCP 连接？
这是因为考虑数据丢包的问题，如果第四次挥手的报文丢失，服务端没收到确认ack报文就会重发第三次挥手的报文，这样报文一去一回最长时间就是2MSL，所以需要等这么长时间来确认服务端确实已经收到。

注意：Linux系统中2MSL默认是60秒，那么一个MSL也就是30秒。

# 4. 如果已建立连接，此时客户端出现故障会如何？
TCP设有一个保活计时器，客户端如果出现故障，服务器不能一直等下去，白白浪费资源。

服务器每收到一次客户端的请求后都会重新复位这个计时器，时间通常是设置为2小时，若两小时还没有收到客户端的任何数据，服务器就会发送一个探测报文段，以后每隔75秒钟发送一次。若一连发送10个探测报文仍然没反应，服务器就认为客户端出了故障，接着就关闭连接。

# 5. HTTP 和 HTTPS 有什么区别？
1、HTTP的URL以http://开头，而HTTPS的URL以https://开头。

2、HTTP是不安全的，而HTTPS是安全的。

3、HTTP标准端口是80，而HTTPS的标准端口是443。

4、在OSI网络模型中，HTTP工作于应用层，而HTTPS的安全传输机制工作在传输层。

5、HTTP无法加密，而HTTPS对传输的数据进行加密。

6、HTTP无需证书，而HTTPS需要CA机构wosign的颁发的SSL证书。

| 区别	   | HTTP	                             | HTTPS                                                                    |
|-------|-----------------------------------|--------------------------------------------------------------------------|
| 协议	   | 运行在TCP之上，明文传输，客户端与服务器端都无法验证对方的身份	 | 身披SSL（Secure Socket Layer）外壳的HTTP，运行于SSL上，SSL运行于TCP之上， 是添加了加密和认证机制的HTTP。 |
| 端口	   | 80	                               | 443                                                                      |
| 资源消耗	 | 较少	                               | 由于加解密处理，会消耗更多的CPU和内存资源                                                   |
| 开销	   | 无需证书	                             | 需要证书，而证书一般需要向认证机构购买                                                      |
| 加密机制	 | 无	                                | 共享密钥加密和公开密钥加密并用的混合加密机制                                                   |
| 安全性	  | 弱	                                | 由于加密机制，安全性强                                                              |
# 6. 什么是对称加密和非对称加密？
对称密钥加密是指加密和解密使用同一个密钥的方式，这种方式存在的最大问题就是密钥发送问题，即如何安全地将密钥发给对方；

而非对称加密是指使用一对非对称密钥，即公钥和私钥，公钥可以随意发布，但私钥只有自己知道。发送密文的一方使用对方的公钥进行加密处理，对方接收到加密信息后，使用自己的私钥进行解密。

由于非对称加密的方式不需要发送用来解密的私钥，所以可以保证安全性；但是和对称加密比起来，非常的慢。

# 7. URL 和 URI 有什么区别？
统一资源标志符URI(uniform resource identifier ）是标识逻辑或物理资源的字符序列，与URL类似，也是一串字符。通过使用位置，名称或两者来标识Internet上的资源；它允许统一识别资源。

有两种类型的URI，统一资源标识符（URL）和统一资源名称（URN）。

URI是指在某一规则下能把一个资源独一无二地标识出来。而URL是通过规则的一种L（locator）定位来标识资源的，因此URL是URI的一种实现方式。

举例，假设世界上所有人的名字都不能重复，那么名字就是URI的一个实例，通过名字就可以标识出唯一的一个人。但是现实中名字肯定是会重复的，所以身份证号才是URI，通过身份证号能让我们能且仅能确定一个人。

统一资源定位符URL（(uniform resource locator）：是Internet上资源的地址，可以定义为引用地址的字符串，用于指示资源的位置以及用于访问它的协议。

URL是在网络上定位资源的最普遍使用的方式，它提供了一种通过描述其网络位置或主要访问机制来检索物理位置的表示的方法。

URL包含以下信息：

1、用于访问资源的协议 2、服务器的位置（无论是通过IP地址还是域名） 3、服务器上的端口号（可选） 4、资源在服务器目录结构中的位置 5、片段标识符（可选）

举例与HTTP的URL做类比，如下：

动物住址协议://地球/中国/河北省/唐山市/../../张三

通过上述描述，这个字符串同样标识出唯一的一个人，起到了URI的作用，所以URL是URI的子集。

# 8. HTTP 中常见的请求头有哪些？
Accept：浏览器可接受的MIME类型；

Accept-Charset：浏览器可接受的字符集；

Accept-Encoding：浏览器能够进行解码的数据编码方式，比如gzip。Servlet能够向支持gzip的浏览器返回经gzip编码的HTML页面。许多情形下这可以减少5到10倍的下载时间；

Accept-Language：浏览器所希望的语言种类，当服务器能够提供一种以上的语言版本时要用到；

Authorization：授权信息，通常出现在对服务器发送的WWW-Authenticate头的应答中；

Connection：表示是否需要持久连接。如果Servlet看到这里的值为“Keep-Alive”，或者看到请求使用的是HTTP 1.1（HTTP 1.1默认进行持久连接），它就可以利用持久连接的优点，当页面包含多个元素时（例如Applet，图片），显著地减少下载所需要的时间。要实现这一点，Servlet需要在应答中发送一个Content-Length头，最简单的实现方法是：先把内容写入ByteArrayOutputStream，然后在正式写出内容之前计算它的大小；

Content-Length：表示请求消息正文的长度；

Cookie：这是最重要的请求头信息之一；

From：请求发送者的email地址，由一些特殊的Web客户程序使用，浏览器不会用到它；

Host：初始URL中的主机和端口；

If-Modified-Since：只有当所请求的内容在指定的日期之后又经过修改才返回它，否则返回304“Not Modified”应答；

Pragma：指定“no-cache”值表示服务器必须返回一个刷新后的文档，即使它是代理服务器而且已经有了页面的本地拷贝；

Referer：包含一个URL，用户从该URL代表的页面出发访问当前请求的页面。

User-Agent：浏览器类型，如果Servlet返回的内容与浏览器类型有关则该值非常有用；

UA-Pixels，UA-Color，UA-OS，UA-CPU：由某些版本的IE浏览器所发送的非标准的请求头，表示屏幕大小、颜色深度、操作系统和CPU类型。

# 9. HTTP 中请求报文包含哪几部分？
客户端发送一个请求报文给服务器，服务器根据请求报文中的信息进行处理，并将处理结果放入响应报文中返回给客户端。

请求报文结构：

1、第一行是包含了请求方法、URL、协议版本；

2、接下来的多行都是请求首部 Header，每个首部都有一个首部名称，以及对应的值。

3、一个空行用来分隔首部和内容主体Body。

4、最后是请求的内容主体。

```shell
GET http://www.example.com/ HTTP/1.1
Accept:text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8
Cache-Control: max-age=0
Host: www.example.com
If-Modified-Since: Thu, 17 Oct 2019 07:18:26 GMT
If-None-Match: "3147526947+gzip"
Proxy-Connection: keep-alive
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 xxx
param1=1&param2=2
```



# 10. HTTP 中常见的响应头有哪些？
Allow： 服务器支持哪些请求方法（如GET、POST等）；

Content-Encoding： 文档的编码（Encode）方法。只有在解码之后才可以得到Content-Type头指定的内容类型。利用gzip压缩文档能够显著地减少HTML文档的下载时间。Java的GZIPOutputStream可以很方便地进行gzip压缩，但只有Unix上的Netscape和Windows上的IE 4、IE 5才支持它。因此，Servlet应该通过查看Accept-Encoding头（即request.getHeader("Accept-Encoding")）检查浏览器是否支持gzip，为支持gzip的浏览器返回经gzip压缩的HTML页面，为其他浏览器返回普通页面；

Content-Length： 表示内容长度。只有当浏览器使用持久HTTP连接时才需要这个数据。如果你想要利用持久连接的优势，可以把输出文档写入ByteArrayOutputStram，完成后查看其大小，然后把该值放入Content-Length头，最后通过byteArrayStream.writeTo(response.getOutputStream()发送内容；

Content-Type： 表示后面的文档属于什么MIME类型。Servlet默认为text/plain，但通常需要显式地指定为text/html。由于经常要设置Content-Type，因此HttpServletResponse提供了一个专用的方法setContentTyep。 可在web.xml文件中配置扩展名和MIME类型的对应关系；

Date： 当前的GMT时间。你可以用setDateHeader来设置这个头以避免转换时间格式的麻烦；

Expires： 指明应该在什么时候认为文档已经过期，从而不再缓存它。

Last-Modified： 文档的最后改动时间。客户可以通过If-Modified-Since请求头提供一个日期，该请求将被视为一个条件GET，只有改动时间迟于指定时间的文档才会返回，否则返回一个304（Not Modified）状态。Last-Modified也可用setDateHeader方法来设置；

Location： 表示客户应当到哪里去提取文档。Location通常不是直接设置的，而是通过HttpServletResponse的sendRedirect方法，该方法同时设置状态代码为302；

Refresh： 表示浏览器应该在多少时间之后刷新文档，以秒计。除了刷新当前文档之外，还可以通过

```js
setHeader("Refresh", "5; URL=http://host/path")
```

让浏览器读取指定的页面。注意这种功能通常是通过设置HTML页面HEAD区的

```html
<META HTTP-EQUIV="Refresh" CONTENT="5;URL=http://host/path">
```
实现，这是因为，自动刷新或重定向对于那些不能使用CGI或Servlet的HTML编写者十分重要。但是，对于Servlet来说，直接设置Refresh头更加方便。注意Refresh的意义是“N秒之后刷新本页面或访问指定页面”，而不是“每隔N秒刷新本页面或访问指定页面”。因此，连续刷新要求每次都发送一个Refresh头，而发送204状态代码则可以阻止浏览器继续刷新，不管是使用Refresh头还是

```html
<META HTTP-EQUIV="Refresh" ...>
```
注意Refresh头不属于HTTP 1.1正式规范的一部分，而是一个扩展，但Netscape和IE都支持它。

# 11. HTTP 中响应报文包含哪几部分？
响应报文结构：

1、第一行包含协议版本、状态码以及描述，最常见的是 200 OK 表示请求成功了。

2、接下来多行也是首部内容。

3、一个空行分隔首部和内容主体。

4、最后是响应的内容主体。

```html
HTTP/1.1 200 OK
Age: 529651
Cache-Control: max-age=604800
Connection: keep-alive
Content-Encoding: gzip
Content-Length: 648
Content-Type: text/html; charset=UTF-8
Date: Mon, 02 Nov 2020 17:53:39 GMT
Etag: "3147526947+ident+gzip"
Expires: Mon, 09 Nov 2020 17:53:39 GMT
Keep-Alive: timeout=4
Last-Modified: Thu, 17 Oct 2019 07:18:26 GMT
Proxy-Connection: keep-alive
Server: ECS (sjc/16DF)
Vary: Accept-Encoding
X-Cache: HIT

<!doctype html>
<html>
<head>
    <title>Example Domain</title>
	// 省略... （关注微信公众号Java精选，内涵3000+道面试题，免费领取）
</body>
</html>
```
# 12. 什么是 Socket？
socket是应用层与传输层的一个抽象，将复杂的TCP/IP协议隐藏在Socket接口之后，只对应用层暴露简单的接口。

socket是一种特殊的文件，它也有文件描述符，进程可以打开一个socket，并且像处理文件一样对它进行read()和write()操作，而不必关心数据是怎么在网络上传输的。

socket是一个tcp连接的两端。

# 13. Socket 如何唯一标识一个进程？
socket基于tcp协议实现，网络层的ip地址唯一标识一台主机，而传输层的协议+端口号可以唯一标识绑定到这个端口的进程。

# 14. 通信双方如何进行端口绑定？
通常服务端启动时会绑定一个端口提供服务，而客户端在发起连接请求时会被随机分配一个端口号。

# 15. Socket 属于网络的哪一层？
Socket不算是一个协议，它是应用层与传输层间的一个抽象层。它把TCP/IP层复杂的操作抽象为几个简单的接口供应用层调用，以实现进程在网络中通信。

# 16. Socket 是全双工通信的吗？
Socket基于TCP协议，是全双工通信的。

# 17. HTTP 协议是全双工通信的吗？
HTTP协议设计的初衷本身就是请求/响应模式，这是规范决定的。不过在技术上是可以利用下层的TCP来进行全双工通信的。

# 18. TCP 中什么是粘包和拆包？
TCP的数据发送都是靠流，流由一个接一个的数据包组成。

发送过程中tcp会把数据拆成很多个包，也有可能将小的数据合成一个大包。

# 19. TCP 中在什么情况下发生粘包问题？
1、应用程序write的字节小于大于socket发送缓冲区大小；

2、进行MSS大小的TCP分段；

3、以太网帧payload大于MTU进行IP分片。

# 20. TCP 中粘包问题有什么解决策略？
由于底层TCP无法理解上层的业务数据，所以在底层是无法保证数据包不被拆分和充足，只能同设计上层的应用的协议栈来解决，根据业界的主流协议的解决方法，归纳如下

1、消息定长，每个报文固定长度为200空格，不够空格来凑。

2、包尾增加回车换行符进行分割，例如FTP协议。

3、将消息分为消息头和消息体，消息头包含消息长度，通常为第一个字段int32来表示消息的总长度。

4、更复杂的应用层协议。
