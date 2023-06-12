---
layout: post
title: NetworkBasic
date: 2023-06-03
tags: network
---

# <span style="color: red;">Computer Network 计算机网络</span>

## <span style="color: red;">1 学习计网的意义</span>

> 对于计网这个学习计算机专业的人必须学习和掌握的东西，有其一个整体的大局观是很有必要的
>
> 因此第一个问题：为什么我要学他？

就个人理解，只有了解了计算机之间的信息传输共享的方法，计算机网络的运作底层机制，作为计算机从业人员才能更好的利用他。
<!--more-->
> 接踵而来的是第二个问题：学些什么？
> 
> 我们可以看看计算机网络的定义，WiKi上对 Computer Network 的定义是：
> A computer network is a set of computers **sharing resources** located on or provided by network nodes. Computers use common **communication protocols** over digital interconnections to communicate with each other. These interconnections are made up of **telecommunication network technologies** based on physically wired, optical, and wireless radio-frequency methods that may be arranged in a variety of network topologies.

到底是在学什么呢？简单来说，计算机网络的目的就是资源共享和信息传递，实现的方式是将计算机（通俗说就是能够上网的设备）组成网络，利用各种各样的传输协议（TCP/IP, http, UDP等）进行信息交换和共享，同时传递信息需要媒介，因此有各种各样的通信网络技术（WAN, LAN, 光纤等）因此，按照这种理解，我们学习计算机网络，很大一部分就是学习计算机如何进行资源传递共享的，包括物理意义上的线路，以及逻辑层面的如何进行信息交换，也就是各种协议，各种传输技术。

## <span style="color: red;">2 基础知识</span>

> 为什么要有TCP/IP协议，ta是用来干什么的？

我们已经了解了，**计算机网络**的目的就是信息交流和共享，如何共享传递信息呢? 对于同一台设备上的进程间通信，我们有很多种方式实现:
1. 管道 pipe
2. 高级管道 popen
3. 有名管道 named pipe
4. 消息队列 message queue
5. 信号量 semophore
6. 共享内存 shared memory
7. 套接字 socket

但，对于不同设备上的进程间通信，就需要网络通信，为了兼容各种不一样的设备，就必须统一出一套**通用的网络协议**

> 为什么分层，有几层？

其实有两种网络模型，OSI（7层） 和 TCP/IP（4 层） ，前者为理论模型，后者为现实实现的模型

先从答案入手吧，**TCP/IP 模型分为4层**，分别是：
1. 应用层 Application Layer
2. 传输层 Transport Layer
3. 网络层 Internet Layer
4. 网络接口层 Link Layer

下层为上层服务，为上层提供接口，同时消息传递可以看成是对应层之间的消息传递，比如应用层对应用层，即协议是在对应的层之间使用的，我的理解，每一层所提及的协议是针对当前层的！


### <span style="color: red;">2.1 TCP/IP 各层功能介绍</span>

#### <span style="color: red;">Application Layer</span>

简单来说，我们使用的很多软件都在这一层，当不同设备上的应用通信的时候，就会把数据按当前层相应的协议处理后再交给下一层传输层来进一步加工

应用层的协议有：HTTP, FTP, Telnet, DNS, SMTP 等

#### <span style="color: red;">Transport Layer</span>

在这一层有两个著名的协议:
1. TCP Transmission Control Protocol
    - 大部分应用使用的就是TCP传输协议，HTTP应用层协议，TCP准确来说是面向链接的可靠的传输协议，因此有许多特性保证传输的可靠，比如流量控制，超时重传，拥塞控制
2. UDP User Datagram Protocol
    - UDP 相对简单，只负责传输数据包，不管他能不能到对面，保证实时和效率

**数据分段** 有的时候如果应用层数据太大，超过MSS( Maximum Segment Size )最大报文段长度，就要将数据分块，有什么好处，如果其中某个块传丢了，不用整个数据重传，只用传这一个分块就行

**端口号** 一台设备可能有很多应用，如何能让传来的数据准确找到某个应用，这个是靠端口号来实现的，80端口是Web服务器用的，22端口远程登陆服务器使用的，端口号在传输层添加

#### <span style="color: red;">Internet Layer</span>

