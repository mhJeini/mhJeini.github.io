# 1. 简述 etcd 适应的场景？
etcd基于其优秀的特点，可广泛的应用于以下场景：

- 服务发现（Service Discovery）：服务发现主要解决在同一个分布式集群中的进程或服务，要如何才能找到对方并建立连接。本质上来说，服务发现就是想要了解集群中是否有进程在监听UDP或TCP端口，并且通过名字就可以查找和连接。

- 消息发布与订阅：在分布式系统中，最适用的一种组件间通信方式就是消息发布与订阅。即构建一个配置共享中心，数据提供者在这个配置中心发布消息，而消息使用者则订阅他们关心的主题，一旦主题有消息发布，就会实时通知订阅者。通过这种方式可以做到分布式系统配置的集中式管理与动态更新。应用中用到的一些配置信息放到etcd上进行集中管理。

- 负载均衡：在分布式系统中，为了保证服务的高可用以及数据的一致性，通常都会把数据和服务部署多份，以此达到对等服务，即使其中的某一个服务失效了，也不影响使用。etcd本身分布式架构存储的信息访问支持负载均衡。etcd集群化以后，每个etcd的核心节点都可以处理用户的请求。所以，把数据量小但是访问频繁的消息数据直接存储到etcd中也可以实现负载均衡的效果。

- 分布式通知与协调：与消息发布和订阅类似，都用到了etcd中的Watcher机制，通过注册与异步通知机制，实现分布式环境下不同系统之间的通知与协调，从而对数据变更做到实时处理。

- 分布式锁：因为etcd使用Raft算法保持了数据的强一致性，某次操作存储到集群中的值必然是全局一致的，所以很容易实现分布式锁。锁服务有两种使用方式，一是保持独占，二是控制时序。

- 集群监控与Leader竞选：通过etcd来进行监控实现起来非常简单并且实时性强。

# 2. 简述 Kubernetes 和 Docker 的关系？
Docker提供容器的生命周期管理和Docker镜像构建运行时容器。它的主要优点是将将软件/应用程序运行所需的设置和依赖项打包到一个容器中，从而实现了可移植性等优点。

Kubernetes用于关联和编排在多个主机上运行的容器。

# 3. 简述Minikube、Kubectl、Kubelet 分别是什么？
Minikube是一种可以在本地轻松运行一个单节点Kubernetes群集的工具。

Kubectl是一个命令行工具，可以使用该工具控制Kubernetes集群管理器，如检查群集资源，创建、删除和更新组件，查看应用程序。

Kubelet是一个代理服务，它在每个节点上运行，并使从服务器与主服务器通信。

# 4. 说一说 Kubernetes 常见的部署方式？
常见的Kubernetes部署方式包括：

- kubeadm，也是推荐的一种部署方式；

- 二进制；

- minikube，在本地轻松运行一个单节点Kubernetes群集的工具。

# 5. Kubernetes 如何实现集群管理？
在集群管理方面，Kubernetes将集群中的机器划分为一个Master节点和一群工作节点Node。其中，在Master节点运行着集群管理相关的一组进程kube-apiserver、kube-controller-manager和kube-scheduler，这些进程实现了整个集群的资源管理、Pod调度、弹性伸缩、安全控制、系统监控和纠错等管理能力，并且都是全自动完成的。

# 6. 简述 Kubernetes 的优势、适应场景及其特点？
Kubernetes作为一个完备的分布式系统支撑平台，其主要优势：

- 容器编排

- 轻量级

- 开源

- 弹性伸缩

- 负载均衡

Kubernetes常见场景：

- 快速部署应用

- 快速扩展应用

- 无缝对接新的应用功能

- 节省资源，优化硬件资源的使用

Kubernetes相关特点：

- 可移植：支持公有云、私有云、混合云、多重云（multi-cloud）。

- 可扩展: 模块化,、插件化、可挂载、可组合。

- 自动化: 自动部署、自动重启、自动复制、自动伸缩/扩展。

# 7. 容器和主机部署应用的区别是什么？
容器的中心思想就是秒级启动；一次封装、到处运行；这是主机部署应用无法达到的效果，但同时也更应该注重容器的数据持久化问题。

另外，容器部署可以将各个服务进行隔离，互不影响，这也是容器的另一个核心概念。

# 8. Kubernetes 集群有哪些相关组件？
Kubernetes Master控制组件，调度管理整个系统（集群），包含如下组件：

- Kubernetes API Server：作为Kubernetes系统的入口，其封装了核心对象的增删改查操作，以RESTful API接口方式提供给外部客户和内部组件调用，集群内各个功能模块之间数据交互和通信的中心枢纽。

