
> 解决了运行环境和配置问题的软件容器，方便做持续集成并有助于整体发布的容器虚拟化技术。

## Docker官网

> docker官网：[http://www.docker.com](https://hub.docker.com/)

> Hub官网: [https://hub.docker.com/](https://hub.docker.com/)

## Docker安装

### 前提条件

>目前，CentOS 仅发行版本中的内核支持 Docker。Docker 运行在CentOS 7 (64-bit)上，
要求系统为64位、Linux系统内核版本为 3.8以上，这里选用CentOS 8

### CentOS8安装Docker

1. **确定你是CentOS7及以上版本**
	
	```shell
	cat /etc/redhat-release
	```

	![在这里插入图片描述](https://img-blog.csdnimg.cn/3d4780e7dad547d3a5fb7de1de9712d7.png)

 2. **卸载旧版本**
	```shell
	yum remove docker-ce docker-ce-cli containerd.io docker-compose-plugin
	rm -rf /var/lib/docker
	rm -rf /var/lib/containerd
	```

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/a0bd135dcec64b869cdeb3f47e327245.png)
 
3. **yum安装gcc相关**

	```shell
	yum -y install gcc
	yum -y install gcc-c++
	```
4. **安装需要的软件包(官网要求)**
	```shell
	yum install -y yum-utils
	```
5. **设置stable镜像仓库(阿里云)**
	```shell
	yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
	```
6. **更新yum软件包索引**
	```shell
	yum makecache fast
	```
7.  **安装DOCKER CE(官网要求)**
	```shell
	yum -y install docker-ce docker-ce-cli containerd.io
	```
8. **启动Docker**
	```shell
	systemctl start docker
	```
9. **测试**
	```shell
	docker version
	docker run hello-world
	```
10. **卸载**
	```shell
	systemctl stop docker 
	yum remove docker-ce docker-ce-cli containerd.io
	rm -rf /var/lib/docker
	rm -rf /var/lib/containerd
	```
	![在这里插入图片描述](https://img-blog.csdnimg.cn/a94e6130c1ea4e66a6ff808901f7975b.png)
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/65dbe489abcc4f73939a177a7967ea0b.png)
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/c662ec15b5c14da1b3c480fa1bcdd64c.png)

## 阿里云镜像加速

> 阿里云官网 **[https://promotion.aliyun.com/ntms/act/kubernetes.html](https://promotion.aliyun.com/ntms/act/kubernetes.html)**

  ![登陆阿里云开发者平台](https://img-blog.csdnimg.cn/ae604487bc8341338a128baa9d5000e9.png) ![点击控制台](https://img-blog.csdnimg.cn/434dcc26976f4a04a05ebd852c3ae4ad.png)
  ![选择容器镜像服务](https://img-blog.csdnimg.cn/d63caf0c8f46472ab7f21f6edd2f6686.png)
  ![获取加速器地址](https://img-blog.csdnimg.cn/ed12cec937ac45ad8a234262d92a0b4f.png)

 1. 粘贴脚本直接执行

	```shell
	mkdir -p /etc/docker
	tee /etc/docker/daemon.json <<-'EOF'
	{
	  "registry-mirrors": ["https://｛自已的编码｝.mirror.aliyuncs.com"]
	}
	EOF
	```
 1. 重启服务器
	```shell
	systemctl daemon-reload
	systemctl restart docker
	```
	![在这里插入图片描述](https://img-blog.csdnimg.cn/8911b9b1346a4631b471292e64c3d604.png)

# Docker常用命令

## 帮助启动类命令

1. 启动docker
	```shell
	systemctl start docker
	```
 2. 停止docker
	```shell
	systemctl stop docker
	```
3. 重启docker
	```shell
	systemctl restart docker
	```
4. 查看docker状态
	```shell
	systemctl status docker
	```
5. 开机启动
	```shell
	systemctl enable docker
	```
6. 查看docker概要信息
	```shell
	docker info
	```
7. 查看docker总体帮助文档
	```shell
	docker --help
	```
8. 查看docker命令帮助文档
	```shell
	docker 具体命令 --help
	例:docker ps --help
	```

## 镜像命令

1. 列出本地主机上的镜像
 OPTIONS说明：
	 - **-a :列出本地所有的镜像（含历史映像层）**
	 -  **-q :只显示镜像ID**

	```shell
	docker images
	```
	![在这里插入图片描述](https://img-blog.csdnimg.cn/1b27dbd469c3441298d85368a001a63b.png)
	>REPOSITORY：**表示镜像的仓库源**
	TAG：**镜像的标签版本号**
	IMAGE ID：**镜像ID**
	CREATED：**镜像创建时间**
	SIZE：**镜像大小**
	
	> 同一仓库源可以有多个 TAG版本，代表这个仓库源的不同个版本，我们使用 REPOSITORY:TAG 来定义不同的镜像。
		如果你不指定一个镜像的版本标签，例如你只使用 ubuntu，docker 将默认使用 ubuntu:latest 镜像
 
 2. 查询镜像
	OPTIONS说明：
	 - **--limit : 只列出N个镜像，默认25个**
	 - **docker search --limit 5 redis**\
	```shell
	docker search redis
    ```
	 ![在这里插入图片描述](https://img-blog.csdnimg.cn/decd3b5afd2e41568157515592c0a7ba.png)
3. 下载镜像
	>**没有TAG就是最新版
	等价于
	docker pull 镜像名字:latest**
	```shell
	# 下载redis最新版
	docker pull redis
	# 下载redis 6.0.8版本
	docker pull redis:6.0.8
	```
	![在这里插入图片描述](https://img-blog.csdnimg.cn/6b4ffaaaec3342288243f3710a8d08bd.png)

4. 查看镜像/容器/数据卷所占的空间
	```shell
	docker system df 
	```
	![在这里插入图片描述](https://img-blog.csdnimg.cn/56c9af7237e2457e970c5227ad105c38.png)

5.  删除镜像
	```shell
	# 删除单个镜像
	docker rmi -f 镜像ID
	# 删除多个镜像
	docker rmi -f 镜像名1:TAG 镜像名2:TAG
	# 删除全部(慎用)
	docker rmi -f ${docker images -qa}
	```
	![在这里插入图片描述](https://img-blog.csdnimg.cn/5035bcfafc0d479586fe74037bf70139.png)

##  容器命令

> 	**有镜像才能创建容器，
> 	这是根本前提(下载一个CentOS或者ubuntu镜像演示)**

1. 新建+启动容器 
	
	> **docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
		OPTIONS说明（常用）：有些是一个减号，有些是两个减号
		--name="容器新名字"       为容器指定一个名称；
		-d: 后台运行容器并返回容器ID，也即启动守护式容器(后台运行)；
		-i：以交互模式运行容器，通常与 -t 同时使用；
		-t：为容器重新分配一个伪输入终端，通常与 -i 同时使用；
		也即启动交互式容器(前台有伪终端，等待交互)；
		-P: 随机端口映射，大写P
		-p: 指定端口映射，小写p**
	```shell
	docker run -it ubuntu /bin/bash
	```
	![在这里插入图片描述](https://img-blog.csdnimg.cn/f31c2e5e3051472a85411d4a367c8bc0.png)

2. 列出当前所有**正在运行**的容器
	>OPTIONS说明（常用）：
	-a :列出当前所有正在运行的容器+历史上运行过的
	-l :显示最近创建的容器。
	-n：显示最近n个创建的容器。
	-q :静默模式，只显示容器编号。
	```shell
	docker ps
	```
3. 退出容器
	```shell
	# run进去容器,exit退出,容器停止
	# run进去容器，ctrl+p+q退出，容器不停止
	```
	![在这里插入图片描述](https://img-blog.csdnimg.cn/d32209b2ffec4dbb8da3e9f542b6a47c.png)

4. 启动已停止运行的容器
	```shell
	docker start 容器ID或容器名
	```
	![在这里插入图片描述](https://img-blog.csdnimg.cn/f2765fd0212140bf9906b85bcd740e3e.png)

5. 重启容器
	```shell
	docker restart 容器ID或容器名
	```
6. 停止容器
	```shell
	docker stop 容器ID或容器名
	```
	![在这里插入图片描述](https://img-blog.csdnimg.cn/b0f926e52cdc4834818cedea5fb4e4a0.png)

7. 强制停止容器
	```shell
	docker kill 容器ID或容器名
	```
8. 删除已停止的容器
	```shell
	docker rm 容器ID
	# 一次性删除多个容器实例
	docker rm -f $(docker ps -a -q)
	docker ps -a -q | xargs docker rm
	```
9. 前台交互式启动
	```shell
	docker run -it redis:6.0.8
	```
10. 后台守护式启动 
	```shell
	docker run -d redis:6.0.8
	```
11. 查看容器日志
	```shell
	docker logs 容器ID
	```
12. 查看容器内运行的进程
	```shell
	docker run -d redis:6.0.8
	```
13. 查看容器内部细节
	```shell
	docker top 容器ID
	```
14. 进入正在运行的容器并以命令行交互
	```shell
	docker inspect 容器ID
	```
15. 从容器内拷贝文件到主机
	```shell
	docker cp  容器ID:容器内路径 目的主机路径
	```
	![在这里插入图片描述](https://img-blog.csdnimg.cn/0b4dc9b41536488da706c8df6b4c89c5.png)

16. 导入
	> export 导出容器的内容留作为一个tar归档文件[对应import命令]
	```shell
	docker export 容器ID > 文件名.tar
	```
	![在这里插入图片描述](https://img-blog.csdnimg.cn/7d9c28f355d3493193639749f4496cbe.png)

17. 导出
	> import 从tar包中的内容创建一个新的文件系统再导入为镜像[对应export]
	```shell
	docker run -d redis:6.0.8
	```
	![在这里插入图片描述](https://img-blog.csdnimg.cn/ba62431684de48ad88d5abeb743ac81e.png)

# 总结常用命令

> **attach**    Attach to a running container                 # **当前 shell 下 attach 连接指定运行镜像**
> 
> **build**     Build an image from a Dockerfile              # **通过 Dockerfile 定制镜像**
> 
> **commit**    Create a new image from a container changes   # **提交当前容器为新的镜像**
> 
> **cp**        Copy files/folders from the containers filesystem to the host path   # **从容器中拷贝指定文件或者目录到宿主机中**
> 
> **create**    Create a new container                        # **创建一个新的容器，同 run，但不启动容器**
> 
> **diff**      Inspect changes on a container's filesystem   # **查看 docker 容器变化**
> 
> **events**    Get real time events from the server          # **从 docker 服务获取容器实时事件**
> 
> **exec**      Run a command in an existing container        # **在已存在的容器上运行命令**
> 
> **export**   Stream the contents of a container as a tar archive   # **导出容器的内容流作为一个 tar 归档文件[对应 import ]**
> 
> **history**   Show the history of an image                  # **展示一个镜像形成历史**
> 
> **images**    List images                                   # **列出系统当前镜像**
> 
> **import**    Create a new filesystem image from the contents of a tarball # **从tar包中的内容创建一个新的文件系统映像[对应export]**
> 
> **info**      Display system-wide information               # **显示系统相关信息**
> 
> **inspect**   Return low-level information on a container   # **查看容器详细信息**
> 
> **kill**      Kill a running container                      # **kill 指定 docker 容器**
> 
> **load**      Load an image from a tar archive              # **从一个 tar 包中加载一个镜像[对应 save]**
> 
> **login**     Register or Login to the docker registry server    # **注册或者登陆一个 docker 源服务器**
> 
> **logout**    Log out from a Docker registry server          # **从当前 Docker registry 退出**
> 
> **logs**      Fetch the logs of a container                 # **输出当前容器日志信息**
> 
> **port**      Lookup the public-facing port which is NAT-ed to PRIVATE_PORT    # **查看映射端口对应的容器内部源端口**
> 
> **pause**     Pause all processes within a container        # **暂停容器**
> 
> **ps**        List containers                               # **列出容器列表**
> 
> **pull**     Pull an image or a repository from the docker registry server   # **从docker镜像源服务器拉取指定镜像或者库镜像**
> 
> **push**      Push an image or a repository to the docker registry server    # **推送指定镜像或者库镜像至docker源服务器**
> 
> **restart**   Restart a running container                   # **重启运行的容器**
> 
> **rm**        Remove one or more containers                 # **移除一个或者多个容器**
> 
> **rmi**      Remove one or more images       # **移除一个或多个镜像[无容器使用该镜像才可删除，否则需删除相关容器才可继续或 -f 强制删除]**
> 
> **run**       Run a command in a new container              # **创建一个新的容器并运行一个命令**
> 
> **save**      Save an image to a tar archive                # **保存一个镜像为一个 tar 包[对应 load]**
> 
> **search**    Search for an image on the Docker Hub         # **在 docker hub 中搜索镜像**
> 
> **start**     Start a stopped containers                    # **启动容器**
> 
> **stop**      Stop a running containers                     # **停止容器**
> 
> **tag**       Tag an image into a repository                # **给源中镜像打标签**
> 
> **top**       Lookup the running processes of a container   # **查看容器中运行的进程信息**
> 
> **unpause**   Unpause a paused container                    # **取消暂停容器**
> 
> **version**   Show the docker version information           # **查看 docker 版本号**
> 
> **wait**      Block until a container stops, then print its exit code   # **截取容器停止时的退出状态值**

# Docker容器数据卷
>Docker容器数据卷有点类似我们Redis里面的rdb和aof文件,将docker容器内的数据保存进宿主机的磁盘中
>运行一个带有容器卷存储功能的容器实例
> **docker run -it --privileged=true -v /宿主机绝对路径目录:/容器内目录      镜像名**

将运用与运行的环境打包镜像，run后形成容器实例运行 ，但是我们对数据的要求希望是持久化的
 
Docker容器产生的数据，如果不备份，那么当容器实例删除后，容器内的数据自然也就没有了。
为了能保存数据在docker中我们使用卷。
 
特点：
1：数据卷可在容器之间共享或重用数据
2：卷中的更改可以直接实时生效，爽
3：数据卷中的更改不会包含在镜像的更新中
4：数据卷的生命周期一直持续到没有容器使用它为止
 
## 宿主vs容器之间映射添加容器卷
```shell
 docker run -it --privileged=true -v /宿主机绝对路径目录:/容器内目录      镜像名
```
1. 启动ubuntu镜像

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/31d888ebd9384db2b7519c8ef21ebd82.png)
2. 查看数据卷是否挂载成功

	![查看数据卷是否挂载成功](https://img-blog.csdnimg.cn/2b6322cf5f334a6480979c2cd2e522a1.png)
3. 容器和宿主机共享数据
	**宿主机**

	![在这里插入图片描述](https://img-blog.csdnimg.cn/c8115ecf1aa145149a013f63e4c3c844.png)
	**容器中**

	![在这里插入图片描述](https://img-blog.csdnimg.cn/839fbffae24149b295089496512e8351.png)

## 读写规则映射添加说明
1. 读写（**默认**）
	```shell
	 docker run -it --privileged=true -v /宿主机绝对路径目录:/容器内目录:rw      镜像名
	```
2. 只读(**容器实例内部被限制，只能读取不能写**)
	```shell
	 docker run -it --privileged=true -v /宿主机绝对路径目录:/容器内目录:ro      镜像名
	```

## 卷的继承和共享
```shell
	docker run -it --privileged=true --volumes-from u1 ubuntu /bin/bash
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/c006769a026d485fa90318d797b0e2e5.png)

>***努力改变自己***