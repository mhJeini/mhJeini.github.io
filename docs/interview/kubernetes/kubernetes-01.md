# 1. Kubernetes 是什么？
 Kubernetes，又称为k8s（首字母为k、首字母与尾字母之间有8个字符、尾字母为s，所以简称k8s）或者简称为 "kube"，Kubernetes这个名字源于希腊语，意为“舵手”或“飞行员”，是一种可自动实施Linux容器操作的开源平台。

Kubernetes可以帮助用户省去应用容器化过程的许多手动部署和扩展操作。也就是说，可以将运行Linux容器的多组主机聚集在一起，由Kubernetes来高效地管理这些集群。而且，这些集群可跨公共云、私有云或混合云部署主机。因此，对于要求快速扩展的云原生应用而言（例如借助Apache Kafka进行的实时数据流处理），Kubernetes是理想的托管平台。

Kubernetes最初由Google的工程师开发和设计，于2014年开源了Kubernetes项目。Google是最早研发Linux容器技术的企业之一（组建了cgroups），曾公开分享介绍Google如何将一切都运行于容器之中（这是 Google 云服务背后的技术）。Google每周会启用超过20亿个容器——全都由内部平台Borg支撑。Borg是Kubernetes的前身，多年来开发Borg的经验教训成了影响Kubernetes中许多技术的主要因素。

趣事：Kubernetes徽标的七个轮辐代表着项目最初的名称"九之七项目"（Project Seven of Nine）。

# 2. 为什么 Kubernetes 如此有用？
![](../../img/java/interview/kubernetes/1.png)

**传统部署时代：**

早期，各个组织机构在物理服务器上运行应用程序。无法为物理服务器中的应用程序定义资源边界，这会导致资源分配问题。 例如，如果在物理服务器上运行多个应用程序，则可能会出现一个应用程序占用大部分资源的情况， 结果可能导致其他应用程序的性能下降。 一种解决方案是在不同的物理服务器上运行每个应用程序，但是由于资源利用不足而无法扩展， 并且维护许多物理服务器的成本很高。

**虚拟化部署时代：**

作为解决方案，引入了虚拟化。虚拟化技术允许你在单个物理服务器的 CPU 上运行多个虚拟机（VM）。 虚拟化允许应用程序在 VM 之间隔离，并提供一定程度的安全，因为一个应用程序的信息 不能被另一应用程序随意访问。

虚拟化技术能够更好地利用物理服务器上的资源，并且因为可轻松地添加或更新应用程序 而可以实现更好的可伸缩性，降低硬件成本等等。

每个 VM 是一台完整的计算机，在虚拟化硬件之上运行所有组件，包括其自己的操作系统。

**容器部署时代：**

容器类似于 VM，但是它们具有被放宽的隔离属性，可以在应用程序之间共享操作系统（OS）。 因此，容器被认为是轻量级的。容器与 VM 类似，具有自己的文件系统、CPU、内存、进程空间等。 由于它们与基础架构分离，因此可以跨云和 OS 发行版本进行移植。

容器因具有许多优势而变得流行起来。下面列出的是容器的一些好处：

- 敏捷应用程序的创建和部署：与使用 VM 镜像相比，提高了容器镜像创建的简便性和效率。
- 持续开发、集成和部署：通过快速简单的回滚（由于镜像不可变性），支持可靠且频繁的 容器镜像构建和部署。
- 关注开发与运维的分离：在构建/发布时而不是在部署时创建应用程序容器镜像， 从而将应用程序与基础架构分离。
- 可观察性：不仅可以显示操作系统级别的信息和指标，还可以显示应用程序的运行状况和其他指标信号。
- 跨开发、测试和生产的环境一致性：在便携式计算机上与在云中相同地运行。
- 跨云和操作系统发行版本的可移植性：可在 Ubuntu、RHEL、CoreOS、本地、 Google Kubernetes Engine 和其他任何地方运行。
- 以应用程序为中心的管理：提高抽象级别，从在虚拟硬件上运行 OS 到使用逻辑资源在 OS 上运行应用程序。
- 松散耦合、分布式、弹性、解放的微服务：应用程序被分解成较小的独立部分， 并且可以动态部署和管理 - 而不是在一台大型单机上整体运行。
- 资源隔离：可预测的应用程序性能。
- 资源利用：高效率和高密度。
# 3. 为什么需要 Kubernetes，它能做什么？
容器是打包和运行应用程序的好方式。在生产环境中，管理运行应用程序的容器，并确保不会停机。例如，如果一个容器发生故障，则需要启动另一个容器。如果系统处理此行为，会不会更容易？