网络层主要负责的是数据在网络中的传递，即负责数据从一个设备到另一个设备的传输，而传输层所负责的是数据在应用间的传递（虽然他叫传输层，但不负责数据在计算机网络中的传递）

##### <span style="color: red;">网络层常用的协议：</span>
- IP Internet Protocol 协议
    - 主要作用是将传输层的报文作为数据部分打包，加上IP包头组成IP报文，如果大小超过MTU( Maximum Transmission Unit 最大传输单元 ) 就会进行**分片**

![传输网络层分段]({{site.baseurl}}/assets/img/传输网络层分段.jpg)

##### <span style="color: red;">我们常说的IPv4 和 IPv6 是干嘛的？</span>
- **目的**：由于网络层负责数据在设备之间的传输，自然需要区分设备，因此需要一个协议来完成这项任务，就有来IPv4 和 IPv6 分别是用32bit 和 128bit 来标识一个设备在网络中的“代号”
- **划分**：IP 地址有两种意义：
    - 网络号，标识IP 属于那个网络
    - 主机号，标识IP 是网络下那个主机
- 怎么划分的？利用**子网掩码**算出，方法就是用IP 地址和子网掩码进行**按位与运算**

![IP子网掩码]({{site.baseurl}}/assets/img/子网掩码.jpg)

寻址过程大概如下： 先匹配到相同的网络号，然后再去找对应的主机号

##### <span style="color: red;">IP协议的另一个功能，路由 routing</span>
- **目的**：找到目标地址所在的子网，将数据包往对应的网络中发送

**所以，IP 协议的寻址作用是告诉我们去往下一个目的地该朝哪个方向走，路由则是根据「下一个目的地」选择路径。寻址更像在导航，路由更像在操作方向盘**

#### <span style="color: red;">Link Layer</span>
网络接口层会在IP头部前加上MAC头部，将数据封装成Data Frame（数据帧）发送到网络上

##### <span style="color: red;">MAC地址是什么？</span>
MAC（media access control）address 是一个独特的，用来标识网络设备位置的地址，又称Ethernet Address 或 Physical Address，MAC地址用来在网络中唯一的标识一个网卡

格式：48bit 十六进制表示，01:xx:xx:xx:xx:xx

**为什么用MAC地址呢？** 因为数据在以太网中的传输思路和网络层的寻址路由有些不同，我们可以把IP 地址看作软件支持，而MAC地址是硬件支持，设备的MAC地址通常保持不变，而设备的IP地址会根据设备所在的网络而变化

IP 网络会管理和支持一个从IP 地址到设备MAC 地址的映射表，这就是 **ARP 协议（address resolution protocol 地址解析协议）** 改表会一直更新

MAC 头部会在Link Layer 中被添加到数据头部，其中包含接收方和发送方的MAC地址

#### <span style="color: red;">Summery</span>
TCP/IP 总共有四层，每层都包含和负责一项特定的Functionality，他们分别是 应用层，传输层，网络层，网络接口层

### <span style="color: red;">2.2 键入网址到网页显示，其间发生了什么？</span>

#### <span style="color: red;">大致过程</span>

1. 第一件事就是浏览器解析键入的**URL**，生成**HTTP**消息
2. 真实地址查询**DNS** 域名系统（会新开一篇blog单独细说）确定服务器对应的IP 地址
3. 获得目标IP地址后，就可以吧HTTP的传输工作交给OS中的**协议栈**
4. 协议栈将打包好的数据交给网卡驱动程序，负责控制网卡硬件
5. 最下面的网卡将会负责完成实际的收发操作，即网线中信号的发送和接受操作

> 下图展示了 TCP/IP模型 中各个层中都有哪些协议：

![InternetProtocolSuite]({{site.baseurl}}/assets/img/InternetProtocolSuite.jpg)

