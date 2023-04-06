# 1. Linux 中 ll 和 ls 命令有什么区别？
ll命令可以列出当前文件或目录的详细信息，含有时间、读写权限、大小、时间等信息 ，类似Windows显示的详细信息。而ls命令只可以列出文件名或目录名，类似windows的列表。

ll命令是ls -l命令的别名。相当于Windows里的快捷方式，也就是可以理解为ll和ls -l命令功能是相同的。

ls [-参数]


-a 列出目录下的所有文件，包括以 . 开头的隐含文件。
-b 把文件名中不可输出的字符用反斜杠加字符编号(就象在C语言里一样)的形式列出。
-c 输出文件的 i 节点的修改时间，并以此排序。
-d 将目录象文件一样显示，而不是显示其下的文件。
-e 输出时间的全部信息，而不是输出简略信息。
-f -U 对输出的文件不排序。
-g 无用。
-i 输出文件的 i 节点的索引信息。
-k 以 k 字节的形式表示文件的大小。
-l 列出文件的详细信息。
-m 横向输出文件名，并以“，”作分格符。
-n 用数字的 UID,GID 代替名称。
-o 显示文件的除组信息外的详细信息。
-p -F 在每个文件名后附上一个字符以说明该文件的类型，“*”表示可执行的普通
文件；“/”表示目录；“@”表示符号链接；“|”表示FIFOs；“=”表示套接字(sockets)。
-q 用?代替不可输出的字符。
-r 对目录反向排序。
-s 在每个文件名后输出该文件的大小。
-t 以时间排序。
-u 以文件上次被访问的时间排序。
-x 按列输出，横向排序。
-A 显示除 “.”和“..”外的所有文件。
-B 不输出以 “~”结尾的备份文件。
-C 按列输出，纵向排序。
-G 输出文件的组的信息。
-L 列出链接文件名而不是链接到的文件。
-N 不限制文件长度。
-Q 把输出的文件名用双引号括起来。
-R 列出所有子目录下的文件。
-S 以文件大小排序。
-X 以文件的扩展名(最后一个 . 后的字符)排序。
-1 一行只输出一个文件。
-–color=no 不显示彩色文件名
-–help 在标准输出上显示帮助信息。
-–version 在标准输出上输出版本信息并退出。
查看当前目录下文件数和目录数（包含子目录中的），命令如下：

```shell
[root@iZ256w2hluuZ tomcat]# ls -l * |grep "^-"|wc -l
247
[root@iZ256w2hluuZ tomcat]# ls -l * |grep "^d"|wc -l
29
```
去掉“|wc -l”命令可查看当前目录下文件和目录（包含子目录中的）。

# 2. Linux 中如何切换到上 N 级目录？
执行cd命令可以切换上下级目录，如果切换到上一级目录执行“cd ..”命令：

```shell
[root@iZ256w2hluuZ tomcat]# pwd
/mnt/app/tomcat
[root@iZ256w2hluuZ tomcat]# cd ..
[root@iZ256w2hluuZ app]# pwd
/mnt/app
```

如果切换上两级目录需执行“cd ../..”命令，这里的“../”可以理解成上一级目录，其中“/”最后一个可以省略也可以携带，没有影响。每增加一个“../”即为增加一次上一级目录，举例如下：

```shell
[root@iZ256w2hluuZ apache-tomcat-jingxuan]# pwd
/mnt/app/tomcat/apache-tomcat-jingxuan
[root@iZ256w2hluuZ apache-tomcat-jingxuan]# cd ../../../
[root@iZ256w2hluuZ mnt]# pwd
/mnt
[root@iZ256w2hluuZ mnt]# 
```
# 3. Linux 中如何快速切换到上 N 级目录？
编辑文件

使修改在当前用户下有效，执行命令如下：

```shell
[root@iZ256w2hluuZ conf]# vim .bashrc
```
使修改在所有用户下有效，需切换root用户下，执行命令如下：

```shell
[root@iZ256w2hluuZ conf]# vim /etc/profile
```
打开文件后，在文件结尾添加别名如下：

```shell
alias cd1 = 'cd ..'
alias cd2 = 'cd ../..'
alias cd3 = 'cd ../../..'
alias cd4 = 'cd ../../../..'
alias cd5 = 'cd ../../../../..'
alias cd6 = 'cd ../../../../../..'
```
执行wq命令，保存文件并退出。

为了使修改立即生效

.bashrc文件执行命令：

```shell
source .bashrc
```
profile文件，执行命令：