- Kubernetes Scheduler：为新建立的Pod进行节点（Node）选择（即分配机器），负责集群的资源调度。

- Kubernetes Controller：负责执行各种控制器，目前已经提供了很多控制器来保证Kubernetes的正常运行。

- Replication Controller：管理维护Replication Controller，关联Replication Controller和Pod，保证Replication Controller定义的副本数量与实际运行Pod数量一致。

- Node Controller：管理维护Node，定期检查Node的健康状态，标识出（失效|未失效）的Node节点。

- Namespace Controller：管理维护Namespace，定期清理无效的Namespace，包括Namesapce下的API对象，比如Pod、Service等。

- Service Controller：管理维护Service，提供负载以及服务代理。

- EndPoints Controller：管理维护Endpoints，关联Service和Pod，创建Endpoints为Service的后端，当Pod发生变化时，实时更新Endpoints。

- Service Account Controller：管理维护Service Account，为每个Namespace创建默认的Service Account，同时为Service Account创建Service Account Secret。

- Persistent Volume Controller：管理维护Persistent Volume和Persistent Volume Claim，为新的Persistent Volume Claim分配Persistent Volume进行绑定，为释放的Persistent Volume执行清理回收。

- Daemon Set Controller：管理维护Daemon Set，负责创建Daemon Pod，保证指定的Node上正常的运行Daemon Pod。

- Deployment Controller：管理维护Deployment，关联Deployment和Replication Controller，保证运行指定数量的Pod。当Deployment更新时，控制实现Replication Controller和Pod的更新。

- Job Controller：管理维护Job，为Jod创建一次性任务Pod，保证完成Job指定完成的任务数目

- Pod Autoscaler Controller：实现Pod的自动伸缩，定时获取监控数据，进行策略匹配，当满足条件时执行Pod的伸缩动作。

# 9. 简述 Kubernetes RC 的机制？
Replication Controller用来管理Pod的副本，保证集群中存在指定数量的Pod副本。当定义了RC并提交至Kubernetes集群中之后，Master节点上的Controller Manager组件获悉，并同时巡检系统中当前存活的目标Pod，并确保目标Pod实例的数量刚好等于此RC的期望值，若存在过多的Pod副本在运行，系统会停止一些Pod，反之则自动创建一些Pod。

# 10. 什么是 Pod？
Pod是最基本的Kubernetes对象。Pod由一组在集群中运行的容器组成。 最常见的是，一个pod运行一个主容器。

# 11. 创建一个 pod 的流程是什么？
1）客户端提交Pod的配置信息（可以是yaml文件定义好的信息）到kube-apiserver；

2）Apiserver收到指令后，通知给controller-manager创建一个资源对象；

3）Controller-manager通过api-server将pod的配置信息存储到ETCD数据中心中；

4）Kube-scheduler检测到pod信息会开始调度预选，会先过滤掉不符合Pod资源配置要求的节点，然后开始调度调优，主要是挑选出更适合运行pod的节点，然后将pod的资源配置单发送到node节点上的kubelet组件上。

5）Kubelet根据scheduler发来的资源配置单运行pod，运行成功后，将pod的运行信息返回给scheduler，scheduler将返回的pod运行状况的信息存储到etcd数据中心。

# 12. Kubernetes 中 Pod 的生命周期有哪些状态？
Pending：表示pod已经被同意创建，正在等待kube-scheduler选择合适的节点创建，一般是在准备镜像；

Running：表示pod中所有的容器已经被创建，并且至少有一个容器正在运行或者是正在启动或者是正在重启；

Succeeded：表示所有容器已经成功终止，并且不会再启动；

Failed：表示pod中所有容器都是非0（不正常）状态退出；

Unknown：表示无法读取Pod状态，通常是kube-controller-manager无法与Pod通信。

# 13. K8s 集群外流量怎么访问 Pod？
可以通过Service的NodePort方式访问，会在所有节点监听同一个端口，比如：30000，访问节点的流量会被重定向到对应的Service上面。

# 14. 删除一个 Pod 会发生什么事情？
Kube-apiserver会接受到用户的删除指令，默认有30秒时间等待优雅退出，超过30秒会被标记为死亡状态，此时Pod的状态Terminating，kubelet看到pod标记为Terminating就开始了关闭Pod的工作；

关闭流程如下：

1、pod从service的endpoint列表中被移除；

2、如果该pod定义了一个停止前的钩子，其会在pod内部被调用，停止钩子一般定义了如何优雅的结束进程；

3、进程被发送TERM信号（kill -14）

