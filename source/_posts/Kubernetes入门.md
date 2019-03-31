---
title: Kubernetes入门
categories:
  - 容器
  - 集群管理
tags:
  - 虚拟化
  - Kubernetes
  - 容器
date: 2019-03-31 09:51:53
---

# Kubernetes 简介

Kubernetes，又称为 k8s（首字母为 k、首字母与尾字母之间有 8 个字符、尾字母为 s，所以简称 k8s）或者简称为 “kube” ，是一种可自动实施 Linux 容器操作的开源平台。它可以帮助用户省去应用容器化过程的许多手动部署和扩展操作。也就是说，您可以将运行 Linux 容器的多组主机聚集在一起，由 Kubernetes 帮助您轻松高效地管理这些集群。而且，这些集群可跨公共云、私有云或混合云部署主机。因此，对于要求快速扩展的云原生应用而言（例如借助 Apache Kafka 进行的实时数据流处理），Kubernetes 是理想的托管平台。

Kubernetes 最初由 Google 的工程师开发和设计。Google 是最早研发 Linux 容器技术的企业之一，曾公开分享介绍 Google 如何将一切都运行于容器之中（这是 Google 云服务背后的技术）。Google 每周会启用超过 20 亿个容器——全都由内部平台 Borg 支撑。Borg 是 Kubernetes 的前身，多年来开发 Borg 的经验教训成了影响 Kubernetes 中许多技术的主要因素。

趣闻：Kubernetes 徽标的七个轮辐代表着项目最初的名称“九之七项目”（Project Seven of Nine）。

红帽是第一批与 Google 合作研发 Kubernetes 的公司之一，作为 Kubernetes 上游项目的第二大贡献者，我们甚至在这个项目启动之前就已参与其中。2015 年，Google 将 Kubernetes 项目捐赠给了新成立的云原生计算基金会。