这就是Kubernetes来解决这些问题的方法！Kubernetes提供了一个可弹性运行分布式系统的框架。Kubernetes可以解决扩展要求、故障转移、部署模式等。 例如，Kubernetes可以轻松管理系统的Canary部署。

Kubernetes提供：

- 服务发现和负载均衡
Kubernetes可以使用DNS名称或自己的IP地址公开容器，如果进入容器的流量很大，Kubernetes可以负载均衡并分配网络流量，从而使部署稳定。

- 存储编排
  Kubernetes支持自动挂载存储系统，例如本地存储、公共云提供商等。

- 自动部署和回滚

  可以使用Kubernetes描述已部署容器的所需状态，它可以以受控的速率将实际状态 更改为期望状态。例如，可以自动化Kubernetes来实现部署创建新容器， 删除现有容器并将它们的所有资源用于新容器。

- 自动完成装箱计算

  Kubernetes允许指定每个容器所需CPU和内存（RAM）。 当容器指定了资源请求时，Kubernetes可以做出更好的决策来管理容器的资源。

- 自我修复

  Kubernetes 重新启动失败的容器、替换容器、杀死不响应用户定义的 运行状况检查的容器，并且在准备好服务之前不将其通告给客户端。

- 密钥与配置管理

  Kubernetes允许存储和管理敏感信息，例如密码、OAuth 令牌和 ssh 密钥。 可以在不重建容器镜像的情况下部署和更新密钥和应用程序配置，也无需在堆栈配置中暴露密钥。

# 4. Kubernetes 不是什么？
Kubernetes不是传统的、包罗万象的PaaS（平台即服务）系统。由于Kubernetes在容器级别而不是在硬件级别运行，它提供了PaaS产品共有的一些普遍适用的功能，例如部署、扩展、负载均衡、日志记录和监视。但是，Kubernetes不是单体系统，默认解决方案都是可选和可插拔的。Kubernetes提供了构建开发人员平台的基础，但是在重要的地方保留了用户的选择和灵活性。

Kubernetes：

- 不限制支持的应用程序类型。Kubernetes旨在支持极其多种多样的工作负载，包括无状态、有状态和数据处理工作负载。如果应用程序可以在容器中运行，那么它应该可以在Kubernetes上很好地运行。
- 不部署源代码，也不构建你的应用程序。持续集成(CI)、交付和部署（CI/CD）工作流取决于组织的文化和偏好以及技术要求。
- 不提供应用程序级别的服务作为内置服务，例如中间件（例如，消息中间件）、数据处理框架（例如，Spark）、数据库（例如，mysql）、缓存、集群存储系统（例如，Ceph）。这样的组件可以在Kubernetes上运行，并且/或者可以由运行在Kubernetes上的应用程序通过可移植机制（例如，开放服务代理）来访问。
- 不要求日志记录、监视或警报解决方案。它提供了一些集成作为概念证明，并提供了收集和导出指标的机制。
- 不提供或不要求配置语言/系统（例如jsonnet），它提供了声明性API，该声明性API可以由任意形式的声明性规范所构成。
- 不提供也不采用任何全面的机器配置、维护、管理或自我修复系统。
此外，Kubernetes不仅仅是一个编排系统，实际上它消除了编排的需要。编排的技术定义是执行已定义的工作流程：首先执行A，然后执行B，再执行C。相比之下，Kubernetes包含一组独立的、可组合的控制过程，这些过程连续地将当前状态驱动到所提供的所需状态。如何从A到C的方式无关紧要，也不需要集中控制，这使得系统更易于使用且功能更强大、系统更健壮、更为弹性和可扩展。
# 5. Kubernetes 有哪些应用功能？借助什么开源项目？
在生产环境中（尤其是面向云优化应用开发时）使用Kubernetes的主要优势在于，它提供了一个便捷有效的平台，可以在物理机和虚拟机集群上调度和运行容器。更广泛一点说，它可以帮助用户在生产环境中，完全实施并依托基于容器的基础架构运营。由于Kubernetes的实质在于实现操作任务自动化，所以可以将其它应用平台或管理系统分配给的许多相同任务交给容器来执行。

利用Kubernetes，能够达成以下目标：

