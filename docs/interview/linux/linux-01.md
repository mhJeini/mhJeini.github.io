# 1. 什么是 Linux 操作系统？
   Linux全称GNU/Linux，是一套免费使用和自由传播的类Unix操作系统，是一个基于POSIX的多用户、多任务、支持多线程和多CPU的操作系统。

伴随着互联网的发展，Linux得到了来自全世界软件爱好者、组织、公司的支持。它除了在服务器方面保持着强劲的发展势头以外，在个人电脑、嵌入式系统上都有着长足的进步。使用者不仅可以直观地获取该操作系统的实现机制，而且可以根据自身的需要来修改完善Linux，使其最大化地适应用户的需要。

Linux不仅系统性能稳定，而且是开源软件。其核心防火墙组件性能高效、配置简单，保证了系统的安全。

在很多企业网络中，为了追求速度和安全，Linux不仅仅是被网络运维人员当作服务器使用，甚至当作网络防火墙，这是Linux的一大亮点。

Linux具有开放源码、没有版权、技术社区用户多等特点，开放源码使得用户可以自由裁剪，灵活性高，功能强大，成本低。尤其系统中内嵌网络协议栈，经过适当的配置就可实现路由器的功能。这些特点使得Linux成为开发路由交换设备的理想开发平台。

# 2. Linux 如何切换用户？
Linux系统切换用户的命令是su

su的含义是(switch user)切换用户的缩写。

通过su命令，可以从普通用户切换到root用户，也可以从root用户切换到普通用户。


使用SecureCRT工具连接终端，由普通用户切换到root用户。

在终端输入su命令然后回车，要求输入密码（linux终端输入的密码不显示）输入密码后回车进入root用户。

在终端输入su root然后回车，也可以进入root用户或su - root回车，也可以切换root用户。针对su root与su - root区别之后的篇幅会阐述，欢迎关注微信公众号“Java精选”，送免费资料。

# 3. su root 和 su - root 有什么区别？
su后面不加用户是默认切到root，su是不改变当前变量；su -是改变为切换到用户的变量。

简单来说就是su只能获得root的执行权限，不能获得环境变量，而su -是切换到root并获得root的环境变量及执行权限。

语法：

```shell
$ su [user_name]
```
su命令可以用来更改你的用户ID和组ID。su是switch user或set user id的一个缩写。

此命令是用于开启一个子进程，成为新的用户ID和赋予存取与这个用户ID关联所有文件的存取权限。

出于安全的考虑，在实际转换身份时，会被要求输入这个用户帐号的密码。

如果没有参数，su命令将转换为root（系统管理员）。root帐号有时也被称为超级用户，因为这个用户可以存取系统中的任何文件。

当必须要提供root密码，想要回到原先的用户身份，可以不使用su命令，只需使用exit命令退出使用su命令而生成的新的对话进程。

```shell
$ su – username
```
当使用命令su username时，对话特征和原始的登录身份一样。

如果需要对话进程拥有转换后的用户ID一致的特征，使用短斜杠命令: su – username。

# 4. Linux 怎么切换目录？
1）使用pwd命令查看一下当前所在的目录

2）切换文件目录使用的cd命令。

切换到根目录命令如下：

```shell
cd /
```
3）根目录下使用ls命令查看该目录下有哪些文件，使用用绝对路径的方式进入所需目录，比如进入usr目录，命令如下：

```shell
cd /usr
```
4）若进入文件下一层级目录，可以使用相对路径的方式切换目录。比如目录结构如下：

```shell
usr/local/other
```
在当前usr目录下使用命令

```shell
cd ./local
```
进入usr目录下的local目录。此命令和使用绝对路径的方式区别在于，前面多了个 "."，这个"."代表的是当前目录。

5）若要回到上一级目录，则可以使用命令

```shell
cd ../
```
# 5. 查看文件内容有哪些命令？

vi 文件名

编辑方式查看，可以修改文件

cat 文件名

显示全部文件内容

tail 文件名

仅查看文件尾部信息，可以指定查看行数

head 文件名

仅查看头部,可以指定查看行数

more 文件名

分页显示文件内容

less 文件名

与more相似，可以往前翻页

# 6. 命令中可以使用哪几种通配符？
“？”

可以替代任意单个字符。

“*”

可以替代任意多个字符。

方括号“[charset]”

可以替代charset集中的任何单个字符，比如[a-z]，[abABC]

# 7. 根据文件名搜索文件有哪些命令？
find <指定目录> <指定条件> <指定动作>，

whereis 加参数与文件名

locate 只加文件名

# 8. bash shell 中 hash 命令有什么作用？
Linux中hash命令管理着一个内置的哈希表，记录了已执行过命令的完整路径, 用该命令可以打印出所使用过的命令以及执行的次数。