#### <span style="color: red;">协议栈分层</span>
![协议栈]({{site.baseurl}}/assets/img/协议栈.jpg)
##### <span style="color: red;">协议层作用</span>
协议栈在操作系统中，内部分为几个部分，分别负责不同的内容,根据上图所示，浏览器（应用程序）调用socket库，委托协议栈工作，协议栈负责HTTP的传输，为其提供导航指南。(按道理，包括传输层和网络层的工作)
##### <span style="color: red;">协议栈工作流程</span>
协议栈大致分为两层：
1. 负责收发数据的**TCP**和**UDP**协议，属于传输层协议，负责接受应用层委托的数据收发操作
2. 负责网络包收发的**IP**协议，将上层数据切分成一块块的网络包，将网络包发送给目标的操作就是**IP**协议的工作，**IP** 协议包含两个重要的子协议：
    1. ICMP (Internet Control Message Protocol 互联网控制消息协议)It is used to **send error messages and operational informations indicating success or failure when communicating with another IP address**
    2. ARP (Address Resolution Protocol 地址解析协议) 前面有提到过，负责将IP地址转化对应的MAC地址，方便数据在以太网中的传输

#### <span style="color: red;">协议栈之TCP</span>
> HTTP是基于TCP协议传输的，TCP协议是面向可靠连接的，因此常说的**三次握手建立连接** 以及 **四次分手解除连接** 都是TCP的特性

##### <span style="color: red;">TCP包头格式</span>

![TCP包头格式]({{site.baseurl}}/assets/img/TCP包头格式.jpg)

简单概念：
1. **源端口号 和 目标端口号**：指明具体收发方应用
2. **序号**：指代包的序号，因为有可能做分片，为了解决包乱序的问题
3. **确认号**：目的是确认对方是否受到包，没收到就重发，为了解决丢包问题
4. **状态位**：为了连接的稳定，因此需要状态位反应双方状态变更
    1. `SYN` 发起连接
    2. `ACK` 确认回复
    3. `RST` 重新连接
    4. `FIN` 结束连接
5. **窗口大小**：为了TCP流量控制，连接双方申明一个窗口，表示处理能力，即缓存大小

##### <span style="color: red;">TCP三次握手建立连接 和 TCP四次分手解除连接</span>

> 在HTTP数据传输之前，需要建立TCP连接，因为三次握手和四次分手大致相同，因此放在一起说明

![TCP三次握手]({{site.baseurl}}/assets/img/TCP三次握手.jpg)
![TCP四次分手]({{site.baseurl}}/assets/img/tcpclose.png)

> 不管是建立还是解除连接，目的是为了保证连接双方达成一致，**都具备收发能力** 或 **都传完数据想要解除连接**

##### <span style="color: red;">TCP分割数据</span>
> 当HTTP消息过长，超过了`MSS`长度，TCP就会把消息分割成几块，而不是一次性发送所有的数据

![MTU和MSS]({{site.baseurl}}/assets/img/MTU和MSS.jpg)

##### <span style="color: red;">TCP报文生成</span>

TCP协议中的端口：
1. 浏览器监听端口，通常为随机生成的
2. Web服务器监听端口，通常为默认端口号（根据协议会有不同，HTTP为80，HTTPS为443）

> **So far so good，至此，TCP报文可以组装好了：**

![tpc报文]({{site.baseurl}}/assets/img/tcp报文.jpg)

#### <span style="color: red;">协议栈之IP</span>

![IP包格式]({{site.baseurl}}/assets/img/IP包格式.jpg)

如上图所示，些许概念解释如下：
1. 指出**源地址IP（客户端输出IP）**和**目标地址IP（DNS域名解析得到的Web服务器IP）**
2. **协议**：指数据部分使用的是什么协议，此处为TCP协议，因为HTTP是基于TCP的，该位置为8bit，`06`十六进制，表示协议为TCP

##### <span style="color: red;">源地址选择</span>

> 当客户端有很多个网卡时，源地址的填写由**路由表**决定，表示由哪个网卡发送包

```shell
$ ip route show
```
> What is a **Routing Table**?

A routing table, or routing information base (RIB), is a **data table** stored in a **router or a network host** that lists the routes to particular network destinations, and in some cases, metrics (distances) associated with those routes. **The routing table contains information about the topology of the network immediately around it**

![源IP选择]({{site.baseurl}}/assets/img/源IP选择.jpg)

> Remark: the default gateway is 0.0.0.0 with mask 0.0.0.0, when all the other IP in the routing table do not match with the destination IP, then the data package will be passed to the router.