- 跨多台主机进行容器编排。
- 更加充分地利用硬件，最大程度获取运行企业应用所需的资源。
- 有效管控应用部署和更新，并实现自动化操作。
- 挂载和增加存储，用于运行有状态的应用。
- 快速、按需扩展容器化应用及其资源。
- 对服务进行声明式管理，保证所部署的应用始终按照部署的方式运行。
- 利用自动布局、自动重启、自动复制以及自动扩展功能，对应用实施状况检查和自我修复。
但是，Kubernetes需要依赖其它项目来全面提供这些经过编排的服务。因此，借助其它开源项目可以有效的将Kubernetes的全部功用发挥出来。这些功能包括：

- 注册表，通过Atomic注册表或Docker注册表等项目实现。
- 联网，通过OpenvSwitch和智能边缘路由等项目实现。
- 遥测，通过heapster、kibana、hawkular和elastic等项目实现。
- 安全性，通过LDAP、SELinux、RBAC和OAUTH等项目以及多租户层来实现。
- 自动化，参照Ansible手册进行安装和集群生命周期管理。
- 服务，可通过自带预建版常用应用模式的丰富内容目录来提供。
# 6. 解释一下 Kubernetes 相关术语？
Kubernetes和其它技术一样，Kubernetes也会采用一些专用的词汇，这可能会对初学者理解和掌握这项技术造成一定的障碍。为了帮助大家了解Kubernetes，下面来解释一些常用术语。

**主机（Master）**： 用于控制 Kubernetes节点的计算机。所有任务分配都来自于此。

**节点（Node）**： 负责执行请求和所分配任务的计算机。由 Kubernetes主机负责对节点进行控制。

**容器集（Pod）**： 被部署在单个节点上的，且包含一个或多个容器的容器组。同一容器集中的所有容器共享同一个 IP 地址、IPC、主机名称及其它资源。容器集会将网络和存储从底层容器中抽象出来。这样，您就能更加轻松地在集群中移动容器。

**复制控制器（Replication controller）**： 用于控制应在集群某处运行的完全相同的容器集副本数量。

**服务（Service）**： 将工作内容与容器集分离。Kubernetes服务代理会自动将服务请求分发到正确的容器集——无论这个容器集会移到集群中的哪个位置，甚至可以被替换掉。

**Kubelet**： 运行在节点上的服务，可读取容器清单（container manifest），确保指定的容器启动并运行。

**kubectl**： Kubernetes的命令行配置工具。

# 7. 你对 Kubernetes 的负载均衡器有什么了解？
负载均衡器是暴露服务的最常见和标准方式之一。

根据工作环境使用两种类型的负载均衡器，即内部负载均衡器或外部负载均衡器。内部负载均衡器自动平衡负载并使用所需配置分配容器，而外部负载均衡器将流量从外部负载引导至后端容器。

# 8. Kubenetes 架构的组成是什么？
和大多数分布式系统一样，K8S集群至少需要一个主节点（Master）和多个计算节点（Node）。

主节点主要用于暴露API，调度部署和节点的管理；

计算节点运行一个容器运行环境，一般是docker环境（类似docker环境的还有rkt），同时运行一个K8s的代理（kubelet）用于和master通信。计算节点也会运行一些额外的组件，像记录日志，节点监控，服务发现等等。计算节点是k8s集群中真正工作的节点。

K8S架构细分：

**1、Master节点（默认不参加实际工作）：**
Kubectl：客户端命令行工具，作为整个K8s集群的操作入口；

Api Server：在K8s架构中承担的是“桥梁”的角色，作为资源操作的唯一入口，它提供了认证、授权、访问控制、API注册和发现等机制。客户端与k8s群集及K8s内部组件的通信，都要通过Api Server这个组件；

Controller-manager：负责维护群集的状态，比如故障检测、自动扩展、滚动更新等；

Scheduler：负责资源的调度，按照预定的调度策略将pod调度到相应的node节点上；

Etcd：担任数据中心的角色，保存了整个群集的状态。

**2、Node节点：**
Kubelet：负责维护容器的生命周期，同时也负责Volume和网络的管理，一般运行在所有的节点，是Node节点的代理，当Scheduler确定某个node上运行pod之后，会将pod的具体信息（image，volume）等发送给该节点的kubelet，kubelet根据这些信息创建和运行容器，并向master返回运行状态。（自动修复功能：如果某个节点中的容器宕机，它会尝试重启该容器，若重启无效，则会将该pod杀死，然后重新创建一个容器）；

