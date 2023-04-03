## Docker镜像,发布到阿里云或私有库

> Docker镜像层都是只读的，容器层是可写的 
> 当容器启动时，一个新的可写层被加载到镜像的顶部。
> 这一层通常被称作“容器层”，“容器层”之下的都叫“镜像层”。

### Docker镜像commit操作案例

> 	docker commit提交容器副本使之成为一个新的镜像 	
> **docker commit -m="提交的描述信息" -a="作者" 容器ID 要创建的目标镜像名:[标签名]**

#### 案例演示ubuntu安装vim

1. 从Hub上下载ubuntu镜像到本地并成功运行

2. 原始的默认Ubuntu镜像是不带着vim命令的

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/6cf0ee6c9a5c418d8eeb8b19d8f13dc5.png)
	
3. 外网连通的情况下，安装vim
	```powershell
	# docker容器内执行上述两条命令：
	apt-get update
	apt-get -y install vim
	```

	![在这里插入图片描述](https://img-blog.csdnimg.cn/24c86f74158b4bfb932f968c6c537843.png)


4. 安装完成后，commit我们自己的新镜像

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/7c444716ef8148a79700d55e2bc3d9f4.png)

## 本地镜像发布到阿里云

### 本地镜像
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/924790b31999468eaa8658464ab3aa49.png)

### 阿里云开发者平台

>https://promotion.aliyun.com/ntms/act/kubernetes.html

### 创建仓库镜像

  选择控制台,进入容器镜像服务,选择个人实例
![阿里云首页](https://img-blog.csdnimg.cn/fe08a2b878674bdfa42068def00a632f.png)
![登录后点击控制台](https://img-blog.csdnimg.cn/1e33569cab3d483b8e4be17490ccaef5.png)
![选择弹性计算中的容器镜像服务](https://img-blog.csdnimg.cn/63a4253399634e6bbe6d8326de32f775.png)
![实例列表中点击个人实例里的个人版](https://img-blog.csdnimg.cn/fc5643d893c9404193c0b6a75639618d.png)
![创建命名空间](https://img-blog.csdnimg.cn/ce3a46c2e8dd41c7946cfc589f47d727.png)
![创建镜像空间](https://img-blog.csdnimg.cn/73431130ef434570853a82ad730ddfd2.png)
![选择本地仓库创建](https://img-blog.csdnimg.cn/45f1260f36154e8d82fd239fcd80679f.png)
![进入管理界面](https://img-blog.csdnimg.cn/cab6cf4fb6874ed1a6f042ba60f9c330.png)

### 将镜像推送到阿里云

![按照官网步骤来](https://img-blog.csdnimg.cn/5e528289e4894b66a6b4f4a7f1318135.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/f26afbf6c09d4d7d91aae6a8ced131c3.png)

### 将阿里云上的镜像下载到本地

 ![将阿里云上的镜像下载到本地](https://img-blog.csdnimg.cn/a107705522f0443b96c49f11fa3d660d.png)

## 本地镜像发布到私有库

1. 下载镜像Docker Registry
	```powershell
	docker pull registry
	```

	![下载镜像Docker Registry](https://img-blog.csdnimg.cn/7dc2914044cf49108234cbebe7c59df4.png)

2. 运行私有库Registry，相当于本地有个私有Docker hub
	```powershell
	docker run -d -p 5000:5000  -v /zzyyuse/myregistry/:/tmp/registry --privileged=true registry
	```

	![在这里插入图片描述](https://img-blog.csdnimg.cn/4d625952bda14b58bcc98b0b50f56af5.png)

3. 案例演示创建一个新镜像，ubuntu安装ifconfig命令
	
	1. 原始ubuntu镜像中是没有ifconfig命令的

	![原始ubuntu镜像中是没有ifconfig命令的](https://img-blog.csdnimg.cn/aee4e0eea82e470ba847ff1c521d995b.png)

	2. 安装ifconfig命令
		```powershell
		apt-get update
		apt-get install net-tools
		```

		![安装ifconfig命令](https://img-blog.csdnimg.cn/ca35e812621d40fcb4e6bf00994dfdc5.png)
	3. 安装成功后再次执行ifconfig
        
        ![安装成功后再次执行ifconfig](https://img-blog.csdnimg.cn/9f153b4bc87c446e8475f3c46a0ddf39.png)
	4. 在容器外commit我们自己的新镜像
		```powershell
		docker commit -m="提交的描述信息" -a="作者" 容器ID 要创建的目标镜像名:[标签名]
		```
		
        ![在容器外commit我们自己的新镜像](https://img-blog.csdnimg.cn/9239b9802f15448baeffcd01f5eb15b1.png)


4. curl验证私服库上有什么镜像
	```powershell 
	curl -XGET http:你的IP:5000/v2/_catalog
	```
	
    ![curl验证私服库上有什么镜像](https://img-blog.csdnimg.cn/807060e50a114a1093460faf08940301.png)
5. 将新镜像 menghaoubuntu:1.0 修改符合私服规范的Tag
	```powershell
	docker   tag   镜像:Tag   Host:Port/Repository:Tag
	```
	
    ![将新镜像 menghaoubuntu:1.0 修改符合私服规范的Tag](https://img-blog.csdnimg.cn/7ef631b895cc46f5944b0fa2007131df.png)

6. 修改配置文件使之支持http
	**2个配置中间有逗号,是json格式的!!!
	2个配置中间有逗号,是json格式的!!!
	2个配置中间有逗号,是json格式的!!!**

	```powershell
	"insecure-registries": ["xxx.x.xx.xxx:5000"]
	```

	![修改配置文件使之支持http](https://img-blog.csdnimg.cn/9d58baf6faaa4bf0acc9ec24f7d57c65.png)
	**修改完后建议重启docker
	systemctl restart docker**
	
7. push推送到私服库

	![push推送到私服库](https://img-blog.csdnimg.cn/b93be10a988c481f9a165be3a8a03595.png)

8. curl验证私服库上有什么镜像

	![curl验证私服库上有什么镜像](https://img-blog.csdnimg.cn/386648b946924acca4098f10592f1d07.png)

9. pull到本地并运行

	![在这里插入图片描述](https://img-blog.csdnimg.cn/e7cf4d1e85084c43b6e0b493d221b657.png)

>***努力改变自己***