##### <span style="color: red;">IP报文生成</span>

> So far so good, and we generated the finial IP package:

![IP包全貌]({{site.baseurl}}/assets/img/IP包全貌.jpg)

#### <span style="color: red;">协议栈之MAC</span>

> 讲道理，MAC属于网络接口层，即 Link Layer，负责两点之间的传递，即同以太网内或局域网内的点到点的传输，我们现在还需要在网络包前部加上MAC头部

![MAC头部]({{site.baseurl}}/assets/img/MAC头部.jpg)

MAC包中包含：
1. 发送方MAC地址
2. 接受方MAC地址
3. 协议类型：只有两种
    1. IP协议：0800
    2. ARP协议：0806

##### <span style="color: red;">如何确认发送方和接收方MAC地址？</span>

> 发送方：MAC地址为网卡生产时写到ROM里面的，读取该值写入到源MAC地址区域就行

> 接收方：一般填写下一跳的MAC地址（网关，gateway），通过查询**routing table 路由表**得到下一条的IP地址，然后通过**ARP协议**帮助我们找到对应的MAC地址

> ARP协议如何帮助我们找到对应的MAC地址？

知道下一跳IP? 却不知道下一跳MAC地址？那就广播呗...

`ARP`协议帮我找到下一跳的MAC地址

![广播ARP确定MAC地址]({{site.baseurl}}/assets/img/广播ARP确定MAC地址.jpg)

过程很简单，就是ARP协议会在以太网中以广播的形式对询问所有设备“谁知道这个IP地址？请告诉我你的MAC地址”，有相应的设备就会回应说“我的IP地址是这个，我的MAC地址是XXX”，于是就有了目标MAC地址了，MAC头部就添加完成了

> ARP缓存

主要作用就是避免了查询MAC地址时候的广播，没事就喊大喇叭很烦的，于是就有ARP缓存，首先会在缓存中查找对方的MAC地址，有的话就不用广播了，ARP缓存在内存空间中，一般有效期就几分钟

> MAC报文

So far so good, now we have generated the MAC frame

![MAC报文]({{site.baseurl}}/assets/img/MAC报文.jpg)

#### <span style="color: red;">Interface: Network Card 网络接口层</span>

**生成了网络包数据，但他们只是数字信息，要将其发送出去只能将他们转化为电信号，网卡就负责这一工作！**

> 网卡(Network Interface Card)如何传输信号？

网卡的工作需要网卡驱动程序的配合

- 对于将要发送出去的数据，首先会先将数据包缓存到网卡的缓存区中，然后添加上**报头和起始帧分界符**(Preamble and start frame delimiter,一般为56bit的Preamble和8bit的帧起始定界符)
- 同时在数据末尾加上**Frame Check Sequence 帧校验序列**，一般为**32bit的CRC(Cyclic Redundancy Code 循环冗余码)**用于错误检测

相关概念
1. 起始帧分界符，start frame delimiter，是一个用来表示包起始位置的标记
2. FCS,帧校验序列，检测包在传输过程中是否损坏

然后网卡将包转化为电信号，通过网线发送出去

> 补充知识 NIC 如何接收数据包

1. 比特信号通过网线等媒介到达网卡，收到的信号，会以帧的形式接受
2. 首先，FCS（CRC循环校验码）会检验包是否损坏，**如果不匹配意味着损坏，直接丢弃该包**
3. 如果没问题，会检测目标MAC地址是否匹配，匹配则将帧头部和尾部去掉交给上层进行进一步的处理

#### <span style="color: red;">交换机————送别者</span>

> 交换机的功能

工作在MAC层，即数据链路层，Link Layer，也被称为**二层网络设备**，主要作用是将网络包原样转发到目的地址

> 交换机workflow 工作流程

- 电信号到达网线接口，交换机内模块进行接收，将电信号转化为数字信号
- 校验数据帧末尾的`FCS(Frame Check Sequence)`，如果有问题就丢弃，没问题就直接放缓冲区
- 与网卡设备(NIC, network interface card)不同的是，**工作在二层的交换机不进行目的MAC地址校对，并且交换机不具备MAC地址**