Kube-proxy：Service在逻辑上代表了后端的多个pod。负责为Service提供cluster内部的服务发现和负载均衡（外界通过Service访问pod提供的服务时，Service接收到的请求后就是通过kube-proxy来转发到pod上的）；

container-runtime：是负责管理运行容器的软件，比如docker

Pod：是k8s集群里面最小的单位。每个pod里边可以运行一个或多个container（容器），如果一个pod中有两个container，那么container的USR（用户）、MNT（挂载点）、PID（进程号）是相互隔离的，UTS（主机名和域名）、IPC（消息队列）、NET（网络栈）是相互共享的。我比较喜欢把pod来当做豌豆夹，而豌豆就是pod中的container。

# 9. Kubernetes 有什么缺点或不足之处？
Kubernetes当前存在的缺点（不足）如下：

- 安装过程和配置相对困难复杂。

- 管理服务相对繁琐。

- 运行和编译需要很多时间。

- 它比其他替代品更昂贵。

- 对于简单的应用程序来说，可能不需要涉及Kubernetes即可满足。

# 10. 说一下 Kubenetes 针对 Pod 资源对象的健康监测机制？
K8s中对于pod资源对象的健康状态检测，提供了三类probe（探针）来执行对pod的健康监测：

**1）livenessProbe探针**

可以根据用户自定义规则来判定pod是否健康，如果livenessProbe探针探测到容器不健康，则kubelet会根据其重启策略来决定是否重启，如果一个容器不包含livenessProbe探针，则kubelet会认为容器的livenessProbe探针的返回值永远成功。

**2）ReadinessProbe探针**

同样是可以根据用户自定义规则来判断pod是否健康，如果探测失败，控制器会将此pod从对应service的endpoint列表中移除，从此不再将任何请求调度到此Pod上，直到下次探测成功。

**3）startupProbe探针**

启动检查机制，应用一些启动缓慢的业务，避免业务长时间启动而被上面两类探针kill掉，这个问题也可以换另一种方式解决，就是定义上面两类探针机制时，初始化时间定义的长一些即可。

每种探测方法能支持以下几个相同的检查参数，用于设置控制检查时间：

initialDelaySeconds：初始第一次探测间隔，用于应用启动的时间，防止应用还没启动而健康检查失败

periodSeconds：检查间隔，多久执行probe检查，默认为10s；

timeoutSeconds：检查超时时长，探测应用timeout后为失败；

successThreshold：成功探测阈值，表示探测多少次为健康正常，默认探测1次。

上面两种探针都支持以下三种探测方法：

1）Exec：通过执行命令的方式来检查服务是否正常，比如使用cat命令查看pod中的某个重要配置文件是否存在，若存在，则表示pod健康。反之异常。

Exec探测方式的yaml文件语法如下：
```yaml
spec:
containers:
  - name: liveness
    image: k8s.gcr.io/busybox
    args:
  - /bin/sh
  - -c
  - touch /tmp/healthy; sleep 30; rm -rf /tmp/healthy; sleep 600
    livenessProbe: #选择livenessProbe的探测机制
    exec: #执行以下命令
    command:
  - cat
  - /tmp/healthy
    initialDelaySeconds: 5 #在容器运行五秒后开始探测
    periodSeconds: 5 #每次探测的时间间隔为5秒
 
```
在上面的配置文件中，探测机制为在容器运行5秒后，每隔五秒探测一次，如果cat命令返回的值为“0”，则表示健康，如果为非0，则表示异常。

2）Httpget：通过发送http/htps请求检查服务是否正常，返回的状态码为200-399则表示容器健康（注http get类似于命令curl -I）。

Httpget探测方式的yaml文件语法如下：
```yaml
spec:
containers:
- name: liveness
  image: k8s.gcr.io/liveness
  livenessProbe: #采用livenessProbe机制探测
  httpGet: #采用httpget的方式
  scheme:HTTP #指定协议，也支持https
  path: /healthz #检测是否可以访问到网页根目录下的healthz网页文件
  port: 8080 #监听端口是8080
  initialDelaySeconds: 3 #容器运行3秒后开始探测
  periodSeconds: 3 #探测频率为3秒
```
上述配置文件中，探测方式为项容器发送HTTP GET请求，请求的是8080端口下的healthz文件，返回任何大于或等于200且小于400的状态码表示成功。任何其他代码表示异常。

