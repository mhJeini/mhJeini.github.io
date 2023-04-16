# 1. 什么是 Nginx？
Nginx（engine x）是一个轻量级、高性能的HTTP和反向代理web服务器，同时也是一个IMAP、POP3、SMTP代理服务器。

Nginx是由伊戈尔·赛索耶夫为俄罗斯访问量第二的Rambler.ru站点（俄文：Рамблер）开发的，第一个公开版本0.1.0发布于2004年10月4日。

Nginx相比较Apache、lighttpd具有占有内存少，稳定性高等优势，并且依靠并发能力强，丰富的模块库以及友好灵活的配置而闻名。

# 2. 为什么要使用 Nginx？
1、跨平台、配置简单。

2、非阻塞、高并发连接：处理2-3万并发连接数，官方监测能支持5万并发。

3、内存消耗小：开启10个nginx才占150M内存，Nginx采取了分阶段资源分配技术。

4、nginx处理静态文件好,耗费内存少。

5、内置的健康检查功能：如果有一个服务器宕机，会做一个健康检查，再发送的请求就不会发送到宕机的服务器了。重新将请求提交到其他的节点上。

6、节省宽带：支持GZIP压缩，可以添加浏览器本地缓存。

7、稳定性高：宕机的概率非常小。

8、master/worker结构：一个master进程，生成一个或者多个worker进程。

9、接收用户请求是异步的：浏览器将请求发送到nginx服务器，它先将用户请求全部接收下来，再一次性发送给后端web服务器，极大减轻了web服务器的压力，一边接收web服务器的返回数据，一边发送给浏览器客户端。

10、网络依赖性比较低，只要ping通就可以负载均衡。

11、可以有多台nginx服务器。

**总体来说：**

Nginx支持跨平台、高并发，配置简单，实现了非常高效的反向代理、负载平衡，它可以处理2-3万并发连接数，官方监测可支持5万并发，目前我国使用Nginx有很多知名网站。

Nginx内存占用少，处理静态文件速度快且Nginx内置了健康检查功能，也就是如果有一个服务器宕机，会做一个健康检查，再发送请求时就不会发送到宕机的服务器上，Nginx会重新将请求提交到其他的节点上。

Nginx对宽带要求低，支持GZIP压缩，还可以添加浏览器本地缓存；其稳定性高，存在宕机的概率非常小，支持异步接收用户请求。

# 3. Nginx 如何处理服务请求？
Nginx接收某个请求后，首先由listen和server_name指令匹配server模块，然后再匹配server模块中的location，location也就是实际访问请求。
```shell
server {            					
    listen       80；      				
    server_name  localhost;
    location  / {
    root   html;
    index  index.html index.htm;
}
```
http模块支持嵌套多个server，配置代理、缓存、日志定义等大多数功能和第三方模块的配置。例如文件引入、mime-type定义、日志自定义、是否使用sendfile传输文件、连接超时时间、单连接请求数等。

server模块配置虚拟主机的相关参数，表示一个独立的虚拟主机站点。

listen参数提供服务的端口，默认80端口。

server_name参数提供服务的域名主机名称。

location模块配置请求的路由，以及各种页面的处理情况。

root参数站点的根目录，相当于Nginx的安装目录。

index参数默认首页文件，多个文件可用空格分开。

# 4. 为什么 Nginx 性能这么高？
Nginx支持异步非阻塞事件处理机制，使用最新的epoll（Linux 2.6内核）和kqueue（freebsd）网络I/O模型。

目前Linux下能够承受高并发访问的Squid、Memcached都是采用的epoll网络I/O模型。

epoll的接口共有三个函数：
```shell
int epoll_create(int size);
```
创建一个epoll的句柄，size参数是指内核监听的数目。
```shell
int epoll_ctl(int epfd, int op, int fd, struct epoll_event *event);
```
epoll的事件注册函数，select()是在监听事件时告知内核要监听什么类型事件，而该函数是先注册要监听的事件类型。
```shell
int epoll_wait(int epfd, struct epoll_event * events, int maxevents, int timeout);
```
等待事件的产生，类似于select()调用。

# 5. 正向代理和反向代理都有哪些区别？
正向代理（forward proxy）

正向代理是指一个位于客户端和目标服务器之间的服务器（代理服务器），为了从目标服务器获得内容，客户端向代理服务器发送一个请求并指定目标，然后代理服务器向目标服务器转交请求并将获得的内容返回给客户端。

