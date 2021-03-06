---
layout: post
title: Nutanix Bible阅读笔记
category: 技术
tags: Nutanix HCI DataCenter Storage

---

<span id = "回到顶部">

> 自从超融合概念兴起，相信很多IT从业人员都有关注超融合的发展趋势。作为超融合产业的先驱，Nutanix自然吸引了大家的眼球。我们先来看这几个问题。

### 超融合是什么？

超融合基础架构（Hyper-Converged Infrastructure，或简称“HCI”）也被称为超融合架构，是指在同一套单元设备（x86服务器）中不仅仅具备计算、网络、存储和服务器虚拟化等资源和技术，而且还包括缓存加速、重复数据删除、在线数据压缩、备份软件、快照技术等元素，而多节点可以通过网络聚合起来，实现模块化的无缝横向扩展（scale-out），形成统一的资源池。[^1]

### Nutanix是什么？

Nutanix是一家设计与销售**超融合基础架构设备**的信息技术公司。他们的产品提供了**软件定义式的信息技术基础架构**，该架构运行虚拟化的应用程序并且提供了许多公共云服务的优点。运算、虚拟、以及存储的资源都被融合至一台可横向扩展的x86服务器（站点），并将之部署在运行Nutanix Acropolis软件的簇之中。Nutanix簇消除了对外部存储数组设备的依赖——譬如存储局域网跟网上附加存储。整个系统的能力可透过增加额外的站点来扩展。另外，Nutanix Prism软件则提供了自动配置、管理及分析的功能。[^2]

### 超融合公司这么多，为什么要关注Nutanix?

- One-click Simplicity
 
    - Reduce complex IT tasks to a single click, and lessen dependence on IT specialists.

- 100% Software

    - Manage one OS across multiple hardware platforms and all IT locations.

- Freedom of Choice

    - Pick the hardware, hypervisor and cloud that is best for your business.

- Lower TCO

    - Reduce IT expenses by as much as 40% to 60%, based on IDC research.

- Fast Time to Value

    - Deploy and manage a complete infrastructure stack in minutes.

- Award-Winning Support

    - You'll love the Nutanix support experience, with a 90+ Net Promoter Score (NPS).

上面几点出自Nutanix官网[^3]，而让我印象更为深刻的一句话出自Nutanix中国区董事总经理陈满恒：

> 在公司创立之初，我们就一直想将软件定义的思路、以AWS为代表的新型的公有云服务模式、苹果公司产品所体现的令人愉悦的体验，全部带入企业级应用市场，而超融合只不过是实现这一愿景的第一步而已。

苹果产品对用户体验的要求几乎到达了工业极致，笔者希望通过对Nutanix Bible的阅读，能够深入理解超融合先驱厂商Nutanix是如何将苹果产品这般令人愉悦的体验带入数据中心/企业级市场。

---

## 数据中心历史回顾

- 计算资源利用率的低效导致向虚拟化转移
- 高级特性，包括 vMotion、HA 和 DRS 等需要集中式存储 
- VM的快速增长不仅增加存储负载，而且增加存储资源争用，带来瓶颈 
- 固态硬盘缓解了磁盘 I/O 问题，但依然不能解决网络和控制器带来的瓶颈
- 跨网络的缓存或内存访问将面临巨大的开销，优势不再明显 
- 阵列配置复杂性依然存在
- 服务器端的缓存可降低阵列的负载和网络影响，但是要引入了新的组件
- 为了克服传统的网络问题，本地化帮助降低瓶颈和负载 
- 将焦点从复杂的基础架构转到管理的易用性和系统堆栈的简化 
- Web Scale 的世界诞生了

---

<span id = "Prism">

## Prism

### 架构

Prism 是一个分布式的资源管理平台，允许用户跨 Nutanix 集群环境管理和监 控对象及服务。