```shell
source /etc/profile
```
下面就可以执行对应不同级别目录的命令切换目录了，举例如下：

```shell
[root@iZ256w2hluuZ tomcat]# pwd
/mnt/app/tomcat
[root@iZ256w2hluuZ tomcat]# cd2
[root@iZ256w2hluuZ mnt]# pwd
/mnt
```
# 4. Linux 中如何返回到切换目录之前的目录？
Linux系统上切换目录使用cd（change directory）命令，查看当前目录使用pwd （print working directory）命令。

例如当前目录为:/home/Jingxuan/Tomcat

```shell
[root@iZ256w2hluuZ Tomcat]#  pwd
/home/Jingxuan/Tomcat
[root@iZ256w2hluuZ Tomcat]#  
```
进入到根目录，执行cd /命令。

```shell
[root@iZ256w2hluuZ Tomcat]#  cd /
[root@iZ256w2hluuZ /]# pwd
/
[root@iZ256w2hluuZ /]#
```
如果需要返回到/home/Jingxuan/Tomcat目录，则执行cd -命令。

```shell
[root@iZ256w2hluuZ /]# cd -
/home/Jingxuan/Tomcat
[root@iZ256w2hluuZ Tomcat]#  pwd
/home/Jingxuan/Tomcat
[root@iZ256w2hluuZ Tomcat]#  
```
# 5. Linux 中零拷贝是什么？
零拷贝主要的任务是避免CPU将数据从一块存储拷贝到另外一块存储，利用各种零拷贝技术，避免让CPU做大量的数据拷贝任务，减少不必要的拷贝，或者让别的组件来做这一类简单的数据传输任务，让CPU解脱出来专注于别的任务。这样就可以让系统资源的利用更加有效。

# 6. Linux 中为什么需要零拷贝？
传统的Linux系统的标准I/O接口（read、write）是基于数据拷贝的，也就是数据都是copy_to_user或者copy_from_user。

好处：通过中间缓存的机制，减少磁盘I/O的操作。

坏处：大量数据的拷贝，用户态和内核态的频繁切换，会消耗大量的CPU资源，严重影响数据传输的性能。

在网络速度比较慢的时代（56K猫、10/100MB以太网）其实不需要零拷贝技术，因为内部再快也会被网络速率卡住，木桶效应。但是当网路速度大幅提升出现1Gb、10Gb甚至100Gb网速的时候这种零拷贝技术就迫切需要，因为网络传输速度已经远远大于计算机内部的数据流转速度。所以有必要提速，那么这时候人们就关注如何优化计算机内部数据流。

# 7. Linux 中如何创建软链接？
用法：ln -s 源文件 目标文件

```shell
[root@mrwang ~]#  ln -s /jingxuan/titles /home/titles
```
其中/jingxuan/titles目录中titles是源文件，/home/titles目录中titles是目标文件，实际链接的是/jingxuan/titles文件。

删除软连接命令

```shell
[root@mrwang ~]# rm -rf /home/titles
```
这样只会删除目标文件，不会删除源文件。

# 8. Linux 中如何创建硬链接？
用法：ln 源文件 目标文件

```shell
[root@mrwang ~]#  ln /jingxuan/titles /home/titles
```
注意如果使用ln –f existingfile newfile，如果newfile已经存在，则无论原来newfile是什么文件，只要当前用户对它有写权限，newfile就成为exisitngfile的硬链接文件。

尽管硬链接节省空间，也是Linux系统整合文件系统的传统方式，但是存在一下不足之处：

1）不可以在不同文件系统的文件间建立链接。

2）不允许给目录创建硬链接。

# 9. hdfs 一个 block 多大，为什么 128M？
1、不能远小于128M：减少硬盘寻道时间、减少Namenode内存消耗。

2、不能远大于128M：

1）Map崩溃问题 （数据块大，重新加载时间长）。

2）预设时间间隔问题（从数据块的角度大概估算，数据块越大，时间越长）。

3）问题分解问题：数据量大小和问题解决的复杂度成线性关系。

4）约束map输出：map之后的数据需要排序后再执行reduce，大文件不利于归并排序的思想。

# 10. Linux 中如何查看运行日志？
动态打印日志信息：tail –f 日志文件

```shell
[root@mrwang apache-tomcat-blog]# tail -f ./logs/catalina.out
``` 
# 11. Linux 中如何关闭进程？
通常用ps查看进程PID，用kill命令终止进程。ps命令用于查看当前正在运行的进程。grep是搜索；-aux显示所有状态；