正向代理通俗的来说就是一个用户发送一个请求直接就访问到目标的服务器。类似一个跳板机，代理访问外部资源。

反向代理（Reverse Proxy）

反向代理是指以代理服务器来接收互联网（浏览器等）上的连接请求，然后将请求转发给内部网络上的服务器，并将从服务器上获得结果返回给互联网上请求连接的客户端，此时的代理服务器对外就表现为一个服务器。

反方代理也就是请求都被Nginx接收并按照一定的规则分发给了后端的业务处理服务器进行处理。

# 6. Nginx 都有哪些应用场景？
http服务器

Nginx是一个http服务可以独立提供http服务，也可以做网页静态服务器。

虚拟主机

可以实现在一台服务器虚拟出多个网站，例如个人网站使用的虚拟机。

反向代理，负载均衡

当网站的访问量达到一定程度后，单台服务器不能满足用户的请求时，需要用多台服务器集群可以使用Nginx作为反向代理。并且多台服务器可以平均分担负载，不会出现某台服务器负载高宕机而某台服务器闲置的情况。

配置安全管理

使用Nginx搭建API接口网关，对每个接口服务进行拦截。

# 7. Nginx 目录结构都有哪些？
```shell
├── client_body_temp
├── conf                             # Nginx所有配置文件目录
│   ├── fastcgi.conf                 # fastcgi相关参数配置文件
│   ├── fastcgi.conf.default         # fastcgi.conf原始备份文件
│   ├── fastcgi_params               # fastcgi参数文件
│   ├── fastcgi_params.default       
│   ├── koi-utf
│   ├── koi-win
│   ├── mime.types                   # 媒体类型
│   ├── mime.types.default
│   ├── nginx.conf                   # Nginx主配置文件
│   ├── nginx.conf.default
│   ├── scgi_params                  # scgi相关参数文件
│   ├── scgi_params.default  
│   ├── uwsgi_params                 # uwsgi相关参数文件
│   ├── uwsgi_params.default
│   └── win-utf
├── fastcgi_temp                     # fastcgi临时数据目录
├── html                             # Nginx默认站点目录
│   ├── 50x.html                     # 错误页面
│   └── index.html                   # 默认首页文件
├── logs                             # Nginx日志目录
│   ├── access.log                   # 访问日志文件
│   ├── error.log                    # 错误日志文件
│   └── nginx.pid                    # pid文件，Nginx进程启动后会把所有进程ID号写到此文件
├── proxy_temp                       # 临时目录
├── sbin                             # Nginx命令目录
│   └── nginx                        # Nginx启动命令
├── scgi_temp                        # 临时目录
└── uwsgi_temp                       # 临时目录
```
# 8. Nginx 中如何解决前端跨域问题？
使用Nginx转发请求。

把跨域的接口写成调本域的接口，然后将这些接口转发到真正的请求地址。

# 9. Nginx 中 location指令的作用是什么？
location指令的作用是根据用户请求的URI来执行不同的应用。

location指令也可以理解成为根据用户请求的网站URL进行匹配，匹配成功即进行相关的操作。

# 10. Nginx 中如何禁止某IP不可访问？
Nginx配置禁止192.168.0.1不可访问，修改nginx.conf文件，在server中添加deny ip配置如下：
```shell
server {
    listen       80;
    server_name  localhost;
    charset utf8;
    deny  192.168.0.1;
    
    location / {
        proxy_pass  http://blog.yoodb.com/;
        proxy_set_header Host      $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```
# 11. Nginx 中实现负载均衡的策略都有哪些？
为了避免服务器崩溃，一般会通过负载均衡的方式来分担服务器压力。将对台服务器组成一个集群，当用户访问时，先访问到一个转发服务器，再由转发服务器将访问分发到压力更小的服务器。

Nginx实现负载均衡的策略有五种。

1、轮询（默认方式）

每个请求按时间顺序逐一分配到不同的后端服务器，如果后端某个服务器宕机，则能自动剔除故障系统。
```shell
upstream jxserver {
    server 192.168.0.1;
    server 192.168.0.2;
    server 192.168.0.3;
}
```
2、weight（权重）

weight的值越大分配到的访问概率越高，主要用于后端每台服务器性能不均衡的情况下或在主从的情况下设置不同的权值，达到合理有效的地利用主机资源。
```shell
upstream jxserver {
    server 192.168.0.1 weight=2;
    server 192.168.0.2 weight=3;
    server 192.168.0.3 weight=5;
}
```
3、ip_hash（IP绑定）

