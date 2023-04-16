# 1. Docker 中如何查看输出和日志信息？
当要查看一个docker容器的日志时，可以直接使用

```shell
docker logs 容器名字或者ID。
```

如果需要找其中包含某些内容（如xxx）的所有行，可以使用
```shell
docker logs 容器名字或者 ID 2>&1 | grep xxx
```
这里的2>&1代表把标准错误（文件描述符2）重定向（>）到标准输出（文件描述符 1）的位置（&）。

如果需要导出日志文件，可以使用
```shell
# grep 的 -i 表示不区分大小写
docker inspect 容器名字或者 ID | grep -i logpath
```
# 2. Docker 如何临时退出正在交互容器终端？
使用快捷键'Ctrl+p'，后'Ctrl+q'，如果按'Ctrl+c'会使容器内的应用进程终止，进而会使容器终止。

# 3. Docker 中如何控制容器占用系统资源情况？
使用docker create命令创建容器或使用docker run创建并运行容器的时候，可以使用'-c|-spu-shares[=0]'参数来调整同期使用SPU的权重，使用-m|-memory参数来调整容器使用内存的大小。

# 4. Docker 中仓库、注册服务器、注册索引有什么联系？
仓库（Repository）是存放一组关联镜像的集合，比如同一个应用的不同版本的镜像。

注册服务器（Registry）是存放实际的镜像的地方。

注册索引（Index）则负责维护用户的账号、权限、搜索、标签等管理。

注册服务器利用注册索引来实现认证等管理。

# 5. Docker 和 LXC 有什么区别？
LXC是在Linux上相关技术实现的容器，docker则在如下的几个方面进行了改进：

1、移植性：通过抽象容器配置，容器可以实现一个平台移植到另一个平台。

2、镜像系统：基于AUFS的镜像系统为容器的分发带来了很多的便利，通是共同的镜像层只需要存储一份，实现高效率的存储。

3、版本管理：类似于GIT的版本管理理念，用户可以更方便的创建、管理镜像文件。

4、仓库系统:仓库系统大大降低了镜像的分发和管理的成本。

5、周边工具：各种现有的工具（配置管理、云平台）对docker的支持，以及基于docker的pass、Cl等系统，让docker的应用更加方便和多样。

# 6. Docker 和 Vagrant 有什么区别？
Docker和Vagrant的定位完全不同。

Vagrant类似于Boot2Docker（一款运行Docker的最小内核），是一套虚拟机的管理环境，Vagrant可以在多种系统上和虚拟机软件中运行，可以在Windows、Mac等非Linux平台上为Docker支持，自身具有较好的包装性和移植性。

原生Docker自身只能运行在Linux平台上，但启动和运行的性能比虚拟机要快，往往更适合快速开发和部署应用的场景。

# 7. Docker 环境如何迁移到另外宿主机？
停止docker服务，之后将整个docker存储文件复制到另外一台宿主机上，然后调整另外一台宿主机的配置即可。

# 8. DockerFile中 COPY 和 ADD 命令有什么区别？
COPY指令和ADD指令的唯一区别在于是否支持从远程URL获取资源。

COPY指令只能从执行docker build所在的主机上读取资源并复制到镜像中，而ADD指令还支持通过URL从远程服务器读取资源并复制到镜像中。

# 9. Docker 中什么是 Container？
container即容器。可以把每个container看做是一个独立的主机。

container的创建通常有一个image作为其模板。类比成虚拟机的话可以理解为image就是虚拟机的镜像，而container就是一个个正在运行的虚拟机。一个虚拟机镜像可以创建出多个运行的虚拟主机且相互独立。

注意：container一旦创建如果没有用rm命令移除，将会一直存在，因此在不使用的情况下需要手动删除。

# 10. Docker 容器中如何启动 Nginx 服务？
启动nginx服务，端口映射随机，并挂载本地文件目录到容器html的命令。
```shell
docker run -d -P --name nginx2 -v /home/nginx:/usr/share/nginx/html nginx
```
# 11. Docker 中什么是 Image？
image即镜像。image相当于container的模板，container创建后里面有什么软件完全取决于它使用什么image。

image可以通过container创建，相当于把此时container的状态保存成快照，也可以通过Dockerfile来创建。其中通过Dockerfile创建的方法能让环境配置和代码一起被版本库一起管理。

# 12. Docker 中什么是 Registry？
registry即存放镜像的仓库。只要能连接到registry每个人都可以很方便地通过pull命令从仓库中获取镜像。

registry在github上有两份代码：老代码库和新代码库。老代码是采用python编写的，存在pull和push的性能问题，出到0.9.1版本之后就标志为deprecated，不再继续开发。从2.0版本开始就到在新代码库进行开发，新代码库是采用go语言编写，修改了镜像id的生成算法、registry上镜像的保存结构，大大优化了pull和push镜像的效率。

官方在Docker hub上提供了registry的镜像，可以直接使用registry镜像来构建一个容器，搭建自己的私有仓库服务。

docker默认使用的仓库是docker hub，国内可以使用DaoCloud来建立Mirror连接到docker hub，进而加快获取image的速度。

# 13. 什么是Docker Hub？
Docker hub是一个基于云的注册表服务，允许您链接到代码存储库，构建镜像并测试它们，存储手动推送的镜像以及指向Docker云的链接，以便可以将镜像部署到主机。它为整个开发流程中的容器镜像发现，分发和变更管理，用户和团队协作以及工作流自动化提供了集中资源。

# 14. 如何备份系统中所有的镜像？
备份镜像列表可以使用命令：
```shell
docker images | awk 'NR>l{prin七$1":"$2} ' | sort > images.list
```
导出所有镜像为当前目录下文件， 可以使用如下命令：
```shell
while read img; do
echo $img
file="${img/\//-}"
sudo docker save --output $file. tar $img
done< images.list
```
将本地镜像文件导入为Docker镜像：
```shell
while read img; do
echo $img
file="${img/\//-}"
docker load< $file.tar
done< images.list
```
# 15. 如何批量清理临时镜像文件？
可以使用'docker rmi $(docker images -q -f dangling = rue)'命令。

# 16. 如何删除所有本地的镜像？
可以使用'docker rmi -f $(docker images -q)'命令。

# 17. 如何清理 Docker 系统中的无用数据？
可以使用'docker system prune --volumes -f'命令， 这个命令会自动清理处于停止状态的容器、 无用的网络和挂载卷、 临时镜像和创建镜像缓存。

# 18. 容器退出后， 通过 docker ps 命令查看不到，数据会丢失么？
容器退出后会处于终止（exited）状态， 此时可以通过'docker ps -a'查看。

其中的数据也不会丢失，还可以通过'docker [container] start'命令来启动它。只有删除掉容器才会清除所有数据。

# 19. 如何停止所有正在运行的容器？
可以使用'docker [container] stop $(docker ps -q)'命令。

# 20. 如何获取某个容器的 PIO 信息？
可以使用'docker [container] inspect --format ' {{ . State.Pid }} '< CONTAINER ID or NAME>'命令。