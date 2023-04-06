# 1. Kubernetes 中 kube-proxy 有什么作用？
kube-proxy运行在所有节点上，它监听apiserver中service和endpoint的变化情况，创建路由规则以提供服务IP和负载均衡功能。简单理解此进程是Service的透明代理兼负载均衡器，其核心功能是将到某个Service的访问请求转发到后端的多个Pod实例上。

# 2. Kubernetes Replica Set 和 Replication Controller 有什么区别？
Replica Set和Replication Controller类似，都是确保在任何给定时间运行指定数量的Pod副本。不同之处在于RS使用基于集合的选择器，而Replication Controller使用基于权限的选择器。

# 3. Kubernetes DaemonSet 类型的资源有什么特性？
DaemonSet资源对象会在每个Kubernetes集群中的节点上运行，并且每个节点只能运行一个Pod，这是它和Deployment资源对象的最大也是唯一的区别。因此，在定义yaml文件中，不支持定义replicas。

它的一般使用场景如下：

在去做每个节点的日志收集工作。

监控每个节点的的运行状态。

# 4. K8s 是怎么进行服务注册的？
pod启动后会加载当前环境所有Service信息，以便不同Pod根据Service名进行通信。

# 5. 简述 kube-proxy ipvs 的原理？
IPVS在Kubernetes1.11中升级为GA稳定版。IPVS则专门用于高性能负载均衡，并使用更高效的数据结构（Hash表），允许几乎无限的规模扩张，因此被kube-proxy采纳为最新模式。

在IPVS模式下，使用iptables的扩展ipset，而不是直接调用iptables来生成规则链。iptables规则链是一个线性的数据结构，ipset则引入了带索引的数据结构，因此当规则很多时，也可以很高效地查找和匹配。

可以将ipset简单理解为一个IP（段）的集合，这个集合的内容可以是IP地址、IP网段、端口等，iptables可以直接添加规则对这个“可变的集合”进行操作，这样做的好处在于可以大大减少iptables规则的数量，从而减少性能损耗。

# 6. K8s 数据持久化的方式有哪些？
1、EmptyDir（空目录）：没有指定要挂载宿主机上的某个目录，直接由Pod内保部映射到宿主机上。类似于docker中的manager volume。

**主要使用场景：**

1）只需要临时将数据保存在磁盘上，比如在合并/排序算法中；

2）作为两个容器的共享存储，使得第一个内容管理的容器可以将生成的数据存入其中，同时由同一个webserver容器对外提供这些页面。

**emptyDir的特性：**

同个pod里面的不同容器，共享同一个持久化目录，当pod节点删除时，volume的数据也会被删除。如果仅仅是容器被销毁，pod还在，则不会影响volume中的数据。

总结来说：emptyDir的数据持久化的生命周期和使用的pod一致。一般是作为临时存储使用。

2、Hostpath：将宿主机上已存在的目录或文件挂载到容器内部。类似于docker中的bind mount挂载方式。

这种数据持久化方式，运用场景不多，因为它增加了pod与节点之间的耦合。

一般对于k8s集群本身的数据持久化和docker本身的数据持久化会使用这种方式，可以自行参考apiService的yaml文件，位于：/etc/kubernetes/main…目录下。

3、PersistentVolume（简称PV）：基于NFS服务的PV，也可以基于GFS的PV。它的作用是统一数据持久化目录，方便管理。

在一个PV的yaml文件中，可以对其配置PV的大小。指定PV的访问模式：

ReadWriteOnce：只能以读写的方式挂载到单个节点； ReadOnlyMany：能以只读的方式挂载到多个节点； ReadWriteMany：能以读写的方式挂载到多个节点。

**以及指定pv的回收策略：**

recycle：清除PV的数据，然后自动回收； Retain：需要手动回收； delete：删除云存储资源，云存储专用；

>注：这里的回收策略指的是在PV被删除后，在这个PV下所存储的源文件是否删除）。

若需使用PV，那么还有一个重要的概念：PVC，PVC是向PV申请应用所需的容量大小，K8s集群中可能会有多个PV，PVC和PV若要关联，其定义的访问模式必须一致。定义的storageClassName也必须一致，若群集中存在相同的（名字、访问模式都一致）两个PV，那么PVC会选择向它所需容量接近的PV去申请，或者随机申请。

# 7. 什么是 Heapster？
Heapster是由每个节点上运行的Kubelet提供的集群范围的数据聚合器。此容器管理工具在Kubernetes集群上本机支持，并作为pod运行，就像集群中的任何其他pod一样。因此，它基本上发现集群中的所有节点，并通过机上Kubernetes代理查询集群中Kubernetes节点的使用信息。