每个请求按访问IP的哈希结果分配，使得来自同一个IP的访客固定访问一台后端服务器且可以有效解决动态网页存在的session共享问题。
```shell
upstream jxserver {
    ip_hash;
    server 192.168.0.1:81;
    server 192.168.0.2:82;
    server 192.168.0.3:83;
}
```
4、fair（第三方插件）

Nginx必须安装upstream_fair模块。

fair算法可以根据页面大小和加载时间长短智能地进行负载均衡，响应时间（rt）短的优先分配请求。
```shell
upstream jxserver {
    server 192.168.0.1;
    server 192.168.0.2;
    server 192.168.0.3;
    fair;
}
```
哪个服务器的响应速度快，就将请求分配到那个服务器上。

5、url_hash（第三方插件）

Nginx必须安装hash软件包。

与ip_hash类似，但是按照访问url的hash结果来分配请求，使得每个url定向到同一个后端服务器，主要应用于后端服务器为缓存时的场景。
```shell
upstream jxserver {
    server 192.168.0.1;
    server 192.168.0.2;  
    hash $request_uri;
    hash_method crc32;
}
```
# 12. Nginx 中如何限制并发连接数？
Nginx中'ngx_http_limit_conn_module'模块提供了限制并发连接数的功能，可以使用'limit_conn_zone'指令以及limit_conn执行进行配置。

实例配置如下：
```shell
http {
    limit_conn_zone $binary_remote_addr zone=myip:10m;
    limit_conn_zone $server_name zone=myServerName:10m;
    server {
        location / {
            limit_conn myip 10;
            limit_conn myServerName 100;
            rewrite / http://blog.yoodb.com permanent;
        }
    }
}
```
# 13. Nginx 和 apache 有什么区别？
Nginx轻量级，同样是web服务，比apache 占用更少的内存及资源；

抗并发，nginx处理请求是异步非阻塞的，而apache 则是阻塞型的，在高并发下nginx 能保持低资源低消耗高性能；

高度模块化的设计，编写模块相对简单；最核心的区别在于apache是同步多进程模型，一个连接对应一个进程；

nginx是异步的，多个连接（万级别）可以对应一个进程。

# 14. Nginx 中是如何实现高并发？
一个主进程，多个工作进程，每个工作进程可以处理多个请求，每进来一个request，会有一个worker进程去处理。

但不是全程的处理，处理到可能发生阻塞的地方，比如向上游（后端）服务器转发request，并等待请求返回。

那么，这个处理的worker继续处理其他请求，而一旦上游服务器返回了，就会触发这个事件，worker才会来接手，这个request才会接着往下走。

由于web server的工作性质决定了每个request的大部份生命都是在网络传输中，实际上花费在server机器上的时间片不多。

这是几个进程就解决高并发的秘密所在。即@skoo所说的webserver刚好属于网络io密集型应用，不算是计算密集型。

# 15. Nginx 中如何在 URL 中保留双斜线？
要在URL中保留双斜线，就必须使用merge_slashes参数。

语法:merge_slashes [on/off]

默认值: merge_slashes on

可以在http、server模块中使用。nginx配置信息组织结构如下：

>http       |__server      |     |__location      |     |__location      |      |__server            |__location

# 16. Nginx 如何处理HTTP请求？
Nginx使用反应器模式。

主事件循环等待操作系统发出准备事件的信号，这样数据就可以从套接字读取，在该实例中读取到缓冲区并进行处理。

单个线程可以提供数万个并发连接。

# 17. Nginx 为什么不使用多线程？
Nginx采用单线程来异步非阻塞处理请求（管理员可以配置Nginx主进程的工作进程的数量），不会为每个请求分配cpu和内存资源，节省了大量资源，同时也减少了大量的CPU的上下文切换，所以才使得Nginx支持更高的并发。

# 18. ngx_http_upstream_module 模块有什么作用？
ngx_http_upstream_module用于定义可通过fastcgi传递、proxy传递、uwsgi传递、memcached传递和scgi传递指令来引用的服务器组。

# 19. Nginx 中 Master 和 Worker 进程分别是什么？
Master进程：读取及评估配置和维持。

Worker进程：处理请求。

# 20. Nginx 中什么是 C10K 问题？
在传统的同步阻塞处理模型中，当创建的进程或线程过多时，缓存I/O、内核将数据拷贝到用户进程空间、阻塞，进程/线程上下文切换消耗大。

简而言之，C10K问题就是无法同时处理大量客户端（10,000）的网络套接字。