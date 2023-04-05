# 1. Linux 中如何翻页查看大文件内容？
通过管道将命令“cat file_name.txt”和“more”连接在一起可以实现翻页查看大文件内容。

```shell
[root@mrwang ~]# cat file_name.txt | more
```
# 2. Linux 中如何查看几颗物理 CPU 和每颗 CPU 的核数？
```shell
[root@JingXuan-Java ~]# cat /proc/cpuinfo|grep -c 'physical id'
4
[root@JingXuan-Java ~]# cat /proc/cpuinfo|grep -c 'processor'
4
```
# 3. Linux 中如何查看系统负载？数值表示什么含义？
```shell
[root@JingXuan-Java ~]# w
11:35:35 up 155 days,  1:22,  1 user,  load average: 0.04, 0.04, 0.01
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
root     pts/1    124.200.36.106   11:33    0.00s  0.01s  0.00s w
[root@JingXuan-Java ~]# uptime
11:35:43 up 155 days,  1:22,  1 user,  load average: 0.04, 0.04, 0.00
```
其中load average即系统负载，三个数值分别表示一分钟、五分钟、十五分钟内系统的平均负载，即平均任务数。

# 4. vmstat 命令中 r、b、si、so、bi、bo 这几列表示什么含义？
```shell
[root@JingXuan-Java ~]# vmstat
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
0  0      0 112896      0 641456    0    0    13    18    1    2  0  0 99  0  0
```
r即running，表示正在跑的任务数。

b即blocked，表示被阻塞的任务数。

si表示有多少数据从交换分区读入内存。

so表示有多少数据从内存写入交换分区。

bi表示有多少数据从磁盘读入内存。

bo表示有多少数据从内存写入磁盘。

其他：

i --input，进入内存。

o --output，从内存出去。

s --swap，交换分区。

b --block，块设备，磁盘。

注：单位都是KB

# 5. Linux 中 buffer 和 cache 如何区分？
buffer和cache都是内存中的一块区域，当CPU需要写数据到磁盘时，由于磁盘速度比较慢，所以CPU先把数据存进buffer，然后CPU去执行其他任务，buffer中的数据会定期写入磁盘；当CPU需要从磁盘读入数据时，由于磁盘速度比较慢，可以把即将用到的数据提前存入cache，CPU直接从Cache中拿数据要快的多。

# 6. 使用 top 查看系统资源占用情况时，哪一列表示内存占用？
```shell
[root@JingXuan-Java ~]# top
top - 11:41:17 up 155 days,  1:28,  1 user,  load average: 0.01, 0.02, 0.00
Tasks: 110 total,   1 running, 109 sleeping,   0 stopped,   0 zombie
%Cpu(s):  1.0 us,  0.5 sy,  0.0 ni, 98.2 id,  0.0 wa,  0.2 hi,  0.2 si,  0.0 st
MiB Mem :   3637.6 total,    130.2 free,   2901.4 used,    606.0 buff/cache
MiB Swap:      0.0 total,      0.0 free,      0.0 used.    509.8 avail Mem

PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND                                                                                                             
803 polkitd   20   0 1608468   5104      0 S   0.3   0.1  43:09.20 polkitd                                                                                                             
8562 root      20   0  140676   8596   7408 S   0.3   0.2   0:00.01 sshd                                                                                                                
8563 root      20   0  143076   8916   7736 S   0.3   0.2   0:00.01 sshd        
```
VIRT表示虚拟内存用量。

RES表示物理内存用量。

SHR表示共享内存用量。

%MEM表示内存用量。

# 7. Linux 中如何实时查看网卡流量？历史网卡流量？
linux安装sysstat包，使用sar命令查看。

```shell
yum install -y sysstat#安装sysstat包，获得sar命令
```
查看网卡流量命令

```shell
sar -n DEV  #查看网卡流量，默认10分钟更新一次
sar -n DEV 1 10  #一秒显示一次，一共显示10次
sar -n DEV -f /var/log/sa/sa22  #查看指定日期的流量日志
```
# 8. Linux 中如何查看系统都开启了哪些端口？
```shell
[root@JingXuan-Java ~]# netstat -lnp
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 0.0.0.0:5355            0.0.0.0:*               LISTEN      1035/systemd-resolv
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      317/nginx: worker p
tcp        0      0 0.0.0.0:6386            0.0.0.0:*               LISTEN      1718/./src/redis-se
tcp        0      0 0.0.0.0:443             0.0.0.0:*               LISTEN      317/nginx: worker p
tcp        0      0 0.0.0.0:1022            0.0.0.0:*               LISTEN      1089/sshd           
tcp6       0      0 :::5355                 :::*                    LISTEN      1035/systemd-resolv
tcp6       0      0 :::8080                 :::*                    LISTEN      27423/java          
```
# 9. Linux 中如何查看网络连接状况？
```shell
[root@JingXuan-Java ~]# netstat -an
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State      
tcp        0      0 0.0.0.0:5355            0.0.0.0:*               LISTEN     
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN     
tcp        0      0 0.0.0.0:6386            0.0.0.0:*               LISTEN     
tcp        0      0 0.0.0.0:443             0.0.0.0:*               LISTEN     
tcp        0      0 0.0.0.0:1022            0.0.0.0:*               LISTEN     
tcp        0      0 172.17.44.191:1022      75.119.154.165:34310    TIME_WAIT  
```
# 10. Linux 中如何重启网卡，使配置生效？
使用vi或者vim编辑器编辑网卡配置文件/etc/sysconfig/network-scripts/ifcft-eth0（如果是eth1文件名为ifcft-eth1），内容如下：