**Remark**：如果只有交换功能的交换机不具备MAC地址，因为不需要，但为了支持上层服务，现在很多的交换机都有MAC地址，原因是当你直接与交换机对话时，比如作为管理的交换机（a managed switch）MAC地址是必要的，这句话怎么理解呢？

STP: Spanning Tree Protocol, 生成树协议，是为了防止交换机冗余链路产生的环路，用于确保以太网中无环路的拓扑结构，从而避免广播风暴占用交换机资源，关于STP协议，有机会开一个新的篇章学习一下

简单来说，对于支持STP的交换机，是需要MAC地址的，一个STP桥是需要一个独特的bridge ID的，通常会用MAC地址来作为这个管理的bridge ID

**我自己也暂时比较懵，暂时说到这里吧**

> MAC 地址表

交换机会维护一个MAC地址表，功能和路由表相识，原理大致相同，其主要包含两个信息：
1. 设备的MAC地址
2. 设备与交换机端口的连接信息

当交换机将接受来的包放进缓冲区，接下来就需要查询一下该包的接收方MAC地址在不在表中：
- 如果在表中，直接发到对应的交换机端口上去
- 如果不在表中，**这可能是因为具有该地址的设备还没有向交换机发送过包，或者这个设备一段时间没有工作导致地址被从地址表中删除了**
    - 这种情况直接将包广播到除了源端口的其他端口上去，如果有回应，交换机就会记下对应的MAC地址，方便下次查询

- MAC广播地址：FF:FF:FF:FF:FF:FF
- IP广播地址：255.255.255.255

#### <span style="color: red;">路由器————出境大门</span>

> 到达路由器后，下一步去哪？

从交换机到达路由器，下一步将会是被转发到下一个路由器或这目标设备

这个转发的原理和交换机类似，但也有不同，因为路由器工作在网络层，为三层结构，基于IP设计的，**路由器的各个端口都有独立的MAC地址和IP地址**

> 路由器的workflow

- 基本原理和网卡NIC类似，分别校验`FCS`和`目标MAC地址`，如果匹配，则放入缓冲区
- 对于在缓冲区的包，路由器会**remove the frame header,去除帧头部**，即去掉MAC头部
- 然后根据目的IP地址，查询**Routing Table**判断转发目标，[<span style="color: blue;">具体查询原理，可参考这篇博客</span>]({{site.baseurl}}{% link _posts/2023-06-12-Routing Table.md %})
- 对应查询得到的网关（gateway）地址不为空，则还需要继续转发，为空，则目标IP就是自己的IP头部中的接收方IP
- 得到下一跳的目的IP之后，需要通过ARP协议获取对方MAC地址，同样，ARP协议也有缓存，可以先查缓存，没有再广播问MAC地址
- 有了接收方MAC地址，源地址填写输出端口的MAC地址
- 打包完成( 添加MAC头部 )以后就可以将数字信号转化为电信号发送了

<span style="color: purple;">Remark: 在网络包传输的过程中，源 IP 和目标 IP 始终是不会变的，一直变化的是 MAC 地址，因为需要 MAC 地址在以太网内进行两个设备之间的包传输</span>


#### <span style="color: red;">服务器和客户端————到达</span>

终于经过重重转发，到达了终点，接着就是层层拆包的过程，同样，每一层会有相应的校验，如果校验通过则保留并向上传递，不通过这丢弃

![拆包和打包]({{site.baseurl}}/assets/img/拆包和打包.jpg)

1. 首先校验FCS
2. 校验MAC地址，是不是发给我的包
3. 检测IP头，查看所使用的协议是什么，是不是TCP？还是UDP? 是否是发给我的包
4. 查看TCP头部，确认序列号，看看是不是我想要的序列包，**是就放入缓存并返回一个ACK**
5. TCP头部还有端口号，HTTP服务正在监听该端口号，于是将该包发给HTTP进程
6. 服务器HTTP进程看到他是HTTP请求，返回所请求的数据内容，该数据内容再经历一次从上到下的打包过程


## <span style="color: red;">总结</span>

本文是关于xiaolincoding的图解网络的学习笔记，为基础部分，分为两个子章节：
1. TCP/IP模型简介
2. 键入网站到网页显示的通信全过程