举例：

ps –ef | grep java表示查看所有进程里CMD是java的进程信息。

```shell
ps –aux | grep java
```
kill命令用于终止进程。例如：kill -9 [PID] -9表示强迫进程立即停止。

# 12. Linux 中查看文件内容有哪些命令？
vi 文件名 #编辑方式查看，可修改。

cat 文件名 #显示全部文件内容。

more 文件名 #分页显示文件内容。

less 文件名 #与 more 相似，更好的是可以往前翻页。

tail 文件名 #仅查看尾部，还可以指定行数。

head 文件名 #仅查看头部,还可以指定行数。

# 13. Linux 中如何让命令后台运行？
一般都是使用&在命令结尾来让程序自动运行。（命令后可以不追加空格）

举例：执行jingxuan.jar包，使其后台运行，退出连接端后不会终止进程。

```shell
java -jar jingxuan.jar &
```
# 14. Linux 中使用什么命令搜索文件？
find <指定目录> <指定条件> <指定动作>

whereis 加参数与文件名

locate 只加文件名

find 直接搜索磁盘，较慢。

举例：

```shell
find / -name "string*"
```
# 15. Linux 中使用什么命令查看磁盘占用情况？
使用df -lh命令来查看磁盘空间占用情况。

```shell
[root@mrwang /]# df -lh
Filesystem      Size  Used Avail Use% Mounted on
devtmpfs        1.8G     0  1.8G   0% /dev
tmpfs           1.8G     0  1.8G   0% /dev/shm
tmpfs           1.8G  512K  1.8G   1% /run
tmpfs           1.8G     0  1.8G   0% /sys/fs/cgroup
/dev/vda1        60G   16G   45G  26% /
tmpfs           364M     0  364M   0% /run/user/0
[root@mrwang /]# 
```
# 16. Linux 中如何启动和关闭防火墙？
CentOS7以上版本的防火墙配置跟以前版本有很大区别，CentOS7以上版本的防火墙默认使用的是firewall，与之前的版本使用iptables不一样。

1、关闭防火墙：

systemctl stop firewalld.service

2、开启防火墙：

systemctl start firewalld.service

3、关闭开机启动：

systemctl disable firewalld.service

4、开启开机启动：

systemctl enable firewalld.service

# 17. Linux 中如何查看指定目录的大小？
查询当前目录总大小可以使用du -sh，其中s代表统计汇总的意思，即只输出一个总和大小。

```shell
[root@mrwang jingxuan]# du -sh
6.1M    .
```
linux查看指定目录的的大小，可以使用“du -sh 目录名称”命令。

```shell
[root@mrwang jingxuan]# du -sh logs         
6.1M    logs
```
# 18. Linux 中使用什么命令查看 ip 地址及接口信息？
Linux中使用ifconfig命令查看ip地址及接口信息。

```shell
[root@mrwang jingxuan]# ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.17.44.191  netmask 255.255.240.0  broadcast 172.17.47.255
        inet6 fe80::216:3eff:fe35:66a2  prefixlen 64  scopeid 0x20<link>
        ether 00:16:3e:35:66:a2  txqueuelen 1000  (Ethernet)
        RX packets 44392695  bytes 23482900900 (21.8 GiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 40107204  bytes 48480485174 (45.1 GiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 19128710  bytes 33464900312 (31.1 GiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 19128710  bytes 33464900312 (31.1 GiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```
# 19. Linux 中 du 和 df 命令有什么区别？
du显示目录或文件的大小。

df显示每个<文件>所在的文件系统的信息，默认是显示所有文件系统。

文件系统分配其中的一些磁盘块用来记录它自身的一些数据，如i节点，磁盘分布图，间接块，超级块等。这些数据对大多数用户级的程序来说是不可见的，通常称为Meta Data。

du命令是用户级的程序，它不考虑Meta Data，而df命令则查看文件系统的磁盘分配图并考虑Meta Data。

df命令获得真正的文件系统数据，而du命令只查看文件系统的部分情况。

# 20. bash shell 中 hash 命令有什么作用？
linux中hash命令管理着一个内置的哈希表，记录了已执行过的命令的完整路径，用该命令可以打印出你所使用过的命令以及执行的次数。

```shell
[root@mrwang jingxuan]# hash
hits    command
   1    /usr/bin/tail
   5    /usr/bin/df
   1    /usr/sbin/ifconfig
   5    /usr/bin/du
   1    /usr/bin/netstat
   2    /usr/bin/ls
```