3）tcpSocket：通过容器的IP和Port执行TCP检查，如果能够建立TCP连接，则表明容器健康，这种方式与HTTPget的探测机制有些类似，tcpsocket健康检查适用于TCP业务。

tcpSocket探测方式的yaml文件语法如下：
```yaml
spec:
containers:
- name: goproxy
  image: k8s.gcr.io/goproxy:0.1
  ports:
- containerPort: 8080
#这里两种探测机制都用上了，都是为了和容器的8080端口建立TCP连接
readinessProbe:
tcpSocket:
port: 8080
initialDelaySeconds: 5
periodSeconds: 10
livenessProbe:
tcpSocket:
port: 8080
initialDelaySeconds: 15
periodSeconds: 20
```
在上述的yaml配置文件中，两类探针都使用了，在容器启动5秒后，kubelet将发送第一个readinessProbe探针，这将连接容器的8080端口，如果探测成功，则该pod为健康，十秒后，kubelet将进行第二次连接。

除了readinessProbe探针外，在容器启动15秒后，kubelet将发送第一个livenessProbe探针，仍然尝试连接容器的8080端口，如果连接失败，则重启容器。

探针探测的结果无外乎以下三者之一：

Success：Container通过了检查；

Failure：Container没有通过检查；

Unknown：没有执行检查，因此不采取任何措施（通常是我们没有定义探针检测，默认为成功）。

更多资料：[https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/)

# 11. Kubenetes 如何控制滚动更新过程？
可以通过下面的命令查看到更新时可以控制的参数：
```shell
[root@JingXuan_Master yaml]# kubectl explain deploy.spec.strategy.rollingUpdate
```
maxSurge： 此参数控制滚动更新过程，副本总数超过预期pod数量的上限。可以是百分比，也可以是具体的值。默认为1。

上述参数的作用就是在更新过程中，值若为3，那么不管三七二一，先运行三个pod，用于替换旧的pod，以此类推。

maxUnavailable： 此参数控制滚动更新过程中，不可用的Pod的数量。这个值和上面的值没有任何关系。

举例：我有十个pod，但是在更新的过程中，我允许这十个pod中最多有三个不可用，那么就将这个参数的值设置为3，在更新的过程中，只要不可用的pod数量小于或等于3，那么更新过程就不会停止。

# 12. K8s 中镜像的下载策略是什么？
通过kubectl explain pod.spec.containers命令，来查看imagePullPolicy这行的解释。

K8s的镜像下载策略有三种：Always、Never、IFNotPresent；

Always：镜像标签为latest时，总是从指定的仓库中获取镜像；

Never：禁止从仓库中下载镜像，也就是说只能使用本地镜像；

IfNotPresent：仅当本地没有对应镜像时，才从目标仓库中下载。

默认的镜像下载策略是：当镜像标签是latest时，默认策略是Always；当镜像标签是自定义时（也就是标签不是latest），那么默认策略是IfNotPresent。

# 13. Kubernetes 版本回滚相关的命令？
运行yaml文件，并记录版本信息
```shell
[root@master httpd-web]# kubectl apply -f httpd2-deploy1.yaml --record
```
查看该deployment的历史版本
```shell
[root@master httpd-web]# kubectl rollout history deployment httpd-devploy1
```
执行回滚操作，指定回滚到版本1
```shell
[root@master httpd-web]# kubectl rollout undo deployment httpd-devploy1 --to-revision=1
```
在yaml文件的spec字段中，可以写以下选项（用于限制最多记录多少个历史版本）：
```yaml
spec:
  revisionHistoryLimit: 5
```
# 14. K8s 标签与标签选择器的作用是什么？
标签：是当相同类型的资源对象越来越多的时候，为了更好的管理，可以按照标签将其分为一个组，为的是提升资源对象的管理效率。

标签选择器：就是标签的查询过滤条件。目前API支持两种标签选择器：

基于等值关系的，如：'“=”、“”“\==”、“! =”'（注：“==”也是等于的意思，yaml文件中的matchLabels字段）；

基于集合的，如：in、notin、exists（yaml文件中的matchExpressions字段）；

>注：in:在这个集合中； notin：不在这个集合中； exists：要么全在（exists）这个集合中，要么都不在（notexists）。

使用标签选择器的操作逻辑：

在使用基于集合的标签选择器同时指定多个选择器之间的逻辑关系为“与”操作（比如：'- {key: name,operator: In,values: [zhangsan,lisi]}' ，那么只要拥有这两个值的资源，都会被选中）；