![Prism 的概要架构](http://ws2.sinaimg.cn/large/007ozevdgy1fwpoehljk5j311c0mk432.jpg)

两个组件：

- Prism Central (PC)
- Prism Element (PE)

![Prism Central 和 Prism Element 关系架构](http://ws1.sinaimg.cn/large/007ozevdgy1fwyaok4q5qj30qa0mywha.jpg)

> 基于 HTML5 的用户界面让 Prism 成为易于使用的管理工具。同时，另一项核 心功能是用于自动化的 API。所有 Prism 用户界面提供的功能都可以通过 REST API 编程接口供外部程序调用。这使得用户或是合作伙伴可以实现自动化部署、第 三方工具集成甚至是创建他们自己的用户界面。

---

<span id = "Acropolis">

## Acropolis

> 名词解释：Acropolos – 数据平台 

> 存储，计算和虚拟化平台

### 1) Acropolis架构

- 分布式存储架构 (DSF)
- 应用移动性架构 (AMF)
- 虚拟化管理器（AHV）

![Acropolis 架构纵览](http://ws4.sinaimg.cn/large/007ozevdgy1fwpomv6r4fj310s0l8q77.jpg)

#### 1. 融合平台

[视频介绍](https://youtu.be/OPYA5-V0yRo)

Nutanix 解决方案是一个融合了存储和计算资源于一体的解决方案。它利用本 地组件来为虚拟化构建一个分布式的平台，亦称作虚拟计算平台。该方案是一个软 硬件一体化平台，在 2U 空间中提供 2（6000/7000）或 4(1000/2000/3000/3050 系列)个节点。

![融合平台](http://wx3.sinaimg.cn/large/007ozevdgy1fwpoo93j4lj30u80min1f.jpg)

#### 2. 分布式系统

一组 Nutanix 节点形成了一个分布式系统（Nutanix 集群），负责提供 Prism 和 Acropolis 功能。 所有服务和组件都分布在集群中的所有 CVM 中，以提供 大规模的高可用性和线性性能。

![Nutanix 集群-分布式系统](http://ws4.sinaimg.cn/large/007ozevdgy1fwpopczu0aj31160f0jxf.jpg)

#### 3. 软件定义

Nutanix 平台是一个基于软件的而以软硬 件一体方式交付的解决方案。控制器虚机（CVM）中实现了 Nutanix 软件绝大多 数功能与逻辑，从一开始就被设计成可扩展并支持插件的架构。软件定义而非依赖 于硬件加速的一个关键优势在于可扩展性。与任何产品生命周期一样，将始终引入 改进和新特性。

![软件定义的控制器框架](http://ws3.sinaimg.cn/large/007ozevdgy1fwpoqo67lbj310s0m6jxq.jpg)

#### 4. 集群组件

[视频介绍](https://www.youtube.com/watch?v=3v5RI_IbfV4&feature=youtu.be)

![集群组件](http://ws3.sinaimg.cn/large/007ozevdgy1fwposkrswuj312m0qywn8.jpg)

- Cassandra(Metadata)

    - 关键角色: 分布式元数据存储 

    - 描述：Cassandra 基于修改过的 Apache Cassandra，以分布式环的方式存放和管 理所有的集群元数据。Paxos 算法被用来保证严密的一致性。在集群中所有节点上 都运行着这个服务。Cassandra 通过一个叫做 Medusa 的接口来访问。

- Zookeeper(Configure Store)

    - 关键角色: 集群配置管理 

    - 描述：基于 Apache Zookeeper 实现，Zookeeper 存放了所有的集群配置信息，包 括主机、IP 地址和状态等。集群中有三个节点会运行此服务，其中的一个被选举 成 leader。Leader 接收所有请求并转发到它的组员。一旦 leader 无响应，新的 leader 会被自动选举出来。Zookeeper 通过称作 Zeus 的接口来访问。

- Stargate(IO/Data Manager)

    - 关键角色: 数据 I/O 管理 

    - 描述：Stargate 负责所有的数据管理和 I/O 操作，是 hypervisor 主要的接口（通过 NFS、iSCSI 或 SMB）。该服务在集群中的每个节点上运行，以服务于本地化的 I/O。

- Curator(Orchestrator, 协调器)

    - 关键角色：以 Mapreduce 方式管理和清理集群 

    - 描述：Curator 负责分配和分配整个集群中的任务，诸如磁盘容量平衡、主动清理 等。Curator 运行在所有节点上，受控于主 Curator（其负责任务委托）。Curator 有两种扫描类型，每 6 小时一次全扫描和每 1 小时一次的部分扫描。

- Prism(Interface)

    - 关键角色：用户界面和 API 

    - 描述：Prism 是一个组件管理网关，它让管理员配置和监控 Nutanix 集群。它提供 多种管理手段，如 Ncli、HTML5 UI 和 REST API。Prism 运行在集群中的每个节 点，如同集群中其他组件一样也采用 leader 选举制。

- Genesis

    - 关键角色：集群组件和服务管理 

    - 描述：Genesis 是一个负责任何服务交互（启动/停止等）以及初始配置的进程， 运行在每个节点上。Genesis 是一个独立于群集运行的进程，不需要配置/运行群 集。它唯一的前提是 Zookeeper 必须起来并运行着。Cluster_init 和 cluster_status 这两个页面所显示的信息由 Genesis 进程提供。

- Chronos

    - 关键角色：作业和任务调度 

    - 描述：Chronos 负责从节点扫描和节点间调度/节流任务中获取作业和任务。 Chronos 运行在每个节点上，受控于主 Chronos（负责任务委托且和主 Curator 运 行在同一节点）。

- Cerebro

    - 关键角色：数据复制和容灾管理 

    - 描述：Cerebro 负责 DSF 中的数据复制和容灾管理部分，包含快照的调度、远程 站点的数据复制及站点的迁移和故障切换。Cerebro 运行在 Nutanix 集群的每个节 点上，并且每个节点都参与远程站点/集群的数据复制。

- Pithos

    - 关键角色：vDisk 配置管理 

    - 描述：Pithos 负责 vDisk（DSF 文件）的配置数据。Pithos 构建于 Cassandra 之 上，并运行在每个节点。

#### 5. Acropolis 服务

集群内的每个 CVM 上都会运行一个 Acropolis 服务，其中一个会被选举为首 选 Acropolis，负责任务调度，任务执行，IPAM（IP 地址管理）等，其余的均为从 属 Acropolis，类似于其他具有首选节点的组件，当集群中首选 Acropolis 发生故障 时，就会在余下的 Acropolis 服务中选举出一个新的首选 Acropolis。

首选 Acropolis:

- 任务调度&执行
- 统计信息收集/发布
- 网络控制器（针对 Hypervisor）
- VNC 代理（针对 Hypervisor）
- HA（针对 Hypervisor）

从属 Acropolis:

- 统计信息收集/发布
- VNC 代理（针对 Hypervisor）

![Acropolis 服务](http://ws1.sinaimg.cn/large/007ozevdgy1fwpovw42ksj30yq0l4gpp.jpg)

#### 6. 动态调度

我们真正需要的是消除资源竞争，而不是消除不平衡。除非有资源竞争，否则 “平衡”工作负载不会带来任何好处。事实上，通过强迫不必要的移动，我们会导致 额外的工作(例如，内存传输、缓存重新定位等)，所有这些都需要消耗资源。

Acropolis 动态调度机制是这样的：**它只会在预期发生资源竞争的情况下调用 工作负载移动，而不是因为不平衡。** 注:Acropolis DSF 以不同的方式工作，以确保 在整个集群中均匀分布数据以消除热点和加速重建。要了解更多 DSF，请查看“磁 盘平衡”部分。

资源调度由两个关键过程组成:

- 初始位置放置 
> 虚拟机在哪台服务器节点上开机 

- 运行时优化
> 基于实时运行指标的工作负载移动

![Acropolis 动态调度图](http://ws2.sinaimg.cn/large/007ozevdgy1fwpowlfe0oj30y60r4dm7.jpg)

> 使用历史利用值和平滑算法预估需求。 这个预估的需求是用来确定移动的，这保证了突然的峰值不会影响决策。

> Acropolis 动态调度机制是这样的：它只会在预期发生资源竞争的情况下调用 工作负载移动，而不是因为不平衡。

放置位置取决于下面的几个因素：

- 计算资源消耗
- 存储性能
- (反)关联规则

> 在迁移之后，系统将判断迁移的“有效性”，并了解实际的好处是什么。通过这 种智能的学习模式可以自我优化，以确保任何迁移决策都有一个有效的依据。

#### 7. 安全与加密

- 安全

Nutanix 超融合架构具有以下安全认证

1. 通用准则（Common Criteria）
2. 安全技术实现指南（Security Technical Implementation Guides(STIGs)）
3. 美国联邦信息处理标准 FIPS 140-2
4. 贸易协定法案 TAA Compliance
5. 自动安全配置管理 Security Configuration Management Automation (SCMA)

- 数据加密

Nutanix 提供的数据静态加密功能， Nutanix 的自加密磁盘（SED）利用了 FIPS-140-2 Level-2 算法，与外部的秘钥管理服务器（KMS）配合使用。与 KMS 系 统通讯使用的是标准的协议，例如 KMIP 和 TCG。 与之配合的 KMS 服务器系统包 括 Vormetric, SafeNet 等等

![加密](http://ws3.sinaimg.cn/large/007ozevdgy1fwpozqkxfoj312i0rw0ym.jpg)

> SED 的数据加密方式是将存储设备划分为能设置为加密状态和非加密状态的 “数据条带”。在 Nutanix 的系统里， boot 和 Nutanix 的 Home 分区是被加密处 理过的。所有的数据磁盘和数据条带是用 big key 到 level-2 标准进行高度加密处 理的。

> 当集群启动的时候，它会想 KMS 服务器发出请求解锁磁盘的秘钥。为了确保 在集群里从来也不缓存任何秘钥。在系统冷启动和 IPMI 重置的过程中，这个节点 将需要向 KMS 服务器发出获取解锁磁盘的请求。CVM 软启动的过程不会强制发 生以上操作。

#### 8. 磁盘驱动器解构

- Nutanix 主目录（CVM 核心） 
- Cassandra （元数据 存储） 
- OpLog （持久写缓冲） 
- 内容缓存 （SSD 缓存） 
- 扩展存储 （持久存储）

下图展示了一个 Nutanix 节点 SSD 的存储分解例子：

![SSD驱动分解](http://ws3.sinaimg.cn/large/007ozevdgy1fwpp0dblckj30x00i0adz.jpg)

由于 HDD 设备主要作用于大容量存储，分解起来非常简单：

- Curator 保留（Curator 存储）
- 扩展存储（持久存储）

![HDD驱动分解](http://ws1.sinaimg.cn/large/007ozevdgy1fwpp1estwgj30yq0iamzq.jpg)

---

### 2) Acropolis分布式存储结构

> 分布式存储 DSF 像任何集中存储阵列一样呈现给 Hypervisor，然而所有的 I/O 是在本地处理以提供更高的性能。

![分布式存储框架纵览](http://ws3.sinaimg.cn/large/007ozevdgy1fwpp32qhh9j315w0j244p.jpg)

#### 1. 数据结构

- 存储池

> 角色：一组物理存储设备

描述：一个存储池是一组物理存储设备，包括集群内所有节点的 PCIe SSD、 SSD 和 HDD。存储池可以跨多个 Nutanix 节点并且随着集群的扩展而扩展。在大 部分情况下，建议单个集群配置一个存储池。

- 容器

> 角色：一组虚拟机或者文件的逻辑分组

描述：容器（container）从逻辑上划分存储池，并包含一组虚拟机或者文件 （即虚拟磁盘）。一些配置项（例如 RF）是在容器层实现的，然后应用在单个虚 拟机文件层面。Container 和 Datastore 是一一对应的（在 NFS、SMB 场景中）。

- vDisk

> 角色：vDisk 

描述：一个虚拟磁盘文件(vDisk)是 DSF 上任意一个大于 512KB 的文件（包 括 VMDK 和虚拟机硬盘）。vDisk 由 extent 组成，它被分组并保存在磁盘上作为 extend group。

**最大 DSF vDisk 容量**

![文件系统分解纵览](http://ws2.sinaimg.cn/large/007ozevdgy1fwpp3tac1nj31000haad0.jpg)

- Extend

> 角色：逻辑上连续的数据块 

描述：一个 Extent 是逻辑上连续的 1MB 大小的数据块，它由 n 个连续的 block 组成（block 是可变的，取决于不同操作系统）。Extent 的读写修改是基于 更小的子块（也称为 slice）以保证可粒度和有效性。当一个 extent 的 slice 被读入 cache 时，可以被修剪（trimmed），这依赖于被读取的数据量大小。

- Extend 组

> 角色：物理上连续的数据块 

描述：一个 Extent Group 是物理上连续存储的 1MB 或者 4MB 大小的数据块。 它以文件的形式由 CVM 管理并保存在磁盘上。Extent 被动态分布在多个 Extent 组中，提供数据跨节点和磁盘的条带功能用来提高 IO 性能。注：在 NOS4.0 中， Extent 组可以是 1MB 或者 4MB，这取决于消重功能。

下图说明这些不同结构分类和各个文件系统之间的对应关系:

![文件系统分解细节](http://wx4.sinaimg.cn/large/007ozevdgy1fwpp4mgch9j30yu0ruq75.jpg)

这是另外一个表明文件系统各部分关系的示意图：

![图形文件系统分解](http://wx1.sinaimg.cn/large/007ozevdgy1fwpp5a65haj30z60i0juj.jpg)

#### 2. I/O 路径和缓存

[视频介绍](https://youtu.be/SULqVPVXefY)

![DSF I/O 路径](http://ws1.sinaimg.cn/large/007ozevdgy1fwpp6tygc1j30zc0vawmo.jpg)

- Oplog

> 角色：持久性写缓存

- Extent Store

> 角色：持久性数据存储

- Unified Cache

> 角色：动态读缓存

![DSF 统一缓存](http://ws4.sinaimg.cn/large/007ozevdgy1fwpp7fszk9j30yi0kwjwt.jpg)

#### 3. 可扩展的元数据

[视频介绍](https://youtu.be/MlQczJhQI3U)

- 必须 100%时间里都是正确的（称为“强一致性”）
- 必须符合 ACID 策略 
- 必须具有无限扩展性 
- 必须没有任何扩展性上的瓶颈（必须是现行扩展）

![Cassandra 环形结构](http://ws4.sinaimg.cn/large/007ozevdgy1fwpp83lg4bj30zu0nu0xn.jpg)

![Cassandra 横向扩展](http://wx3.sinaimg.cn/large/007ozevdgy1fwpp8ogobjj30zi0g8q8n.jpg)

#### 4. 数据保护

[视频介绍](https://youtu.be/OWhdo81yTpk)

Nutanix 平台使用复制因子（RF - Replication Factor/Resillience Factor）和 校验和（Checksum）来保证当节点或者磁盘失效时，数据的冗余度和可用性。按 照上面的解释，Oplog 作为暂存区来接受所有写操作，并保存到低延迟的 SSD 层 上。当数据写入本地 Oplog 时，数据被“同步”复制到另 1 个或者 2 个 Nutanix CVM 的 Oplog 之中（取决于 RF 设置），当这个操作完成之后，此次写操作才被确认 （Ack）。这样能确保数据至少存在于 2 个或者 3 个独立的节点上，保证数据的冗 余度。注：当 RF 为 3 时，至少需要 5 个节点，并且元数据（metadata）数据会 保留 5 份（RF5）。

![DSF 数据保护](http://wx4.sinaimg.cn/large/007ozevdgy1fwppfwck27j30yg0k80ys.jpg)

#### 5. 可用域（机箱感知）

[视频介绍](https://youtu.be/LDaNY9AJDn8)

> 可用域（也称为节点/模块/机架感知）是分布式系统的关键结构，以遵守组件 和数据放置的决定。DSF 现在是节点和机箱感知的，然而，随着集群规模的增长， 需要提升到机架感知。Nutanix 说的模块(block)是指一个机箱，包含一个，两个或 者四个服务器节点。注：如果需要实现机箱感知，最少需要 3 个模块，否则缺省 是节点感知。

下图展示了在一个 3 模块的部署中副本如何放置的例子：

![机箱感知的副本放置](http://wx1.sinaimg.cn/large/007ozevdgy1fwppgj1quwj30z00tgn3z.jpg)

当模块故障时，机箱感知将被维持，副本将重新复制到其他模块中：

![区块故障情况下的副本放置](http://wx2.sinaimg.cn/large/007ozevdgy1fwpph06z77j30xq0ze46m.jpg)

Cassandra 对等复制在整个环里用顺时钟的方式迭代到各节点。在机箱感知 下，对等（peer）会分布到模块里，确保没有两个对等会在同一个模块里。

下图显示了一个节点布局按照上述环形转换到模块布局的例子：

![Cassandra 节点块感知放置](http://ws1.sinaimg.cn/large/007ozevdgy1fwsmk8j9obj30yi0rmwlv.jpg)

**元数据感知条件**

FT1（数据 RF2/元数据 RF3）将会是机箱感知，如果：

- \>3 模块
- X 代表模块最大节点数，剩余模块的节点数最少有 2X 个

> 例 1：4 个模块分别有 2，3，4，2 个节点，不会机箱感知，因为 2+3+2<2*4

> 例 2：4 个模块分别有 3，3，4，3 个节点，机箱感知，因为 3+3+3>2*4

FT2（数据 RF3/元数据 RF5）将会机箱感知

- \>5 模块
- X 代表模块最大节点数，剩余模块的节点数最少有 4X 个

> 例 1:6 个模块分别有 2,3,4,2,3,3 个节点，不会机箱感知，因为 2+3+2+3+3<4*4

> 例 2:6 个模块分别有 2,4,4,4,4,4 个基点，机箱感知，因为 2+4+4+4+4>4*4

#### 6. 数据路径冗余

![通常状态集群数据摆放](http://ws1.sinaimg.cn/large/007ozevdgy1fwppjkq61dj311g0emgqo.jpg)

> 关键点：使用 Nutanix，确保数据的一致性分布可以保证一致的写性能和极其 优秀的重新保护时间。这也会应用到任何集群范围的活动（例如纠删码，压缩，重 删等）。

潜在故障的级别

- 磁盘故障

![磁盘故障和重新保护](http://wx1.sinaimg.cn/large/007ozevdgy1fwppk8jrucj314c0e4jw9.jpg)

- 节点故障

![节点故障](http://ws3.sinaimg.cn/large/007ozevdgy1fwppl6mo2mj31120fo43h.jpg)

#### 7. 容量优化

![优化技术](http://ws1.sinaimg.cn/large/007ozevdgy1fwppmow8ejj30zk0w6do2.jpg)

- 纠删码

> 和 RAID（级别 4，5，6 等）的概念相似，校验位是计算出来的。EC 对不同 节点上的数据块条带进行编码和计算校验位。当主机或者磁盘故障时，校验位可以 被利用为计算任何缺失的数据块（解码）。

> 专家提示：强烈建议集群里节点的数量要比条带大小组合（数据块数量 +校验块数量）至少加一，以便当节点故障时可以重建条带。这样避免了条 带重建时（由 Curator 自动进行）产生的计算过载。例如，一个 4/1 条带应该 在集群里至少有 6 个节点。上面的表格是按照最佳实践得出来的。

> 专家提示：纠删码和在线压缩可以完美地结合使用，大大节省存储空

间。在我们的环境里，总是利用压缩+EC 配置。

- 压缩

[视频介绍](https://youtu.be/ERDqOCzDcQY)

> Nutanix 容量优化引擎（COE）负责数据的转换来增加磁盘中数据的效率。目 前压缩是 COE 实现数据优化的一项关键特性。 DSF 提供在线和离线两种压缩方式 以满足客户的需求和不同数据类型。

> OpLog 压缩 截至 5.0 版本，Oplog 现在压缩所有进入的大于 4K 的写，并表现出良好的压缩效果（Gflag: vdisk_distributed_oplog_enable_compression）。这样会更有效的利用 Oplog 空间并提升实际 性能。 当数据从 Oplog 落入 Extent Store 会被解压缩、对齐并被按照 32K 对齐的尺寸重新压缩。 （截至 5.1 版本） 本特性缺省开启，不需要用户配置。

> 离线压缩开始按照正常方式写数据（以不压缩状态），然后借助 Curator 框架在集群范围 压缩数据。当在线压缩开启但是 I/O 都是随机数据，Oplog 中的数据会被解压缩，归并， 然后在内存中压缩后写入 Extent Store。

> 正常数据：三天没有读写访问（Gflag: curator_medium_compress_mutable_data_delay_secs） 不可更改数据（快照）：一天没有访问（Gflag: curator_medium_compress_immutable_data_delay_secs）

在线压缩 I/O 路径:

![在线压缩 I/O 路径](http://ws1.sinaimg.cn/large/007ozevdgy1fwsn90yvzoj30z80rsq8s.jpg)

离线压缩 I/O 路径:

![image](http://wx1.sinaimg.cn/large/007ozevdgy1fwsn9mwefqj30xe0vowls.jpg)

> 专家提示：建议总是使用在线压缩（压缩延迟=0），它只压缩大的/顺序写，不影响随机写 的性能。 这也将增加 SSD 层的可用容量，增加有效性能并允许更多数据放置在 SSD 层。而且，对于 写入并在线压缩的大的或者顺序数据，用于 RF 的复制也会传输压缩过的数据，由于在网络 上发送更少的数据而进一步增强性能。 在线压缩也完美地和纠删码配合使用。

解压缩 I/O 路径:

![解压缩 I/O 路径](http://wx2.sinaimg.cn/large/007ozevdgy1fwsnar8mbaj30yy0wu7bs.jpg)

- 弹性消重引擎

[视频介绍](https://youtu.be/C-rp13cDpNw)

> 弹性消重引擎是 DSF 中一个基于软件的特性，它允许在容量层面（HDD）和 性能层面（SSD/内存）对数据进行消重操作。 顺序的数据流在写入（ingest）时， 按照 16KB 粒度进行 SHA-1 的散列（hash）运算标记指纹。指纹标记操作仅在数 据块被写入（ingest）的时候进行，并作为该数据块的元数据信息的一部分被持久 保存。注意：最初采用 4KB 粒度进行指纹标记，然而经过测试， 16KB 粒度的指 纹标记操作能在消重能力和减少元数据(metadata)开销之间取得最好的平衡。当已 消重数据进入 unified cache 之后，将以 4KB 粒度进行消重。

> 为了使元数据更加高效率，指纹标记参考计数被监控来跟踪可消重能力。参考 计数低的指纹计数将被丢弃以减少元数据的过载。为最小化碎片，完全 extents 是 用于在容量层面的消重首选.

弹性消重引擎 I/O 路径:

![弹性消重引擎 I/O 路径](http://ws2.sinaimg.cn/large/007ozevdgy1fwsndkoc88j30yi0x4jyj.jpg)

> 专家提示：消重+压缩 在 4.5 版本，消重和压缩都可以在一个容器里启用，但是，除非数据是可 以被消重的（条件在之前的章节里解析过），坚持使用压缩。

#### 8. 存储分层和优先级

> 简单而言，磁盘的分层是实现在集群内 所有节点的 SSD 和 HDD 上，并且由 ILM 负责触发数据迁移。本地节点的 SSD 层 总是最高优先级的，负责所有本地虚拟机 I/O 的读写操作。并且还可以使用集群内 所有其他节点的 SSD，因为 SSD 层总是能提供最好的读写性能，并且在混合阵列 中尤为重要。

![DSF 层级优先顺序](http://ws1.sinaimg.cn/large/007ozevdgy1fwpppnstsyj30z80kiq7o.jpg)

> 特定类型的存储资源（eg. SSD, HDD,等）在集群范围内被池化统一管理，形成集群范围的存储分层。这意味着集群内任何节点都可以使用到整个存储分层，无 论这一存储资源在本地还是在其他节点上。

![DSF 集群范围内的分层](http://ws1.sinaimg.cn/large/007ozevdgy1fwpps0x7cjj30zk0fcwkk.jpg)

> 一个经常被问到的问题就是：如果节点本地的 SSD 被用满之后，性能是否会 下降？之前我们提到使用磁盘平衡功能会保证集群范围内所有磁盘的利用率基本一 致。那么当本地 SSD 利用率非常高时，磁盘平衡功能会自动迁移 SSD 中最冷的 数据到集群内另一个节点的 SSD 上。这将释放本地 SSD 的空间，数据会继续写 到本地 SSD 上，而不会因为通过网络写到另一个节点的 SSD 上而导致性能下降。 一个关键点需要提及的是所有 CVM 和 SSD 都参与这种远程 IO 操作，可以消除潜 在的瓶颈，并且通过执行远程 IO 操作可以自愈一些命中率的问题。

![DSF 集群内的分层均衡](http://wx3.sinaimg.cn/large/007ozevdgy1fwppsfpezvj30zk0fy45w.jpg)

> 迁移数据是使用最后访问时间来判断。例如当 SSD 利用率到 95%，那么将有 20% 的数据会被移动到 HDD（95% -- 75%），如果 SSD 利用率时 80%，则只有 15%的数据会被移动到 HDD。

![DSF 层级 ILM](http://wx2.sinaimg.cn/large/007ozevdgy1fwppt0k7t6j30z80fa0z8.jpg)

#### 9. 磁盘平衡

[视频介绍](https://youtu.be/atbkgrmpVNo)

![磁盘均衡 - 非平衡状态](http://wx3.sinaimg.cn/large/007ozevdgy1fwppuyos9gj30z80gg44k.jpg)

> 磁盘平衡功能使用 DSF 的 Curator 框架作为调度进程，当磁盘阈值被触发时， 在后台自动运行（例如，当本地节点利用率超过 n%后自动运行）。当数据不平衡 时，Curator 会决定哪些数据移动到哪里，并自动生成 MapReduce 任务分发到各 节点执行。

![磁盘均衡 - 平衡状态](http://wx2.sinaimg.cn/large/007ozevdgy1fwppvc5y90j30zg0i444g.jpg)

> 在一些情形下，客户可能希望将一个节点只作为存储使用，则这个节点上将没 有虚拟机运行，它的主要功能作为大容量存储设备使用。在这种情况下，节点上所 有内存都可以给到 CVM 使用，来提供更大的读缓存。

![磁盘均衡 - 单纯的存储节点](http://ws4.sinaimg.cn/large/007ozevdgy1fwppvqot9ij30zu0fydlb.jpg)

#### 10. 快照和克隆

[视频介绍](https://youtu.be/uK5wWR44UYE)

下图显示了快照是如何进行工作的例子：

![块图快照实例](http://wx1.sinaimg.cn/large/007ozevdgy1fwsntvqxwzj30z40fin15.jpg)

当一个 vDisk 的快照和克隆继续进行快照和克隆时，采用相同的方法：

![块图和新写入的多重快照](http://wx3.sinaimg.cn/large/007ozevdgy1fwsnuoq3nlj30ya0gegqp.jpg)

> 采用相同的方法进行虚机和 vDisk 的快照和克隆。当一个虚机或 vDisk 进行克 隆时，当前的块映射被锁定，克隆被创建出来。这些更新仅仅是元数据，没有 I/O 实际发生。相同的方法应用到克隆的克隆。之前克隆的虚机充当基础 vDisk，数据 块映射被锁定，2 个克隆被创建：一个给已经克隆的虚机，另外一个给新的克隆。

![块图的多重克隆](http://wx2.sinaimg.cn/large/007ozevdgy1fwsnvy5ivzj30ug0l8tdi.jpg)

> 如之前说过，每个虚机/vDisk 有它自己的单独的数据块映射。所以在上述例子 里，所有从基础虚机来的克隆将会有它们自己的数据块映射，所有写和更新将会发 生在那里。

![块图克隆 - 新写入](http://ws4.sinaimg.cn/large/007ozevdgy1fwsnwrggj7j30xa0n6n2j.jpg)

> 任何后续虚机/vDisk 克隆或快照将会引起原始数据块映射被锁定，并将产生一 个新的读写访问。

#### 11. 网络及 I/O

> Nutanix 平台的内部节点间无背板链接，而是依赖于标准的 10GbE 网络进 行通讯。运行于 Nutanix 平台上的虚机的所有存储 I/O 都在专用私有网络里由虚拟 化监控程序（HyperVisor）处理。虚拟化层接收 I/O 请求，随后将其转发给本地 CVM 的私网 IP。此 CVM 随后将通过外部 IP 及外部的 10GbE 网络把数据远程复 制至其他 CVM 中。对于读请求，绝大部分都将由本地 CVM 提供服务，而无需通 过外部的 10GbE 网络。这就意味着，只有 DFS 的远程复制和 VM 的网络流量需 要用到 10GbE 网络，除非本地 CVM 宕机或跨节点访问远程数据（如 vMotion 后）。当然，集群的其他一些情况也会临时使用到 10GbE 网络，例如：磁盘的负 载均衡等。

以下是 VM 使用到的 I/O 路径的示意图，包含内部、外部 10GbE 网络：

![DSF 网络](http://wx1.sinaimg.cn/large/007ozevdgy1fwsnybjv0lj310u0e2qaw.jpg)

#### 12. 数据本地化 Data Locality

> I/O 和数据的本地化（data locality），是 Nutanix 超融合平台强劲性能的关 键所在。正如之前 I/O 路径（I/O Path）章节所述，所有的读、写 I／O 请求都藉由 VM 的所在节点的本地 CVM 所响应处理。VM 的数据都将由本地的 CVM 及其所管 理的本地磁盘提供服务。当 VM 由一个节点迁移至另一个节点时（或者发生 HA 切 换），此 VM 的数据又将由现在所在节点中的本地 CVM 提供服务。当读取旧的数 据（存储在之前节点的 CVM 中）时，I／O 请求将通过本地 CVM 转发至远端 CVM。所有的写 I／O 都将在本地 CVM 中完成。DFS 检测到 I/O 请求落在其他节 点时，将在后台自动将数据移动到本地节点中，从而让所有的读 I/O 由本地提供服 务。数据仅在被读取到才进行搬迁，进而避免过大的网络压力。

以下展示了数据是如何随着 VM 而迁移的：

![数据本地化](http://ws3.sinaimg.cn/large/007ozevdgy1fwsnzrzpk5j310q0dmdll.jpg)

数据迁移条件：

> Data Locality 是一个实时的操作过程，当满足以下条件时将搬迁此数据 （An Extent Group）：10 分钟内被触发 3 次随机读或者 10 次顺序读，其中 10 秒内的多次读取被视为 1 次触发.

#### 13. 影子克隆 Shadow Clones

[视频介绍](https://www.youtube.com/watch?v=0Q19RdbaLkY)

> Acropolis DSF 有一项功能称之为“Shadow Clones”，允许在大量并发读取 时分布式的缓存特定的 vDisk 或 VM 数据。最好的例子莫过于在 VDI 部署中，大 量的链接克隆（Linked Clones）需要读取模版虚机（A central master or ‘Base VM’）。

> 注意：数据将在读取后执行搬迁，因此不会大量增加网络负载，并且可以大幅 提升缓存的利用率。

![影子克隆](http://ws4.sinaimg.cn/large/007ozevdgy1fwso1l86htj311c0h2ti5.jpg)

#### 14. 存储层和监控

Nutanix 平台监控着存储中的多个层面，从 VM 或 Guest OS 到物理磁盘。下图显示各个层面相关的监控事项及监控的颗粒度等：

![存储层级](http://wx3.sinaimg.cn/large/007ozevdgy1fwso2idnldj30zw0b2tcx.jpg)

### 3) Acropolis服务

- Nutanix Guest Tools (NGT)
- OS 定制化
- 块服务
- 文件服务
- 容器服务

### 4) Acropolis备份与容灾

- 实施组件
- 保护对象
- 备份和恢复
- 应用一致性快照
- 复制和容灾（DR）
- 近线同步复制技术（NearSync）
- 城域高可靠 Metro Availability
- Cloud Connect

---

<span id = "AHV">

## AHV

> Acropolis Hypervisor Virtualization

### 1) AHV架构

#### 1. 节点架构

![节点架构](http://ws2.sinaimg.cn/large/007ozevdgy1fwrpa7xpbuj30w00oc43j.jpg)

#### 2. KVM 架构

> Libvirt是用于管理虚拟化平台的开源的API，后台程序和管理工具。

> 旨在提供一套统一的接口管理虚拟化

![KVM架构](http://ws4.sinaimg.cn/large/007ozevdgy1fwrphg9h8xj30vy0kun05.jpg)

处理器兼容性

类似于 VMware 的增强 vMotion 兼容性(EVC) 功能，允许虚拟机在不同代处 理器之间进行移动；__AHV 将自动检测集群中最低的处理器的，并限制所有的 QEMU 域运行在这个级别中。__ 通过此功能允许包含跨代处理器的 AHV 集群中，提 供虚拟机在主机间在线迁移的能力。

#### 3. 最高配置和可扩展性

*AHV 不像 ESXi / Hyper-V 这样传统的存储堆栈; 在 AHV 中，所有的磁盘以 __裸 SCSI 块设备方式直通到虚拟机中。__ 这意味着最大的虚拟磁盘容量仅受限于 DSF 的虚拟磁盘容量(最大 9 EB)。

#### 4. 网络

![网络](http://wx2.sinaimg.cn/large/007ozevdgy1fwrpqxakgnj310a0rkgrl.jpg)

### 2) AHV如何工作

#### 1. 存储 I/O 路径

*AHV 不像 ESXi / Hyper-V 这样传统的存储堆栈; 在 AHV 中，__所有的磁盘以 裸 SCSI 块设备方式直通到虚拟机中。__ 这样能够保持最佳和最轻量的 I/O 路径。

- iSCSI 多路径-正常状态

> 在有登录请求时，iSCSI Redirector 将把 iSCSI 登录请求重定向到一个健康的 Stargate （默认本地优先）。

![iSCSI 多路径-正常状态](http://ws3.sinaimg.cn/large/007ozevdgy1fwrq3alvizj30x00o243m.jpg)

- iSCSI 多路径 – 本地 CVM 下线

> 当一个活动的 Stargate 下线后(通过 NOP OUT 命令获取状态), iSCSI Redirector 将本地的 Stargate 标记为不健康状态。当 QEMU 试图发起一个 iSCSI 请求时，Redirector 将重定向此登录请求到一个健康的 Stargate 之上。

![iSCSI 多路径 – 本地 CVM 下线](http://wx1.sinaimg.cn/large/007ozevdgy1fwrq6d2cgkj30y00pgdlc.jpg)

- iSCSI 多路径 – 本地 CVM 在线上线

> 一旦本地 CVM 重新上线（通过 NOP OUT 命令获得状态），远程的 Stargate 将结束所有的远程 iSCSI 会话。QEMU 将再次发送一个新的 iSCSI 登录请求并重 定向到本地的 Stargate。

![iSCSI 多路径 – 本地 CVM 在线上线](http://wx4.sinaimg.cn/large/007ozevdgy1fwrq4bu9y6j30yw0o6aft.jpg)

#### 2. 微分段

- 类别选项
- 安全规则
- 应用规则
- 隔离规则

![微分段](http://wx4.sinaimg.cn/large/007ozevdgy1fwrqftbsidj311u0ps0yn.jpg)

执行方式

![执行方式](http://ws1.sinaimg.cn/large/007ozevdgy1fwrqgtz2omj31320kcjvw.jpg)

#### 3. Service Chaining服务链

> 作为网络路径的一部分， AHV 服务链功能允许截取所有流量，并转发给数据包处理器（NFV，设备，虚拟设备等）用于后续处理。

- 防火墙（如 Palo Alto 等） 
- 负载均衡器（例如 F5，NetScaler等）
- IDS / IPS /网络监视器
- 实时包处理器
- 触发式包处理器

任何服务链都是 __在应用微分段规则之后，在数据包离开本地 OVS 之前完成的：__

![image](http://wx3.sinaimg.cn/large/007ozevdgy1fwrqjetov8j311u0kan1m.jpg)

同时也支持在一个服务链中将多种数据包处理器结合使用：

![image](http://wx2.sinaimg.cn/large/007ozevdgy1fwrqk14z7uj314e0dk41g.jpg)

#### 4. IP 地址管理

> Acropolis IP 地址管理(IPAM) 解决方案提供了 DHCP 段以及分配 IP 地址到虚 拟机的能力。IPAM 利用了 VXLAN 和 OpenFlow 规则去中断和响应 DHCP 请求。

- IPAM – 本地 Acropolis Master

![image](http://wx3.sinaimg.cn/large/007ozevdgy1fwrqml6btnj30u00q2wiz.jpg)

- IPAM – 远程 Acropolis Master

![image](http://ws4.sinaimg.cn/large/007ozevdgy1fwrqnd962vj314k0mctdz.jpg)

#### 5. VM 高可用性 (HA)

> AHV 虚拟机高可用性（HA）是一个用于在主机或机架在出现故障时确保虚拟 机高可用性的功能。当主机出现故障时，虚拟机将自动在集群中一个健康的节点上 重新启动。Acropolis Master 负责重新启动虚拟机到健康的节点之上。

HA – 主机监控

![image](http://wx1.sinaimg.cn/large/007ozevdgy1fwrqnvkuf1j30wq0hw0wl.jpg)

- 预留主机

下图显示了一个预留主机的示例场景：

![HA – 预留主机](http://wx2.sinaimg.cn/large/007ozevdgy1fwrqpbv2qcj30pc0fo406.jpg)

在这种情况下，所有故障节点上的虚拟机将在预留主机上重新启动：

![HA – 预留主机 – 失效转移](http://wx2.sinaimg.cn/large/007ozevdgy1fwrqpbv2qcj30pc0fo406.jpg)

如果故障的主机重新上线后，之前运行在故障节点上的虚拟机将通过在线迁移 技术重新迁移到原主机上，以最少化因为数据本地化所带来的数据移动。

![HA – 预留主机 – 失效恢复](http://ws4.sinaimg.cn/large/007ozevdgy1fwrqq4ksx1j30og0goac2.jpg)

- 预留分段

> 预留分段分布所有预留的资源到整个集群中的所有主机。在这个场景中，每一 个主机将分担 HA 的一部分预留，以确保在主机出现故障时，整个集群仍然有充足 的失效转移能力去重新启动虚拟机。

下图显示了一个预留分段的示例场景：

![HA –预留分段](http://wx2.sinaimg.cn/large/007ozevdgy1fwrqs4o2ilj30qq0dm0ub.jpg)

当一个主机失效时，其上运行的所有虚拟机将在集群中剩余的健康主机上重新 启动。

![HA – 预留分段– 故障转移](http://ws1.sinaimg.cn/large/007ozevdgy1fwrqsh5oxdj30pq0gqac3.jpg)

> 预留分段= (最高负载的主机/ 分段容量) x (1 + 碎片开销)

### 3) AHV管理

- 在 OVS 上仅启动 10GbE 链路

```
// 描述: 在本机的 bond0 上将启用 10g 链路
manage_ovs --interfaces 10g update_uplinks
//描述: 在整个集群上仅启用 10g 链路 
allssh "manage_ovs --interfaces 10g update_uplinks"

```
- 显示 OVS 上联链路

```
//描述: 在本机上显示 ovs 上联链路情况
manage_ovs show_uplinks 
//描述: 在整个集群上显示 ovs 上联链路情况
allssh "manage_ovs show_uplinks"

```

- 显示 OVS 接口

```

//描述: 显示本机的 ovs 接口 
manage_ovs show_interfaces 
//描述: 显示整个集群的 ovs 接口 
allssh "manage_ovs show_interfaces"

```

- 显示 OVS 交换机信息

```

//描述: 显示交换机信息 
ovs-vsctl show

```

- 列出 OVS 桥

```

//描述:列出桥 
ovs-vsctl list br

```

- 显示 OVS 端口信息

```

//描述: 显示 OVS 端口信息 
ovs-vsctl list port br0 
ovs-vsctl list port <bond>

```

- 显示 OVS 接口信息

```

//描述: 显示接口信息 
ovs-vsctl list interface br0

```

- 在桥上显示端口/接口

```

//描述: 显示上的端口 
ovs-vsctl list-ports br0 
//描述: 显示桥上的 
iface ovs-vsctl list-ifaces br0

````

- 创建 OVS 桥

```

//描述: 创建桥 
ovs-vsctl add-br <bridge>

```

- 为桥添加端口

```

//描述: 为桥添加端口 
ovs-vsctl add-port <bridge> <port> 
//描述: 为桥添加 bond 端口 
ovs-vsctl add-bond <bridge> <port> <iface>

```

- 显示 OVS bond 信息

```

//描述: 显示 bond 信息
ovs-appctl bond/show <bond>
//示例:
ovs-appctl bond/show bond0

```

- 设置 bond 模式并配置 LACP

```

//描述: 在端口上启用 LACP 
ovs-vsctl set port <bond> lacp=<active/passive> 
//描述:启用所有主机上的 bond0 
for i in `hostips`;do echo $i; ssh $i source /etc/profile > /dev/null 2>&1; ovs-vsctl set port bond0 lacp=active;done

```

- 显示 bond 上的 LACP 信息

```

//描述: 显示 LACP 细节 
ovs-appctl lacp/show <bond>

```

- 设置 bond 模式

```

//描述: 设置端口的 bond 模式 
ovs-vsctl set port <bond> bond_mode=<active-backup, balance-slb, balance-tcp>

```

- 显示 OpenFlow 信息

```

//描述: 显示 OVS openflow 细节 
ovs-ofctl show br0 
//描述: Show OpenFlow rules 
ovs-ofctl dump-flows br0

```

- 得到 QEMU 进程 ID 和 top 信息

```

//描述: 得到 QEMU 的进程 ID 信息 
ps aux | grep qemu | awk '{print $2}' 
//描述: 得到指定进程 ID 的性能计量信息 
top -p <PID>

```

- 得到所有 QEMU 进程的活动 Stargate

```

//描述: 为所有 QEMU 进程得到活动的 
Stargate 信息 netstat –np | egrep tcp.*qemu

```

- 检查 iSCSI Redirector 日志

```

//描述: 检查所有主机的 iSCSI Redirector 日志
for i in `hostips`; do echo $i; ssh root@$i cat /var/log/iscsi_redirector;done
//单个主机示例
Ssh root@<HOST IP> 
Cat /var/log/iscsi_redirector

```

- 监控 CPU 闲置(闲置 CPU)

```

//描述: 监控 CPU 闲置情况
//执行 top 并查看 %st (如下)
Cpu(s):0.0%us, 0.0%sy, 0.0%ni, 96.4%id, 0.0%wa, 0.0%hi, 0.1%si, 0.0%st

```

- 监控 VM 网络资源状态

```

//描述: 监控虚拟机资源状态
//执行 virt-top
virt-top

```

> 查看网络页，可以按数字 2 进入网络页面。

#### 故障排除和高级管理

```

//Description: Check iSCSI Redirector Logs for all hosts
for i in `hostips`; do echo $i; ssh root@$i cat /var/log/iscsi_redirector;done

```

```

//Example for single host
Ssh root@<HOST IP> 
Cat /var/log/iscsi_redirector

```

> Monitor CPU steal (stolen CPU)

```

//Description: Monitor CPU steal time (stolen CPU)
//Launch top and look for %st (bold below)
Cpu(s): 0.0%us, 0.0%sy, 96.4%id, 0.0%wa, 0.0%hi, 0.0%ni, 0.1%si, 0.0%st

```

> Monitor VM network resource stats

```

//Launch virt-top
virt-top

```

> Go to networking page 2 – Networking

---

<span id = "vSphere">

## vSphere

### 1) vSphere架构

#### 1. 节点架构

![ESXi 的节点架构](http://wx3.sinaimg.cn/large/007ozevdgy1fwsd5axnzej31020rmjxq.jpg)

#### 2. 最高配置和可扩展性

- 最大群集大小 ： 64 
- 每个虚拟机的最大虚拟 CPU ： 128
- 每个虚拟机最大内存 ： 4TB 
- 每台主机最大的虚拟机 ： 1024
- 每个集群最大的虚拟机 ： 8000 （如果启用 HA 2048 每个数据存储）

对比AHV的KVM架构

- 群集最大数量（Maximum cluster size）: N/A – 跟 Nutanix 群集一样
- 每个 VM 的最大虚拟 CPU（Maximum vCPUs per VM）: 每台主机的最大
- 每个虚拟磁盘的容量: 9EB* (Exabyte)
- 每个 VM 的最大内存（Maximum memory per VM）: 2TB
- 每个节点的最大 VM 数量（Maximum VMs per host）: N/A – 取决于内存 的分配限制
- 每个集群的最大 VM 数量（Maximum VMs per cluster）: N/A –取决于内存 的分配限制

> 用benchmark测试ESXi主机要将power policy设置为High performance

#### 3. 网络

![ESXi vSwitch 网络概述](http://wx1.sinaimg.cn/large/007ozevdgy1fwsddny2vsj31420om79p.jpg)

### 2) vSphere如何工作

#### 1. 磁盘阵列减轻负载－VAAI

- 克隆的 VM 快照 - > VAAI 将不能使用
- 克隆虚拟机没有快照且处于关机状态 - > VAAI 将被使用 
- 克隆虚拟机到不同的数据存储/容器 - > VAAI 将不能使用 
- 克隆开机状态的虚拟机 - > VAAI 将不能使用

这些方案适用于 VMware View： 
- View 完整克隆（模板与快照） - > VAAI 将不能使用
- View 完整克隆（模板 W/O 快照） - > VAAI 将被使用
- View 链接克隆（VCAI） - > VAAI 将被使用

#### 2. CVM Autopathing aka Ha \. py

> DSF 有一个称为 autopathing 特征，其中当一个 本地 CVM 变得不可用时，I/O 的透明传递给其他 CVMS 集群中的处理。Hypervisor 和 CVM 通信使用一个专用的 vSwitch 的私网地址 192.168.5.0。这意味着，对于所有存储的 I/O，这些都发生在上 CVM（192.168.5.2） 的内部 IP 地址。在 CVM 的外部 IP 地址用于远程复制和 CVM 通信。

![ESXi 主机网络](http://wx1.sinaimg.cn/large/007ozevdgy1fwsdhz7vg1j314g0mkq9o.jpg)

> 在一个本地的 CVM 故障的情况下，先前由本地 CVM 托管在本地 192.168.5.2 地址是不可用的。DFS 将自动检测到该故障并通过万兆网络重定向这些 I/O 到群 集中另一个 CVM 上。Hypervisor 和主机上运行的虚拟机将以透明的方式重新路由。 这意味着，即使一个 CVM 关机，虚拟机仍然会继续能够执行 I/O。一旦本地 CVM 的还原并可用，流量随后将无缝地转交给本地 CVM 继续服务。

![ESXi主机网络-本地CVM下线](http://ws4.sinaimg.cn/large/007ozevdgy1fwsdpy199rj311g0fowll.jpg)

### 3) vSphere管理

ESXi 的集群升级

```

//＃上传离线升级包到 Nutanix NFS 容器
//＃登录到 Nutanix CVM 
//＃进行升级
for i in `hostips`;do echo $i && ssh root@$i "esxcli software vib install -d /vmfs/volumes/<Datastore Name>/<Offline bundle name>";done

//示例
for i in `hostips`;do echo $i && ssh root@$i "esxcli software vib install -d /vmfs/volumes/NTNX- upgrade/update-from-esxi5.1-5.1_update01.zip";done

```

重新启动 ESXi 主机服务

```

//说明：以一个增量的方式重新启动每个 ESXi 主机服务
for i in `hostips`;do ssh root@$i "services.sh restart";done

```

显示 ESXi 主机的网卡“up”状态

```

//说明：显示 ESXi 主机的网卡这是在“UP”状态
for i in `hostips`;do echo $i && ssh root@$i esxcfg-nics -l | grep Up;done

```

显示 ESXi 主机的 10GbE 网卡和状态

```

//说明：显示 ESXi 主机的 10GbE 网卡和状态
for i in `hostips`;do echo $i && ssh root@$i esxcfg-nics -l | grep ixgbe;done

```

显示 ESXi 主机的活动适配器

```

//说明：显示 ESXi 主机的工作，待机和未使用的适配器
for i in `hostips`;do echo $i && ssh root@$i "esxcli network vswitch standard policy failover get -- vswitch-name vSwitch0";done

```

显示 ESXi 主机路由表

```

//说明：显示 ESXi 主机的路由表
for i in `hostips`;do ssh root@$i 'esxcfg-route -l';done

```

检查 VAAI 在 Datastore 上是否启用

```

//说明：检查 VAAI 在 Datastore 上是否启用/支持
vmkfstools -Ph /vmfs/volumes/<Datastore Name>

```

设定 VIB 校验级别为 community supported

```

//说明：设置 VIB 校验程度为 CommunitySupported 允许第三方安装 VIB
esxcli software acceptance set --level CommunitySupported

```

安装 VIB

```

//说明：不检查签名安装 VIB
esxcli software vib install --viburl=/<VIB directory>/<VIB name> --no-sig-check
//或者
esxcli software vib install --depoturl=/<VIB directory>/<VIB name> --no-sig-check

```

检查的 ESXi 虚拟盘空间

```

//说明: 检查 ESXi ramdisk 可用空间
for i in `hostips`;do echo $i; ssh root@$i 'vdf -h';done

```

清除 pynfs 日志

```

//说明：清除每个 ESXi 主机上的 pynfs 记录
for i in `hostips`;do echo $i; ssh root@$i '> /pynfs/pynfs.log';done

```

---

<span id = "Hyper-V">

## Hyper-V

### 1) Hyper-V架构

#### 1. 节点架构

![Hyper-V 的节点架构](http://ws1.sinaimg.cn/large/007ozevdgy1fwse09qq10j30za0rawmj.jpg)

#### 2. 最高配置和可扩展性

- 最大群集大小 ： 64 
- 每个 VM 最大的 vCPU ： 64 
- 每个虚拟机最大内存 ： 1TB 
- 每台主机最大的虚拟机 ： 1024
- 每个集群最大的虚拟机 ： 8000

#### 3. 网络

![Hyper-V Virtual Switch Network Overview](http://ws3.sinaimg.cn/large/007ozevdgy1fwse1rl1o3j314a0tagrw.jpg)

### 2) Hyper-V如何工作

#### 1. 磁盘阵列减轻负载－ODX

> Nutanix 平台支持微软 Offloaded Data Transfer（ODX），允许 Hypervisor卸载某些任务负载到阵列。

目前 ODX 是以下操作调用：

- 在 DSF SMB 共享上的虚拟机或虚拟机 VM 文件副本
- SMB 共享文件副本

ODX 不能通过以下操作来调用：

- 通过 SCVMM 克隆虚拟机 
- 从 SCVMM 库（非 DSF SMB 共享）部署模板 
- XenDesktop 的克隆部署

### 3)Hyper-V管理

在多台远程主机上执行命令

```

//说明：在一个或多个远程主机执行一个 PowerShell
$targetServers = "Host1","Host2","Etc"
Invoke-Command -ComputerName $targetServers {
<COMMAND or SCRIPT BLOCK>
    
}

```

检查可用 VMQ Offloads

```

//Description: Display the available number of VMQ offloads for a particular host 说明：显示特定主机 VMQ Offloads 的可用数量
gwmi –Namespace "root\virtualization\v2" –Class Msvm_VirtualEthernetSwitch | select elementname, MaxVMQOffloads

```

禁用匹配特定的前缀虚拟机的 VMQ

```

//说明：禁用特定虚拟机的 VMQ
$vmPrefix = "myVMs" 
Get-VM | Where {$_.Name -match $vmPrefix} | Get-VMNetworkAdapter | Set-VMNetworkAdapter - 
VmqWeight 0

```

启用匹配特定的前缀虚拟机的 VMQ

```

//说明：启用特定虚拟机的 VMQ
$vmPrefix = "myVMs" 
Get-VM | Where {$_.Name -match $vmPrefix} | Get-VMNetworkAdapter | Set-VMNetworkAdapter - 
VmqWeight 1

```

开机特定前缀的虚拟机

```

//说明： 将特定前缀的虚拟机开机
$vmPrefix = "myVMs" 
Get-VM | Where {$_.Name -match $vmPrefix -and $_.StatusString -eq "Stopped"} | Start-VM

```

关机特定前缀的虚拟机

```

//说明： 将特定前缀的虚拟机关机
$vmPrefix = "myVMs" 
Get-VM | Where {$_.Name -match $vmPrefix -and $_.StatusString -eq "Running"}} | Shutdown-VM - 
RunAsynchronously

```

停止特定前缀的虚拟机

```

//说明： 将特定前缀的虚拟机停止
$vmPrefix = "myVMs" 
Get-VM | Where {$_.Name -match $vmPrefix} | Stop-VM

```

获取 Hyper-V 主机的 RSS 设置

```

//说明：获取 Hyper-V 主机 RSS(recieve side scaling 设置
Get-NetAdapterRss

```

检查 Winsh 和 WinRM 连接

```

//说明：通过执行一个简单的查询检查 Winsh 和 WinRM 连接/状态，应返回“the computer system object not an error”
allssh "winsh "get-wmiobject win32_computersystem"

```

---

<span id = "XenServer">

## XenServer

Coming soon!

---

<span id = "Foundation">

## Foundation

> Foundation 是 Nutanix 提供的系统工具，用于引导，安装镜像和部署 Nutanix 集群.镜像过程会安装所需版本的 AOS 软件和所选的 Hypervisor 软件。

> 默认情况下，Nutanix 节点出厂会预装 AHV 虚拟化平台，如果需要不同的 Hypervisor 平台，你必须用 Foundation 软件重新对所有节点采用所需的 Hypervisor 进行重新镜像操作。

![Foundation-架构](http://ws3.sinaimg.cn/large/007ozevdgy1fwsediqalkj30xy0iotct.jpg)

Applet

![Foundation-小程序架构](http://wx4.sinaimg.cn/large/007ozevdgy1fwseer4nrqj30xw0iaq7b.jpg)

> 注意：发现小程序仅仅是发现和代理运行在所有节点上的 Foundation 服务一 种方式。所有镜像和配置工作都由 Foundation 服务来完成，而不是小程序本身。 

> 高级技巧： 如果你位于和目标的 Nutanix 节点（比如跨广域网）不同的网络（L2），而 CVM 已经配置了 IPv4 地址，你可以直接连接到 CVM 的 Foundation 服务（而不是 用发现小程序）。

> 直接将浏览器连接到：<CVM_IP>:8000/gui/index.html 来访问

---

## 总结

### Nutanix方案优点：

1）本地落盘策略，确保虚机访问存储速度：虚机写入的数据都在本物理节点的磁盘上，避免跨节点存储访问，确保访问速度，减轻网络压力。

2）采用SSD磁盘作为数据缓存，大幅提升IO性能。

3）永远优先写入SSD，确保高IO性能数据写入HDD不参与，即使本地SSD容量满了会将冷数据迁移到集群其他节点SSD，然后还是SSD进行读写，确保高IO。后续异步将SSD冷数据迁移到HDD。

4）数据冷热分层存储冷数据存放在HDD，热数据保留在SSD，确保热点数据高IO读取。

5）设备密度高，节省机房机架空间2U可以配置4个节点，包含了存储与计算，比以往机架式/刀片服务器与磁盘阵列的解决方案节省了大量的空间。

### Nutanix方案缺点：

1）本地落盘及SSD缓存方案确保了高IO，但是硬盘的带宽得不到保证。传统磁盘阵列，多块SATA/SAS硬盘加入Raid组，数据写入的时候，将文件拆分为多个block，分布到各个硬盘中，同个Raid组的硬盘同时参与该文件的block的读写。通过多块硬盘的并行读写，从而提升IO与带宽性能。而Nutanix的解决方案中，单个文件的读写遵循本地落盘的策略，因此不再对文件拆分到多块硬盘进行并行读写，而只有本地节点的SSD硬盘会对该文件进行写入。虽然SSD硬盘的IO与带宽都是SATA/SAS的数百上千倍，但是SSD对比SATA/SAS硬盘在带宽上面只有2~3倍的速率提升，而传统Raid的方式，多块硬盘并行读写，虽然IO比不上SSD，但是带宽则比单块/两块SSD带宽高出很多。因此Nutanix的解决方案适合用于高IO需求的业务类型，但是因为它的读写原理，则决定了它不合适低IO、高带宽的业务类型。[^4]

---

## [回到顶部](#回到顶部)

[^1]:[超融合基础架构-百度百科](https://baike.baidu.com/item/%E8%B6%85%E8%9E%8D%E5%90%88%E5%9F%BA%E7%A1%80%E6%9E%B6%E6%9E%84/16569527?fr=aladdin)

[^2]:[Nutanix-维基百科](https://zh.wikipedia.org/wiki/Nutanix)

[^3]:[Nutanix官网](https://www.nutanix.com/)

[^4]:[Nutanix是什么?-知乎](https://www.zhihu.com/question/22864413/answer/110939149)


