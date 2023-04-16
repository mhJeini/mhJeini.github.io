# 1. 如何临时退出一个正在交互的容器的终端， 而不终止它？
按'ctrl-p Ctrl-q'。

如果按'ctrl-c'往往会让容器内应用进程终止， 进而会终止容器。

# 2. 如何控制容器占用系统资源（CPU、内存）的份额？
使用'docker [container] create'命令创建容器或使用'docker [container] run'创建并启动容器的时候，可以使用'-c | - cpu -shares[=O]'参数来调整容器使用CPU的权重；使用'-ml-memory[=MEMORY]'参数来调整容器使用内存的大小。

# 3. Docker 安全吗？
Docker利用Linux内核中很多安全特性来保证不同容器之间的隔离，并且通过签名机制来对镜像进行验证。

大量生产环境的部署证明，Docker虽然隔离性无法与虚拟机相比，但仍然具有极高的安全性。

# 4. 开发环境中 Docker 与 Vagrant 该如何选择？
Docker不是虚拟机，而是进程隔离，对于资源的消耗很少，单一开发环境下Vagrant是虚拟机上的封装，虚拟机本身会消耗资源。

# 5. Docker的配置文件放在什么位置，如何修改配置？
Ubuntu系统下Docker的配置文件是/etc/default/docker，CentOS系统配置文件存放在/etc/sysconfig/docker。

# 6. 如何更改 Docker 的默认存储设置？
Docker的默认存放位置是/var/lib/docker，如果希望将Docker的本地文件存储到其他分区，可以使用Linux软连接的方式来实现。

# 7. 非官方仓库下载镜像时，可能提示“Error：Invaild registry endpoint https://dl.docker.com:5000/v1/…”？
非官方地址，例如：dl.dockerpool.com。

Docker自1.3.0版本之后，加强了对镜像安全性的验证，需要手动添加对非官方仓库的信任。

```shell
DOCKER_OPTS=”–insecure-registry dl.dockerpool.com:5000”
```

重启docker服务即可。

# 8. Docker 需要查询日志应该使用什么命令？
```shell
docker logs
```
# 9. Docker 镜像和层有什么区别？
Docker镜像是由一系列只读层构建的，而每个层代表Dockerfile中的一条指令。

# 10. DevOps 有哪些优势？
技术优势：

- 持续的软件交付
- 修复不太复杂的问题
- 更快地解决问题

商业利益：
- 更快速地传递功能
- 更稳定的操作环境
- 有更多时间可以增加价值（而不是修复/维护）
# 11. CI（持续集成）服务器的功能是什么？
CI服务器功能是不断地集成所有正在进行的更改并由不同的开发人员提交到存储库，并检查编译错误。它需要每天多次构建代码，最好是在每次提交之后，以便它可以检测在问题发生时是哪个提交Bug了。

# 12. 容器与主机之间的数据拷贝命令是什么？
docker cp命令，用于容器与主机之间的数据拷贝。

1、从主机往容器中拷贝

将主机/www/jingxuan目录拷贝到容器a1234556789的/www目录下。
```shell
docker cp /www/jingxuan a1234556789:/www/
```
2、将容器中文件拷往主机

将容器a1234556789的/www目录拷贝到主机的/tmp目录中。
```shell
docker cp  a1234556789:/www /tmp/
```