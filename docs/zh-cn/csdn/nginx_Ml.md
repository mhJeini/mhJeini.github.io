
# 1. 查看 Nginx 安装位置
 `whereis nginx`

![在这里插入图片描述](https://img-blog.csdnimg.cn/fc53f42724974c1592f293475935250e.png)
# 2. Nginx 启动
 1. 进入nginx sbini目录 **`cd /usr/local/nginx/sbin`**
 2. 启动命令1 **`./nginx`**

![在这里插入图片描述](https://img-blog.csdnimg.cn/51830809a1f749e683169d973e8fad7e.png)

 3. 启动命令2指定配置文件： **`/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx_ftp.conf`**

![在这里插入图片描述](https://img-blog.csdnimg.cn/18106bdeb2704652bb7271d54b58515e.png)

# 3.查看 Nginx 是否启动
 - **`ps -ef | grep nginx`**

![在这里插入图片描述](https://img-blog.csdnimg.cn/1073d63f54e844c9920b3130b48e2a12.png)

# 4. 关闭 Nginx
 1. 从容停止Nginx **`kill -quit nginx进程号`**
 2. 快速停止Nginx **`kill -term nginx进程号`**
 3. 强制停止Nginx **`pkill -9 nginx`**
 
![在这里插入图片描述](https://img-blog.csdnimg.cn/70e6b8a96b8e4bee94dd125775060762.png)

# 5. 修改配置文件重新加载 Nginx
 1. **`/usr/local/nginx/sbin/nginx -s reload`**
 2. **`/usr/local/nginx/sbin/nginx -t -c /usr/local/nginx/conf/nginx_ftp.conf`**
 3. **`kill -hup nginx进程号`**

 
>***努力改变自己***