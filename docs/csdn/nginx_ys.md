# Windows 使用nginx

## 1.下载Nginx到本地，并解压

官方地址：[http://nginx.org/en/download.html](http://nginx.org/en/download.html)

![nginx官网](https://img-blog.csdnimg.cn/00ab4ae662a24c9b9854a7b44616bfb1.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBASmllTmlM,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)

## 2.启动访问nginx

解压后的文件：

![在这里插入图片描述](https://img-blog.csdnimg.cn/f347cfed804e48cb89bff13121f4f330.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBASmllTmlM,size_20,color_FFFFFF,t_70,g_se,x_16)

进入 **conf** 文件夹，打开 **nginx.conf** 配置文件

![在这里插入图片描述](https://img-blog.csdnimg.cn/fe58d439d8644202a0e19bec0ac5babb.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBASmllTmlM,size_20,color_FFFFFF,t_70,g_se,x_16)

查看nginx默认端口为 **80**

![在这里插入图片描述](https://img-blog.csdnimg.cn/8c427ff800f34f39ba015fba6212060d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBASmllTmlM,size_20,color_FFFFFF,t_70,g_se,x_16)

启动 nginx

![在这里插入图片描述](https://img-blog.csdnimg.cn/b81e9026519f42d2a276e4d83a9e8d05.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBASmllTmlM,size_20,color_FFFFFF,t_70,g_se,x_16)

双击后会一闪而过，所以我们在黑窗口中运行这个执行文件。

键盘Wind+R，输入cmd,确定

![在这里插入图片描述](https://img-blog.csdnimg.cn/30830d4cae9e44f59db1f1a8483f6826.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBASmllTmlM,size_20,color_FFFFFF,t_70,g_se,x_16)

我解压到了桌面，进入到 nginx 解压目录，执行 **nginx.exe**

![在这里插入图片描述](https://img-blog.csdnimg.cn/f9d8d9c378ad4201a0103c3d8c794e4c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBASmllTmlM,size_20,color_FFFFFF,t_70,g_se,x_16)

在浏览器直接访问 **[http://localhost](http://localhost/)**

![在这里插入图片描述](https://img-blog.csdnimg.cn/9c31036211234077a8aad632ab22b435.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBASmllTmlM,size_20,color_FFFFFF,t_70,g_se,x_16)

出现上图，表示nginx启动成功了，这就是nginx启动。

## 3.配置一个简单的映射本地文件

修改 nginx.conf 配置文件，主要改server{}里的配置

![在这里插入图片描述](https://img-blog.csdnimg.cn/e76be76bc06a47f5b0798bcea0b46ba1.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBASmllTmlM,size_20,color_FFFFFF,t_70,g_se,x_16)

因为修改了nginx配置文件，我们要重新加载下配置文件 **nginx -s reload**

![在这里插入图片描述](https://img-blog.csdnimg.cn/bf06b353075b49c1b0844589e8059553.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBASmllTmlM,size_20,color_FFFFFF,t_70,g_se,x_16)

本地目录中的文件：

![在这里插入图片描述](https://img-blog.csdnimg.cn/6b5d0cb50e744d2eb62a5180f748516c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBASmllTmlM,size_20,color_FFFFFF,t_70,g_se,x_16)


访问 [http://localhost:8081/1.jpg](http://localhost:8081/1.jpg)

![在这里插入图片描述](https://img-blog.csdnimg.cn/dfa66ad3578849338c1e4008c65c3899.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBASmllTmlM,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/2cb72f7b86fe43588056e73c45d4c017.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBASmllTmlM,size_20,color_FFFFFF,t_70,g_se,x_16)

这样我们就访问到了本地目录中的文件。

>***努力改变自己***