# 9. Linux 中进程有哪几种状态？
1）不可中断状态：进程处于睡眠状态，此时的进程不可中断，进程不响应异步信号。

2）暂停状态/跟踪状态：向进程发送一个SIGSTOP信号，它就会因响应信号而进入TASK_STOPPED状态；当进程正在被跟踪时，它处于TASK_TRACED这个特殊的状态。正在被跟踪指的是进程暂停下来，等待跟踪它的进程对它进行操作。

3）就绪状态：在run_queue队列中的状态

4）运行状态：在run_queue队列中的状态

5）可中断睡眠状态：处于这个状态的进程因为等待某事件的发生，比如等待socket连接等，而被挂起

6）zombie状态（僵尸）：父进程没有通过wait系列的系统调用，直接将子进程的task_struct也释放掉。

7）退出状态

# 10. Linux 如何统计系统当前进程连接数？
Linux 统计系统当前进程连接数，输入命令如下：

```shell
netstat -an | grep ESTABLISHED | wc -l
```
输出结果33。意思就是目前一共有33个连接数。

# 11. Linux 常见目录结构都有哪些？
/bin： 存放二进制可执行文件(ls,cat,mkdir等)，常用命令一般都在这里；

/etc： 存放系统管理和配置文件；

/home： 存放所有用户文件的根目录，是用户主目录的基点，比如用户user的主目录就是/home/user，可以用~user表示；

/usr ： 用于存放系统应用程序；

/opt： 额外安装的可选应用程序包所放置的位置。一般情况下，我们可以把tomcat等都安装到这里；

/proc： 虚拟文件系统目录，是系统内存的映射。可直接访问这个目录来获取系统信息；

/root： 超级用户（系统管理员）的主目录（特权阶级o）；

/sbin: 存放二进制可执行文件，只有root才能访问。这里存放的是系统管理员使用的系统级别的管理命令和程序。如ifconfig等；

/dev： 用于存放设备文件；

/mnt： 系统管理员安装临时文件系统的安装点，系统提供这个目录是让用户临时挂载其他的文件系统；

/boot： 存放用于系统引导时使用的各种文件；

/lib ： 存放着和系统运行相关的库文件 ；

/tmp： 用于存放各种临时文件，是公用的临时文件存储点；

/var： 用于存放运行时需要改变数据的文件，也是某些大文件的溢出区，比方说各种服务的日志文件（系统启动日志等）等；

/lost+found： 这个目录平时是空的，系统非正常关机而留下“无家可归”的文件（windows下叫什么.chk）就在这里。

# 12. Linux 中什么是硬链接和软链接？
1、硬链接

由于Linux下的文件是通过索引节点(inode)来识别文件，硬链接可以认为是一个指针，指向文件索引节点的指针，系统并不为它重新分配inode。每添加一个一个硬链接，文件的链接数就加1。

硬链接不足是不可以在不同文件系统的文件间建立链接；只有超级用户才可以为目录创建硬链接。

2、软链接

软链接克服了硬链接的不足，没有任何文件系统的限制，任何用户可以创建指向目录的符号链接。因而现在更为广泛使用，它具有更大的灵活性，甚至可以跨越不同机器、不同网络对文件进行链接。

软链接不足之处是链接文件包含有原文件的路径信息，所以当原文件从一个目录下移到其他目录中，再访问链接文件，系统就找不到了文件了，而硬链接就没有这个缺陷，想怎么移就怎么移；还有它要系统分配额外的空间用于建立新的索引节点和保存原文件的路径。

# 13. Linux 设置 DNS 修改哪个配置文件？
全局的配置可以在/etc/resolv.conf文件中配置。

指定网卡的配置可以在/etc/sysconfig/network-scripts/ifcfg-eth0文件中配置。

一般来说，肯定是全局配置即可。

# 14. Linux 常见服务占用端口都有哪些？
80 8080 443

20 21 22 23 25 53

135（RPC）137（NetBIOS/UDP） 138（UDP） 139 （samba）

161 SNMP

1080 Socket代理

3306 11211 8080 jboss tomcat 50170

# 15. Shell 脚本是什么？
一个Shell脚本可以理解为一个文本文件，它包含一个或多个命令。

比如作为系统管理员，经常需要使用多个命令来完成一项任务，那么就可以可以将这些命令添加在一个文本文件中（Shell脚本），来完成这些日常的工作任务。

Shell是一种脚本语言，那么就必须有解释器来执行这些脚本，常见的脚本解释器如下：

bash

bash脚本解释器是Linux标准默认的shell。

bash由Brian Fox和Chet Ramey共同完成，是BourneAgain Shell的缩写，内部命令一共有40个。

sh

sh脚本解释器是由Steve Bourne开发，是Bourne Shell的缩写，sh是Unix标准默认的shell。