使用空值的标签选择器，意味着每个资源对象都被选中（如：标签选择器的键是“A”，两个资源对象同时拥有A这个键，但是值不一样，这种情况下，如果使用空值的标签选择器，那么将同时选中这两个资源对象）

空的标签选择器（注意不是上面说的空值，而是空的，都没有定义键的名称），将无法选择出任何资源；

在基于集合的选择器中，使用“In”或者“Notin”操作时，其values可以为空，但是如果为空，这个标签选择器，就没有任何意义了。

两种标签选择器类型（基于等值、基于集合的书写方法）：
```yaml
selector:
  matchLabels: #基于等值
    app: nginx
    matchExpressions: #基于集合
      - {key: name,operator: In,values: [zhangsan,lisi]} #key、operator、values这三个字段是固定的
      - {key: age,operator: Exists,values:} #如果指定为exists，那么values的值一定要为空。
```
# 15. K8s 常用的标签分类有哪些？
标签分类是可以自定义的，但是为了能使他人可以达到一目了然的效果，一般会使用以下一些分类：

版本类标签（release）：stable（稳定版）、canary（金丝雀版本，可以将其称之为测试版中的测试版）、beta（测试版）；

环境类标签（environment）：dev（开发）、qa（测试）、production（生产）、op（运维）；

应用类（app）：ui、as、pc、sc；

架构类（tier）：frontend（前端）、backend（后端）、cache（缓存）；

分区标签（partition）：customerA（客户A）、customerB（客户B）；

品控级别（Track）：daily（每天）、weekly（每周）。

# 16. K8s 有几种查看标签的方式？
常用的有以下三种查看方式：
```shell
[root@jingxuan_master ~]# kubectl get pod --show-labels #查看pod，并且显示标签内容
[root@jingxuan_master ~]# kubectl get pod -L env,tier #显示资源对象标签的值
[root@jingxuan_master ~]# kubectl get pod -l env,tier #只显示符合键值资源对象的pod，而“-L”是显示所有的pod
```
# 17. K8s 添加、修改、删除标签的命令？
对pod标签的操作命令
```shell
[root@master ~]# kubectl label pod label-pod abc=123 #给名为label-pod的pod添加标签
[root@master ~]# kubectl label pod label-pod abc=456 --overwrite #修改名为label-pod的标签
[root@master ~]# kubectl label pod label-pod abc- #删除名为label-pod的标签
[root@master ~]# kubectl get pod --show-labels
```
对node节点的标签操作命令
```shell
[root@master ~]# kubectl label nodes node01 disk=ssd #给节点node01添加disk标签
[root@master ~]# kubectl label nodes node01 disk=sss –overwrite #修改节点node01的标签
[root@master ~]# kubectl label nodes node01 disk- #删除节点node01的disk标签
```
# 18. DaemonSet 资源对象的特性？
DaemonSet这种资源对象会在每个k8s集群中的节点上运行，并且每个节点只能运行一个pod，这是它和deployment资源对象的最大也是唯一的区别。所以，在其yaml文件中，不支持定义replicas，除此之外，与Deployment、RS等资源对象的写法相同。

它的一般使用场景如下：

在去做每个节点的日志收集工作；

监控每个节点的的运行状态。

# 19. kube-proxy ipvs 和 iptables 有什么异同？
iptables与IPVS都是基于Netfilter实现的，但因为定位不同，二者有着本质的差别：iptables是为防火墙而设计的；IPVS则专门用于高性能负载均衡，并使用更高效的数据结构（Hash表），允许几乎无限的规模扩张。

与iptables相比，IPVS拥有以下明显优势：

- 为大型集群提供了更好的可扩展性和性能；

- 支持比iptables更复杂的复制均衡算法（最小负载、最少连接、加权等）；

- 支持服务器健康检查和连接重试等功能；

- 可以动态修改ipset的集合，即使iptables的规则正在使用这个集合。

# 20. 简述 kube-proxy iptables 的原理？
Kubernetes从1.2版本开始，将iptables作为kube-proxy的默认模式。iptables模式下的kube-proxy不再起到Proxy的作用，其核心功能：通过API Server的Watch接口实时跟踪Service与Endpoint的变更信息，并更新对应的iptables规则，Client的请求流量则通过iptables的NAT机制“直接路由”到目标Pod。