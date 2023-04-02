
# 1、Nginx反向代理实例1

1. 在 windows 系统的 host 文件进行域名和 ip 对应关系的配置

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/76387fc1f48f44859193822b1f0ed37f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASmllTmlM,size_20,color_FFFFFF,t_70,g_se,x_16)

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/99a2a1ebb90748afb0a7750ce540be54.png)

 2.  在 nginx 进行请求转发的配置（反向代理配置）
    ```shell
            对外开放访问的端口
            firewall-cmd --add-port=8080/tcp --permanent 
            firewall-cmd –reload
        
            查看已经开放的端口号
            firewall-cmd --list-all
    ```
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/7254024439bc44699d4b9bb22fd01ee8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASmllTmlM,size_20,color_FFFFFF,t_70,g_se,x_16)

 3. www.menghao.com访问浏览器测试
 
# 2、Nginx反向代理实例2

>  - 使用 nginx 反向代理，根据访问的路径跳转到不同端口的服务中
>  - nginx 监听端口为 9001
>  - 访问 http://120.48.8.67:9001/edu/ 直接跳转到 127.0.0.1:8080
>  - 访问 http://120.48.8.67:9001/vod/ 直接跳转到 127.0.0.1:8081

![在这里插入图片描述](https://img-blog.csdnimg.cn/b331af352a27423faecf4602a0163b46.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASmllTmlM,size_20,color_FFFFFF,t_70,g_se,x_16)

**开放对外访问的端口号 9001 8080 8081**

# 3、Nginx 负载均衡

 >浏览器地址栏输入地址 http://120.48.8.67/edu/a.html，负载均衡效果，平均 8080和 8081 端口中

![在这里插入图片描述](https://img-blog.csdnimg.cn/f495c8613dd24c69af96eda68b3304d1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASmllTmlM,size_20,color_FFFFFF,t_70,g_se,x_16)

# 4、Nginx  分配服务器策略

 1. **轮询（默认）**

   每个请求按时间顺序逐一分配到不同的后端服务器，如果后端服务器 down 掉，能自动剔除
 
 2. **weight**

 weight 代表权重默认为 1,权重越高被分配的客户端越多

![在这里插入图片描述](https://img-blog.csdnimg.cn/0ef76a443dbe400eb7273a6f642274fd.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASmllTmlM,size_20,color_FFFFFF,t_70,g_se,x_16)

 3. **ip_hash**

 每个请求按访问 ip 的 hash 结果分配，这样每个访客固定访问一个后端服务器

![在这里插入图片描述](https://img-blog.csdnimg.cn/50a8f60caafc481d8d12bdf549dafcb9.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASmllTmlM,size_19,color_FFFFFF,t_70,g_se,x_16)

 4. **fair(第三方)**

 按后端服务器的响应时间来分配请求，响应时间短的优先分配

![在这里插入图片描述](https://img-blog.csdnimg.cn/753ab5df27184a7bab50a8919a510520.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASmllTmlM,size_20,color_FFFFFF,t_70,g_se,x_16)

>***努力改变自己***