4、当超过优雅退出的时间后，Pod中的所有进程都会被发送SIGKILL信号（kill -9）。

# 15. Kubernetes 中什么是静态 Pod？
静态Pod是由kubelet进行管理的仅存在于特定Node的Pod上，他们不能通过API Server进行管理，无法与ReplicationController、Deployment或者DaemonSet进行关联，并且kubelet无法对他们进行健康检查。静态Pod总是由kubelet进行创建，并且总是在kubelet所在的Node上运行。

# 16. Kubernetes 中 Pod 可能位于什么状态？
1）Pending：API Server已经创建该Pod，且Pod内还有一个或多个容器的镜像没有创建，包括正在下载镜像的过程。

2）Running：Pod内所有容器均已创建，且至少有一个容器处于运行状态、正在启动状态或正在重启状态。

3）Succeeded：Pod内所有容器均成功执行退出，且不会重启。

4）Failed：Pod内所有容器均已退出，但至少有一个容器退出为失败状态。

5）Unknown：由于某种原因无法获取该Pod状态，可能由于网络通信不畅导致。

#17. 描述 Kubernetes 创建一个 Pod 的主要流程？
Kubernetes中创建一个Pod涉及多个组件之间联动，主要流程如下：

- 客户端提交Pod的配置信息（可以是yaml文件定义的信息）到kube-apiserver。

- Apiserver收到指令后，通知给controller-manager创建一个资源对象。

- Controller-manager通过api-server将Pod的配置信息存储到etcd数据中心中。

- Kube-scheduler检测到Pod信息会开始调度预选，会先过滤掉不符合Pod资源配置要求的节点，然后开始调度调优，主要是挑选出更适合运行Pod的节点，然后将Pod的资源配置单发送到Node节点上的kubelet组件上。

- Kubelet根据scheduler发来的资源配置单运行Pod，运行成功后，将Pod的运行信息返回给scheduler，scheduler将返回的Pod运行状况的信息存储到etcd数据中心。

# 18. Kubernetes 中 Pod 有哪些重启策略？
Pod重启策略（RestartPolicy）应用于Pod内的所有容器，并且仅在Pod所处的Node上由kubelet进行判断和重启操作。当某个容器异常退出或者健康检查失败时，kubelet将根据RestartPolicy的设置来进行相应操作。

Pod的重启策略包括Always、OnFailure和Never，默认值为Always。

- Always：当容器失效时，由kubelet自动重启该容器；

- OnFailure：当容器终止运行且退出码不为0时，由kubelet自动重启该容器；

- Never：不论容器运行状态如何，kubelet都不会重启该容器。

同时Pod的重启策略与控制方式关联，当前可用于管理Pod的控制器包括ReplicationController、Job、DaemonSet及直接管理kubelet管理（静态Pod）。

不同控制器的重启策略限制如下：

- RC和DaemonSet：必须设置为Always，需要保证该容器持续运行；

- Job：OnFailure或Never，确保容器执行完成后不再重启；

- kubelet：在Pod失效时重启，不论将RestartPolicy设置为何值，也不会对Pod进行健康检查。

# 19. Kubernetes 中 Pod 有什么健康检查方式？
对Pod的健康检查可以通过两类探针来检查：LivenessProbe和ReadinessProbe。

LivenessProbe探针：用于判断容器是否存活（running状态），如果LivenessProbe探针探测到容器不健康，则kubelet将杀掉该容器，并根据容器的重启策略做相应处理。若一个容器不包含LivenessProbe探针，kubelet认为该容器的LivenessProbe探针返回值用于是“Success”。

ReadineeProbe探针：用于判断容器是否启动完成（ready状态）。如果ReadinessProbe探针探测到失败，则Pod的状态将被修改。Endpoint Controller将从Service的Endpoint中删除包含该容器所在Pod的Eenpoint。

startupProbe探针：启动检查机制，应用一些启动缓慢的业务，避免业务长时间启动而被上面两类探针kill掉。

# 20. Kubernetes Pod 如何实现对节点的资源控制？
Kubernetes集群里的节点提供的资源主要是计算资源，计算资源是可计量的能被申请、分配和使用的基础资源。当前Kubernetes集群中的计算资源主要包括CPU、GPU及Memory。CPU与Memory是被Pod使用的，因此在配置Pod时可以通过参数CPU Request及Memory Request为其中的每个容器指定所需使用的CPU与Memory量，Kubernetes会根据Request的值去查找有足够资源的Node来调度此Pod。

通常，一个程序所使用的CPU与Memory是一个动态的量，确切地说，是一个范围，跟它的负载密切相关：负载增加时，CPU和Memory的使用量也会增加。