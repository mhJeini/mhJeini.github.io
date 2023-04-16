# 1. Nginx 中 stub_status 和 sub_filter 指令有什么作用？
stub_status指令：该指令用于了解Nginx当前状态的当前状态，如当前的活动连接，接受和处理当前读/写/等待连接的总数。

sub_filter指令：它用于搜索和替换响应中的内容，并快速修复陈旧的数据。

# 2. Nginx 中如何获得当前的时间？
Nginx中要获得当前时间，必须使用SSI模块、$date_gmt和$date_local的变量。

Proxy_set_header THE-TIME $date_gmt;
# 3. Nginx 中有可能将错误替换为 502、503 错误吗？
502 =错误网关

503 =服务器超载

Nginx中是有可能将错误替换为502、503错误，前提是必须确保fastcgi_intercept_errors被设置为ON，并使用错误页面指令。
```shell
location / {
    fastcgi_pass 127.0.01:8081;
    fastcgi_intercept_errors on;
    error_page 502 =503/error_page.html;
    #…
}
```
# 4. Nginx 中有多个 server{} 时先匹配哪个？
如果请求同时命中多个server{}，则是从上到下匹配。

如果是分布在多个配置文件中，则通过目录中的摆放顺序，在前面的文件优先被读取。

# 5. Nginx 中 location 匹配优先级顺序？
在开始处理一个http请求时，nginx会取出header头中的host，与nginx.conf中每个server的server_name进行匹配，以此决定到底由哪一个server块来处理这个请求。

location匹配优先级为= > ^ > ~* = ~。

=：表示进行普通字符精确匹配，也就是完全匹配。

^~：表示普通字符匹配，使用前缀匹配。

~*：表示执行一个正则匹配（不区分大小写）。

~：表示执行一个正则匹配（区分大小写）。

server_name与host匹配优先级如下：

1、完全匹配

2、通配符在前的，如*.test.com

3、在后的，如www.test.*

4、正则匹配，如~^.www.test.com$

如果都无法匹配时

1、优先选择listen配置项后有default或default_server。

2、找到匹配listen端口的第一个server块。

# 6. Nginx 中常见状态码有哪些？
状态码413

request entity too large：默认nginx会限制用户长传文件大小，如果想修改限制的文件大小可以使用client_max_body_size修改。

状态码502

bad gateway：一般为后端服务无响应。比如反向代理到127.0.0.1，如果127.0.0.1没有响应则nginx会返给客户端502。

状态码504

gateway time-out：一般为后端服务执行超时，nginx默认等待时间为60秒。

# 7. Nginx 中如何配置实现高可用性？
TODO

# 8. Nginx 服务器解释 -s 参数有什么作用？
用于运行Nginx -s参数的可执行文件。

# 9. 为什么 Nginx 要做动、静分离？
在我们的软件开发中，有些请求是需要后台处理的（如：.jsp,.do等等），有些请求是不需要经过后台处理的（如：css、html、jpg、js等等），这些不需要经过后台处理的文件称为静态文件，否则动态文件。

因此后台处理忽略静态文件，但是如果直接忽略静态文件的话，后台的请求次数就明显增多了。在我们对资源的响应速度有要求的时候，应该使用这种动静分离的策略去解决动、静分离将网站静态资源（HTML，JavaScript，CSS等）与后台应用分开部署，提高用户访问静态代码的速度，降低对后台应用访问。这里将静态资源放到nginx中，动态资源转发到tomcat服务器中,毕竟Tomcat的优势是处理动态请求。

# 10. Nginx 中产生 502 错误可能原因？
1、FastCGI进程是否已经启动

2、FastCGI worker进程数是否不够

3、FastCGI执行时间过长

1、fastcgi_connect_timeout 300;

2、fastcgi_send_timeout 300;

3、fastcgi_read_timeout 300;

FastCGI Buffer不够

1、nginx和apache一样，有前端缓冲限制，可以调整缓冲参数

2、fastcgi_buffer_size 32k;

3、fastcgi_buffers 8 32k;

Proxy Buffer不够

1、如果你用了Proxying，调整

2、proxy_buffer_size 16k;

3、proxy_buffers 4 16k;

php脚本执行时间过长

将php-fpm.conf的0s的0s改成一个时间。

# 11. Nginx 中如何判断 IP 是否可访问？
假设访问ip地址为192.168.0.10，否则返回403无权限访问：
```shell
if  ($remote_addr = 192.168.0.10) {
  return 403;
}
```