另外还有：ash、csh、ksh等。

# 16. Linux 中什么是默认登录 Shell ？
在Linux操作系统中"/bin/bash"是默认登录Shell，这是在创建用户时分配的。

使用chsh命令可以改变默认的Shell。命令如下：

```shell
## chsh <用户名> -s <新shell>
## chsh JingXuan -s /bin/sh
```
# 17. Linux 中 Shell 脚本如何增加注释？
注释是用来描述一个脚本是做什么的和它是如何工作的。

每一行注释以#开头，举例如下：

```shell
#!/bin/bash
## This is a command
echo "关注微信公众号“Java精选”，领取成套面试题资料"
```
# 18. Shell 脚本中 $? 标记有什么用途？
编写一个Shell脚本时，如果想要检查前一命令是否执行成功，可以在if条件中使用“$?”来检查前一命令的结束状态。

如果结束状态是0，说明前一个命令执行成功。举例如下：

```shell
[root@iZ256w2hluuZ ~]# ls /mnt/app/tomcat/
apache-tomcat-blog    apache-tomcat-jingxuan  jingxuan-admin.jar  logs
apache-tomcat-client  apache-tomcat-server    jingxuan.log        zs-jingxuan-admin.jar
[root@iZ256w2hluuZ ~]# echo $?
0
[root@iZ256w2hluuZ ~]#
```
如果结束状态不是0，说明命令执行失败。举例如下：

```shell
[root@iZ256w2hluuZ ~]# ls /mnt/app/nginx
ls: cannot access /mnt/app/nginx: No such file or directory
[root@iZ256w2hluuZ ~]# echo $?          
2
[root@iZ256w2hluuZ ~]#
```
# 19. Linux 中如何修改文件权限？
语法

```shell
chmod [-cfvR] [--help] [--version] mode file...
```
参数说明

mode : 权限设定字串，格式如下 :
```shell
[ugoa...][[+-=][rwxX]...][,...]
```
其中：

u表示该文件的拥有者，g表示与该文件的拥有者属于同一个群体(group)者，o表示其他以外的用户，a表示这三者皆是。

+ 表示增加权限、-表示取消权限、=表示唯一设定权限。

r表示可读取，w表示可写入，x表示可执行，X表示只有当该文件是个子目录或者该文件已经被设定过为可执行。

其他参数说明

-c若该文件权限确实已经更改，才显示其更改动作

-f若该文件权限无法被更改也不要显示错误讯息

-v显示权限变更的详细资料

-R对目前目录下的所有文件与子目录进行相同的权限变更，即以递回的方式逐个变更。

--help显示辅助说明

--version显示版本

chmod命令使用数字方式修改文件权限

文件的基本权限由9个字符组成，以rwxrw-r-x为例，可以使用数字来代表各个权限，各个权限与数字的对应关系如下：

r --> 4 w --> 2 x --> 1

这9个字符分属3类用户，每种用户身份包含3个权限（r、w、x），通过将3个权限对应的数字累加，最终得到的值即可作为每种用户所具有的权限。

比如rwxrw-r-x，所有者、所属组和其他人分别对应的权限值为：

所有者 = rwx = 4+2+1 = 7 所属组 = rw- = 4+2 = 6 其他人 = r-x = 4+1 = 5

因此权限对应的权限值就是765。

使用数字方式修改文件权限的chmod命令基本格式为：

```shell
chmod [-R] 权限值 文件名
```
举例修改微信小程序“Java精选面试题”，后台部署的jingxuan-admin.jar包文件的权限：

```shell
[root@iZ256w2hluuZ tomcat]# ll
total 100
-rw-r--r--  1 root root 75504722 May 29 13:29 jingxuan-admin.jar
[root@iZ256w2hluuZ tomcat]# chmod 765 jingxuan-admin.jar
[root@iZ256w2hluuZ tomcat]# ll
total 100
-rwxrw-r-x  1 root root 75504722 May 29 13:29 jingxuan-admin.jar
```
# 20. Linux 中如何进入含有空格的目录？
Linux中有时需要进入带有空格的目录，比如“jing xuan”目录。

可以将整个目录用英文半角的双引号""包过起来执行cd命令，举例如下：

```shell
[root@iZ256w2hluuZ tomcat]# ls
jing xuan
[root@iZ256w2hluuZ tomcat]# cd jing xuan
-bash: cd: jing: No such file or directory
[root@iZ256w2hluuZ tomcat]# cd "jing xuan"
[root@iZ256w2hluuZ jing xuan]#
```
可以使用转义的方法\将空格转义执行cd命令，举例如下：

```shell
[root@iZ256w2hluuZ tomcat]# ls
jing xuan  
[root@iZ256w2hluuZ tomcat]# cd jing\ xuan
[root@iZ256w2hluuZ jing xuan]# 
```