[认识 Kubernetes?](https://kubernetes.io/zh/docs/concepts/overview/what-is-kubernetes/)


## 您为何需要 Kubernetes？

真正的生产型应用会涉及多个容器。这些容器必须跨多个服务器主机进行部署。Kubernetes 可以提供所需的编排和管理功能，以便您针对这些工作负载大规模部署容器。借助 Kubernetes 编排功能，您可以构建跨多个容器的应用服务、跨集群调度、扩展这些容器，并长期持续管理这些容器的健康状况。

Kubernetes 还需要与联网、存储、安全性、遥测和其他服务集成整合，以提供全面的容器基础架构。

![kubernetes](https://www.redhat.com/cms/managed-files/kubernetes-diagram-902x416.png)

当然，这取决于您如何在您的环境中使用容器。Linux 容器中的基本应用将它们视作高效、快速的虚拟机。一旦把它部署到生产环境或扩展为多个应用，您显然需要许多托管在相同位置的容器来协同提供各种服务。随着这些容器的累积，您运行环境中容器的数量会急剧增加，复杂度也随之增长。

Kubernetes 通过将容器分类组成 “容器集” （pod），解决了容器增殖带来的许多常见问题容器集为分组容器增加了一个抽象层，可帮助您调用工作负载，并为这些容器提供所需的联网和存储等服务。Kubernetes 的其它部分可帮助您在这些容器集之间达成负载平衡，同时确保运行正确数量的容器，充分支持您的工作负载。

如果能正确实施 Kubernetes，再辅以其它开源项目（例如 [Atomic 注册表](http://www.projectatomic.io)、[Open vSwitch](http://openvswitch.org/)、[heapster](https://github.com/kubernetes/heapster)、[OAuth](https://oauth.net) 以及 [SELinux](https://selinuxproject.org/page/Main_Page)），您就能够轻松编排容器基础架构的各个部分。

## Kubernetes 有哪些用途？

在您生产环境中（尤其是当[您要面向云优化应用开发时](https://www.redhat.com/zh/topics/cloud-native-apps)）使用 Kubernetes 的主要优势在于，它提供了一个便捷有效的平台，让您可以在物理机和虚拟机集群上调度和运行容器。更广泛一点说，它可以帮助您在生产环境中，完全实施并依托基于容器的基础架构运营。由于 Kubernetes 的实质在于实现操作任务自动化，所以您可以将其它应用平台或管理系统分配给您的许多相同任务交给容器来执行。

利用 Kubernetes，您能够达成以下目标：

*   跨多台主机进行容器编排。
*   更加充分地利用硬件，最大程度获取运行企业应用所需的资源。
*   有效管控应用部署和更新，并实现自动化操作。
*   挂载和增加存储，用于运行有状态的应用。
*   快速、按需扩展容器化应用及其资源。
*   对服务进行声明式管理，保证所部署的应用始终按照部署的方式运行。
*   利用自动布局、自动重启、自动复制以及自动扩展功能，对应用实施状况检查和自我修复。

## 学习 Kubernetes 术语

和其它技术一样，Kubernetes 也会采用一些专用的词汇，这可能会对初学者理解和掌握这项技术造成一定的障碍。为了帮助您了解 Kubernetes，我们在下面来解释一些常用术语。

* 主机（Master）
用于控制 Kubernetes 节点的计算机。所有任务分配都来自于此。

* 节点（Node）
负责执行请求和所分配任务的计算机。由 Kubernetes 主机负责对节点进行控制。

* 容器集（Pod）
部署在单个节点上的，且包含一个或多个容器的容器组。同一容器集中的所有容器共享同一个 IP 地址、IPC、主机名称及其它资源。容器集会将网络和存储从底层容器中抽象出来。这样，您就能更加轻松地在集群中移动容器。

* 复制控制器（Replication controller）
用于控制容器集在集群上运行的实例数量。

* 服务（Service）
将工作内容与容器集分离。Kubernetes 服务代理会自动将服务请求分发到正确的容器集——无论这个容器集会移到集群中的哪个位置，甚至可以被替换掉。

* Kubelet
运行在节点上的服务，可读取容器清单（container manifest），确保指定的容器启动并运行。

* kubectl
Kubernetes 的命令行配置工具。

[想要了解更多？查看 Kubernetes 术语表。](https://kubernetes.io/docs/reference/)


# Kubernetes架构

## 整体架构

K8s设置由几个部分组成，其中一些是可选的，一些是整个系统运行所必需的。下面是k8s的全局架构图

![](https://user-gold-cdn.xitu.io/2018/8/29/16584ccc8cc45050?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



## master和node架构

k8s的集群组件如下：

master: apiserver,scheduler,controller\-manager,etcd

node:kubelet(agent),kube\-proxy,docker(container engine)

Registry:harbor，属于集群外部的

Addons(附件)：kube\-dns,UI(如 dashboard)等等。在集群运行正常后，在集群上运行pod实现。

![](https://upload-images.jianshu.io/upload_images/6943703-635819b959d1f300.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 集群环境架构

注意，下图的ip可根据实际环境调整

![](https://upload-images.jianshu.io/upload_images/6943703-3b5a09edcd957797.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


Kubernetes有两个不同的部分构成，一个是Master，一个是Node。Master负责调度资源和为客户端提供API，客户端可以是UI界面或者CLI工具，在Kubernetes中CLI工具通常为kubectl。 Kubernetes Master接受使用YAML定义的配置文件，根据配置文件中相关信息将容器分配到其中一个Node上。另外，镜像库在Kubernetes中也起到一个很重要的角色，Kubernetes需要从镜像库中拉取镜像基于这个镜像的容器才能成功启动。常用的镜像库有dockerhub、阿里云镜像库等。下面图片为Master的架构图：

![](https://user-gold-cdn.xitu.io/2018/8/29/16584cd113cca3e7?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

Master有三个组件：API Server、Scheduler、Controller。API Server提供了友好易用的API供外部调用，同时有很多强大的工具使得API调用更加简单，如kubectl封装了大量API调用，使得部署、配置更加简单。Kubernetes\-dashboard可以让用户在界面上操作Kubernetes，而无需手动输入各个API的调用地址参数等信息。

当API Server收到部署请求后，Scheduler会根据所需的资源，判断各节点的资源占用情况分配合适的Node给新的容器。判断依据包括内存、CPU、磁盘等。

Controller负责整个集群的整体协调和健康，保证每个组件以正确的方式运行。

在图的最下边是ETCD数据库。如前文所述ETCD是分布式存储数据库，其作为Kubernetes的中央数据库，存储了集群的状态，组件可以通过查询ETCD了解集群的状态。

Kubernetes Master分配容器到Node执行，Node将会承受压力，通常情况下新容器不会运行在Master上。或者说Master是不可调度的，但是你也可以选择把Master同时也作为Node,但是这并不是地道的用法。下面的为Node的架构图:

![](https://user-gold-cdn.xitu.io/2018/8/29/16584cd561b5ee60?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

Kube\-proxy在Node中管理网络，其左右至关重要。Kube\-proxy通过管理iptables等方式使得pod到pod之间，和pod到node之间网络能够互通。实质上在跨主机的pod之间网络也能够互通。

Kubelet负责向api server报告信息，并把健康状态、指标和节点状态信息存入ETCD中。

Supervisord保证Docker和kubelet一直在运行中，supervisord并不是必须组件，可以使用其他类似组件替换。

Pod是可以在Kubernetes中创建和管理的最小可部署计算单元。一个POD中可以包含多个容器，但Kubernetes仅管理pod。如果多个容器运行在一个POD中，就相当于这些容器运行在同一台主机中，需要注意端口占用问题。

参考资料:

[k8s.docker8.com/](https://link.juejin.im?target=http%3A%2F%2Fk8s.docker8.com%2F) 
[www.youtube.com/watch?v=zeS…](https://link.juejin.im?target=https%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3DzeS6OyDoy78)


# 核心概念

### 集群

集群是一组节点，这些节点可以是物理服务器或者虚拟机，之上安装了Kubernetes平台。下图展示这样的集群。注意该图为了强调核心概念有所简化。[这里](http://kubernetes.io/v1.1/docs/design/architecture.html)可以看到一个典型的Kubernetes架构图。

[![1.png](http://dockone.io/uploads/article/20151230/d56441427680948fb56a00af57bda690.png "1.png")](http://dockone.io/uploads/article/20151230/d56441427680948fb56a00af57bda690.png)

上图可以看到如下组件，使用特别的图标表示Service和Label：

*   Pod
*   Container（容器）
*   Label(![label](http://omerio.com/wp-content/uploads/2015/12/label.png))（标签）
*   Replication Controller（复制控制器）
*   Service（![enter image description here](http://omerio.com/wp-content/uploads/2015/12/service.png)）（服务）
*   Node（节点）
*   Kubernetes Master（Kubernetes主节点）

### Pod

[Pod](http://kubernetes.io/v1.1/docs/user-guide/pods.html)（上图绿色方框）安排在节点上，包含一组容器和卷。同一个Pod里的容器共享同一个网络命名空间，可以使用localhost互相通信。Pod是短暂的，不是持续性实体。你可能会有这些问题：

*   如果Pod是短暂的，那么我怎么才能持久化容器数据使其能够跨重启而存在呢？ 是的，Kubernetes支持[卷](http://kubernetes.io/v1.1/docs/user-guide/volumes.html)的概念，因此可以使用持久化的卷类型。
*   是否手动创建Pod，如果想要创建同一个容器的多份拷贝，需要一个个分别创建出来么？可以手动创建单个Pod，但是也可以使用Replication Controller使用Pod模板创建出多份拷贝，下文会详细介绍。
*   如果Pod是短暂的，那么重启时IP地址可能会改变，那么怎么才能从前端容器正确可靠地指向后台容器呢？这时可以使用Service，下文会详细介绍。

### Lable

正如图所示，一些Pod有Label（![enter image description here](http://omerio.com/wp-content/uploads/2015/12/label.png)）。一个Label是attach到Pod的一对键/值对，用来传递用户定义的属性。比如，你可能创建了一个"tier"和“app”标签，通过Label（**tier=frontend, app=myapp**）来标记前端Pod容器，使用Label（**tier=backend, app=myapp**）标记后台Pod。然后可以使用 [Selectors](http://kubernetes.io/v1.1/docs/user-guide/labels.html#label-selectors)选择带有特定Label的Pod，并且将Service或者Replication Controller应用到上面。

### Replication Controller

*是否手动创建Pod，如果想要创建同一个容器的多份拷贝，需要一个个分别创建出来么，能否将Pods划到逻辑组里？*

Replication Controller确保任意时间都有指定数量的Pod“副本”在运行。如果为某个Pod创建了Replication Controller并且指定3个副本，它会创建3个Pod，并且持续监控它们。如果某个Pod不响应，那么Replication Controller会替换它，保持总数为3.如下面的动画所示：

[![2.gif](http://dockone.io/uploads/article/20151230/5e2bad1a25e33e2d155da81da1d3a54b.gif "2.gif")](http://dockone.io/uploads/article/20151230/5e2bad1a25e33e2d155da81da1d3a54b.gif)

如果之前不响应的Pod恢复了，现在就有4个Pod了，那么Replication Controller会将其中一个终止保持总数为3。如果在运行中将副本总数改为5，Replication Controller会立刻启动2个新Pod，保证总数为5。还可以按照这样的方式缩小Pod，这个特性在执行滚动[升级](https://cloud.google.com/container-engine/docs/replicationcontrollers/#rolling_updates)时很有用。

当创建Replication Controller时，需要指定两个东西：

1.  [Pod模板](http://kubernetes.io/v1.1/docs/user-guide/replication-controller.html#pod-template)：用来创建Pod副本的模板
2.  [Label](http://kubernetes.io/v1.1/docs/user-guide/replication-controller.html#labels)：Replication Controller需要监控的Pod的标签。

现在已经创建了Pod的一些副本，那么在这些副本上如何均衡负载呢？我们需要的是Service。

### Service

*如果Pods是短暂的，那么重启时IP地址可能会改变，怎么才能从前端容器正确可靠地指向后台容器呢？*

[Service](http://kubernetes.io/v1.1/docs/user-guide/services.html)是定义一系列Pod以及访问这些Pod的策略的一层**抽象**。Service通过Label找到Pod组。因为Service是抽象的，所以在图表里通常看不到它们的存在，这也就让这一概念更难以理解。

现在，假定有2个后台Pod，并且定义后台Service的名称为‘backend\-service’，lable选择器为（**tier=backend, app=myapp**）。*backend\-service* 的Service会完成如下两件重要的事情：

*
    会为Service创建一个本地集群的DNS入口，因此前端Pod只需要DNS查找主机名为 ‘backend\-service’，就能够解析出前端应用程序可用的IP地址。
*
    现在前端已经得到了后台服务的IP地址，但是它应该访问2个后台Pod的哪一个呢？Service在这2个后台Pod之间提供透明的负载均衡，会将请求分发给其中的任意一个（如下面的动画所示）。通过每个Node上运行的代理（kube\-proxy）完成。[这里](http://kubernetes.io/v1.1/docs/user-guide/services.html#virtual-ips-and-service-proxies)有更多技术细节。

下述动画展示了Service的功能。注意该图作了很多简化。如果不进入网络配置，那么达到透明的负载均衡目标所涉及的底层网络和路由相对先进。如果有兴趣，[这里](http://www.dasblinkenlichten.com/kubernetes-101-networking/)有更深入的介绍。

[![3.gif](http://dockone.io/uploads/article/20151230/125bbccce0b3bbf42abab0e520d9250b.gif "3.gif")](http://dockone.io/uploads/article/20151230/125bbccce0b3bbf42abab0e520d9250b.gif)

有一个特别类型的Kubernetes Service，称为'[LoadBalancer](http://kubernetes.io/v1.1/docs/user-guide/services.html#type-loadbalancer)'，作为外部负载均衡器使用，在一定数量的Pod之间均衡流量。比如，对于负载均衡Web流量很有用。

### Node

节点（上图橘色方框）是物理或者虚拟机器，作为Kubernetes worker，通常称为Minion。每个节点都运行如下Kubernetes关键组件：

*   Kubelet：是主节点代理。
*   Kube\-proxy：Service使用其将链接路由到Pod，如上文所述。
*   Docker或Rocket：Kubernetes使用的容器技术来创建容器。

### Kubernetes Master

集群拥有一个Kubernetes Master（紫色方框）。Kubernetes Master提供集群的独特视角，并且拥有一系列组件，比如Kubernetes API Server。API Server提供可以用来和集群交互的REST端点。master节点包括用来创建和复制Pod的Replication Controller。




# 基础组件

## 核心组件

Kubernetes 主要由以下几个核心组件组成：

*   etcd：保存了整个集群的状态；
*   apiserver：提供了资源操作的唯一入口，并提供认证、授权、访问控制、API 注册和发现等机制；
*   controller manager：负责维护集群的状态，比如故障检测、自动扩展、滚动更新等；
*   scheduler：负责资源的调度，按照预定的调度策略将 Pod 调度到相应的机器上；
*   kubelet：负责维护容器的生命周期，同时也负责 Volume（CVI）和网络（CNI）的管理；
*   Container runtime：负责镜像管理以及 Pod 和容器的真正运行（CRI）；
*   kube\-proxy：负责为 Service 提供 cluster 内部的服务发现和负载均衡

![](https://user-gold-cdn.xitu.io/2018/8/29/16584cc39a95345b?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

除了核心组件，还有一些推荐的 Add\-ons：

*   kube\-dns：负责为整个集群提供 DNS 服务
*   Ingress Controller：为服务提供外网入口
*   Heapster：提供资源监控
*   Dashboard：提供 GUI
*   Federation：提供跨可用区的集群
*   Fluentd\-elasticsearch：提供集群日志采集、存储与查询

## 组件详细介绍

### Etcd

Etcd是CoreOS基于Raft开发的分布式key\-value存储，可用于服务发现、共享配置以及一致性保障（如数据库选主、分布式锁等）。

Etcd主要功能：

*   基本的key\-value存储
*   监听机制
*   key的过期及续约机制，用于监控和服务发现
*   原子CAS和CAD，用于分布式锁和leader选举

### kube\-apiserver

kube\-apiserver 是 Kubernetes 最重要的核心组件之一，主要提供以下的功能：

*   提供集群管理的 REST API 接口，包括认证授权、数据校验以及集群状态变更等
*   提供其他模块之间的数据交互和通信的枢纽（其他模块通过 API Server 查询或修改数据，只有 API Server 才直接操作 etcd）

### kube\-controller\-manager

Controller Manager由kube\-controller\-manager和cloud\-controller\-manager组成，是Kubernetes的大脑，它通过apiserver监控整个集群的状态，并确保集群处于预期的工作状态。

kube\-controller\-manager由一系列的控制器组成

*   Replication Controller
*   Node Controller
*   CronJob Controller
*   Daemon Controller
*   Deployment Controller
*   Endpoint Controller
*   Garbage Collector
*   Namespace Controller
*   Job Controller
*   Pod AutoScaler
*   RelicaSet
*   Service Controller
*   ServiceAccount Controller
*   StatefulSet Controller
*   Volume Controller
*   Resource quota Controller

### cloud\-controller\-manager

在Kubernetes启用Cloud Provider的时候才需要，用来配合云服务提供商的控制，也包括一系列的控制器，如：

*   Node Controller
*   Route Controller
*   Service Controller

### kube\-scheduler

kube\-scheduler 负责分配调度 Pod 到集群内的节点上，它监听 kube\-apiserver，查询还未分配 Node 的 Pod，然后根据调度策略为这些 Pod 分配节点（更新 Pod的 NodeName 字段）。

调度器需要充分考虑诸多的因素：

*   公平调度
*   资源高效利用
*   QoS
*   affinity 和 anti\-affinity
*   数据本地化（data locality）
*   内部负载干扰（inter\-workload interference）
*   deadlines

### Kubelet

每个节点上都运行一个 kubelet 服务进程，默认监听 10250 端口，接收并执行 master 发来的指令，管理 Pod 及 Pod 中的容器。每个 kubelet 进程会在 API Server 上注册节点自身信息，定期向 master 节点汇报节点的资源使用情况，并通过 cAdvisor 监控节点和容器的资源。

### Container runtime

容器运行时（Container Runtime）是 Kubernetes 最重要的组件之一，负责真正管理镜像和容器的生命周期。Kubelet 通过 Container Runtime Interface (CRI) 与容器运行时交互，以管理镜像和容器。

### kube\-proxy

每台机器上都运行一个 kube\-proxy 服务，它监听 API server 中 service 和 endpoint 的变化情况，并通过 iptables 等来为服务配置负载均衡（仅支持 TCP 和 UDP）。

kube\-proxy 可以直接运行在物理机上，也可以以 static pod 或者 daemonset 的方式运行。

kube\-proxy 当前支持一下几种实现：

*   userspace：最早的负载均衡方案，它在用户空间监听一个端口，所有服务通过 iptables 转发到这个端口，然后在其内部负载均衡到实际的 Pod。该方式最主要的问题是效率低，有明显的性能瓶颈。
*   iptables：目前推荐的方案，完全以 iptables 规则的方式来实现 service 负载均衡。该方式最主要的问题是在服务多的时候产生太多的 iptables 规则，非增量式更新会引入一定的时延，大规模情况下有明显的性能问题
*   ipvs：为解决 iptables 模式的性能问题，v1.8 新增了 ipvs 模式，采用增量式更新，并可以保证 service 更新期间连接保持不断开
*   winuserspace：同 userspace，但仅工作在 windows 上。





# 实践

[使用 Minikube 创建一个集群](https://kubernetes.io/zh/docs/tutorials/kubernetes-basics/cluster-intro/)