```shell
DEVICE=eth0
HWADDR=00:0C:29:06:37:BA
TYPE=Ethernet
UUID=0eea1820-1fe8-4a80-a6f0-39b3d314f8da
ONBOOT=yes
NM_CONTROLLED=yes
BOOTPROTO=static
IPADDR=192.168.147.130
NETMASK=255.255.255.0
GATEWAY=192.168.147.2
DNS1=192.168.147.2
DNS2=8.8.8.8
```
修改网卡后，使用命令重启网卡：

```shell
ifdown eth0
ifup eth0
```
也可以重启网络服务：

```shell
service network restart
```
# 11. Linux 中如何查看某个网卡是否连接着交换机？
使用mii-tool eth0或者mii-tool eth1命令，查看某个网卡是否连接着交换机。

# 12. 请描述 Linux 系统优化的 12 个步骤。
1）登录系统:不使用root登录，通过sudo授权管理，使用普通用户登录。 2）禁止SSH远程：更改默认的远程连接SSH服务及禁止root远程连接。

3）时间同步：定时自动更新服务器时间。

4）配置yum更新源，从国内更新下载安装rpm包。

5）关闭selinux及iptables（iptables工作场景如有wan ip，一般要打开，高并发除外）

6）调整文件描述符数量，进程及文件的打开都会消耗文件描述符。

7）定时自动清理/var/spool/clientmquene/目录垃圾文件，防止节点被占满（c6.4默认没有sendmail，因此可以不配。）

8）精简开机启动服务（crond、sshd、network、rsyslog）

9）Linux内核参数优化/etc/sysctl.conf，执行sysct -p生效。

10）更改字符集，支持中文，但是还是建议使用英文，防止乱码问题出现。

11）锁定关键系统文件（chattr +i /etc/passwd /etc/shadow /etc/group /etc/gshadow /etc/inittab处理以上内容后，把chatter改名，就更安全了。）

12）清空/etc/issue，去除系统及内核版本登陆前的屏幕显示。

# 13. 描述 Linux 系统从开机到登陆界面的启动过程。
1、开机BIOS自检，加载硬盘。

2、读取MBR,MBR引导。

3、grub引导菜单(Boot Loader)。

4、加载内核kernel。

5、启动init进程，依据inittab文件设定运行级别

6、init进程，执行rc.sysinit文件。

7、启动内核模块，执行不同级别的脚本程序。

8、执行/etc/rc.d/rc.local

9、启动mingetty，进入系统登陆界面。

# 14. 生产场景如何对 Linux 系统进行合理规划分区？
分区的根本原则是简单、易用、方便批量管理。根据服务器角色定位建议如下：

①单机服务器：如8G内存，300G硬盘

分区： /boot 100-200M，swap 16G，内存大小8G*2，/ 80G，/var 20G（也可不分），/data 180G（存放web及db数据）

优点：数据盘和系统盘分开，有利于出问题时维护。

RAID方案：视数据及性能要求，一般可采用raid5折中。

②负载均衡器（如LVS等）

分区：/boot 100-200M，swap 内存的1-2倍，/ ，

优点：简单方便，只做转发数据量很少。

RAID方案：数据量小，重要性高，可采用RAID1

③负载均衡下的RS server

分区： /boot 100-200M，swap 内存的1-2倍，/

优点：简单方便，因为有多机，对数据要求低。

RAID方案：数据量大，重要性不高，有性能要求，数据要求低，可采用RAID0

④数据库服务器mysql及oracle如16/32G内存

分区：/boot 100-200M，swap 16G，内存的1倍，/ 100G，/data 剩余（存放db数据）

优点：数据盘和系统盘分开，有利于出问题时维护,及保持数据完整。

RAID方案：视数据及性能要求主库可采取raid10/raid5，从库可采用raid0提高性能（读写分离的情况下。）

⑤存储服务器

分区：/boot 100-200M，swap 内存的1-2倍，/ 100G，/data(存放数据)

优点：此服务器不要分区太多。只做备份，性能要求低。容量要大。

RAID方案：可采取sata盘，raid5

⑥共享存储服务器（如NFS）

分区：/boot 100-200M，swap 内存的1-2倍，/ 100G，/data(存放数据)

优点：此服务器不要分区太多。NFS共享比存储多的要求就是性能要求。

RAID方案：视性能及访问要求可以raid5,raid10,甚至raid0（要有高可用或双写方案）

⑦监控服务器cacti,nagios

分区：/boot 100-200M，swap 内存的1-2倍，/

优点：重要性一般，数据要求也一般。

RAID方案：单盘或双盘raid1即可。三盘就RAID5，看容量要求加盘即可。

# 15. 什么是 Linux 内核？
Linux 系统的核心是内核。内核控制着计算机系统上的所有硬件和软件，在必要时分配硬件，并根据需要执行软件。

系统内存管理

应用程序管理

硬件设备管理

文件系统管理

# 16. Linux 的体系结构包括哪些？
从大的方面讲，Linux 体系结构可以分为两块：

用户空间（User Space）： 用户空间又包括用户的应用程序（User Applications）、C 库（C Library） 。

内核空间（Kernel Space）： 内核空间又包括系统调用接口（System Call Interface）、内核（Kernel）、平台架构相关的代码（Architecture-Dependent Kernel Code） 。

# 17. Linux 性能调优都有哪几种方法？
1、Disabling daemons（关闭 daemons）。

2、Shutting down the GUI（关闭 GUI）。

3、Changing kernel parameters（改变内核参数）。

4、Kernel parameters（内核参数）。

5、Tuning the processor subsystem（处理器子系统调优）。

6、Tuning the memory subsystem（内存子系统调优）。

7、Tuning the file system（文件系统子系统调优）。

8、Tuning the network subsystem（网络子系统调优）。