# 8. 什么是 Minikube？
Minikube是一种工具，可以在本地轻松运行Kubernetes。这将在虚拟机中运行单节点Kubernetes群集。

# 9. 什么是 Kubectl？
Kubectl是一个平台，可以使用该平台将命令传递给集群。因此，它基本上为CLI提供了针对Kubernetes集群运行命令的方法，以及创建和管理Kubernetes组件的各种方法。

# 10. 描述一下 Kubernetes 初始化容器（init container）？
init container的运行方式与应用容器不同，它们必须先于应用容器执行完成，当设置了多个init container时，将按顺序逐个运行，并且只有前一个init container运行成功后才能运行后一个init container。当所有init container都成功运行后，Kubernetes才会初始化Pod的各种信息，并开始创建和运行应用容器。

# 11. kube-apiserver 和 kube-scheduler 的作用是什么？
kube -apiserver遵循横向扩展架构，是主节点控制面板的前端。这将公开Kubernetes主节点组件的所有API，并负责在Kubernetes节点和Kubernetes主组件之间建立通信。

kube-scheduler负责工作节点上工作负载的分配和管理。因此，它根据资源需求选择最合适的节点来运行未调度的pod，并跟踪资源利用率。它确保不在已满的节点上调度工作负载。

# 12. 什么是 ETCD？
Etcd是用Go编程语言编写的，是一个分布式键值存储，用于协调分布式工作。因此，Etcd存储Kubernetes集群的配置数据，表示在任何给定时间点的集群状态。

# 13. Kubernetes 如何实现网络策略原理？
Network Policy的工作原理主要为：policy controller需要实现一个API Listener，监听用户设置的Network Policy定义，并将网络访问规则通过各Node的Agent进行实际设置（Agent则需要通过CNI网络插件实现）。

# 14. 说说你对 Job 这种资源对象的了解？
Job与其他服务类容器不同，Job是一种工作类容器（一般用于做一次性任务）。使用常见不多，可以忽略这个问题。

提高Job执行效率的方法：
```yaml
spec:
    parallelism: 2 #一次运行2个
    completions: 8 #最多运行8个
    template:
    metadata:
```

# 15. 如何解释 kubernetes 架构组件之间的不同 ？
Kubernetes由两层组成：控制平面和数据平面。

控制平面是容器编排层包括：

1）控制集群的 Kubernetes 对象。

2）有关集群状态和配置的数据。

数据平面是处理数据请求的层，由控制平面管理。

# 16. daemonset、deployment、replication 之间有什么区别？
daemonset：确保您选择的所有节点都运行Pod的一个副本。

deployment：是Kubernetes中的一个资源对象，它为应用程序提供声明性更新。它管理Pod的调度和生命周期。它提供了几个管理Pod的关键特性，包括Pod健康检查、Pod滚动更新、回滚能力以及轻松水平扩展Pod的能力。

replication：指定应该在集群中运行多少个Pod的精确副本。它与deployment的不同之处在于它不提供pod健康检查，并且滚动更新过程不那么健壮。

# 17. 什么是 Sidecar 容器，使用它做什么？
Sidecar容器是一个实用容器，用于扩展对Pod中主容器的支持。

Sidecar容器可以与一个或多个主容器配对，它们增强了这些主容器的功能。

例如：使用sidecar容器专门处理系统日志或监控。

# 18. Kubernetes 中如何处理容器日志 ？
使用传统的服务器设置，应用程序日志被写入文件，然后在每个服务器上查看或由日志代理收集并发送到集中位置。

在Kubernetes中，不建议将日志从pod写入磁盘，因为这种方式不得不管理pod的日志文件。

推荐的方法是让应用程序输出日志到stdout和stderr。每个节点上的kubelet收集正在运行的pod上的stdout和stderr，然后将它们组合成一个由Kubernetes管理的日志文件。然后您可以使用不同的kubectl命令来查看日志。

# 19. Kubernetes 中如何隔离资源？
可以使用命名空间来分隔资源。这些可以使用 kubectl或应用YAML文件创建。

创建命名空间后，可以在该命名空间中放置资源或创建新资源。有些人认为Kubernetes中的命名空间就像实际Kubernetes集群中的虚拟集群。

# 20. 简述 etcd 及其特点？
etcd是CoreOS团队发起的开源项目，是一个管理配置信息和服务发现（service discovery）的项目，它的目标是构建一个高可用的分布式键值（key-value）数据库，基于Go语言实现。

特点：

简单：支持REST风格的HTTP+JSON API

安全：支持HTTPS方式的访问

快速：支持并发1k/s的写操作

可靠：支持分布式结构，基于Raft的一致性算法，Raft是一套通过选举主节点来实现分布式系统一致性的算法。