---
layout: post
title: NetworkBasic
date: 2023-06-03
tags: network
---

# Computer Network 计算机网络

## 1 学习计网的意义

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

## 2 基础知识

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


### 2.1 TCP/IP 各层功能介绍

#### Application Layer

简单来说，我们使用的很多软件都在这一层，当不同设备上的应用通信的时候，就会把数据按当前层相应的协议处理后再交给下一层传输层来进一步加工

应用层的协议有：HTTP, FTP, Telnet, DNS, SMTP 等

#### Transport Layer

在这一层有两个著名的协议:
1. TCP Transmission Control Protocol
    - 大部分应用使用的就是TCP传输协议，HTTP应用层协议，TCP准确来说是面向链接的可靠的传输协议，因此有许多特性保证传输的可靠，比如流量控制，超时重传，拥塞控制
2. UDP User Datagram Protocol
    - UDP 相对简单，只负责传输数据包，不管他能不能到对面，保证实时和效率

**数据分段** 有的时候如果应用层数据太大，超过MSS( Maximum Segment Size )最大报文段长度，就要将数据分块，有什么好处，如果其中某个块传丢了，不用整个数据重传，只用传这一个分块就行

**端口号** 一台设备可能有很多应用，如何能让传来的数据准确找到某个应用，这个是靠端口号来实现的，80端口是Web服务器用的，22端口远程登陆服务器使用的，端口号在传输层添加

#### Internet Layer

网络层主要负责的是数据在网络中的传递，即负责数据从一个设备到另一个设备的传输，而传输层所负责的是数据在应用间的传递（虽然他叫传输层，但不负责数据在计算机网络中的传递）

##### 网络层常用的协议：
- IP Internet Protocol 协议
    - 主要作用是将传输层的报文作为数据部分打包，加上IP包头组成IP报文，如果大小超过MTU( Maximum Transmission Unit 最大传输单元 ) 就会进行**分片**

![传输网络层分段]({{site.baseurl}}/assets/img/传输网络层分段.jpg)

##### 我们常说的IPv4 和 IPv6 是干嘛的？
- **目的**：由于网络层负责数据在设备之间的传输，自然需要区分设备，因此需要一个协议来完成这项任务，就有来IPv4 和 IPv6 分别是用32bit 和 128bit 来标识一个设备在网络中的“代号”
- **划分**：IP 地址有两种意义：
    - 网络号，标识IP 属于那个网络
    - 主机号，标识IP 是网络下那个主机
- 怎么划分的？利用**子网掩码**算出，方法就是用IP 地址和子网掩码进行**按位与运算**

![IP子网掩码]({{site.baseurl}}/assets/img/子网掩码.jpg)

寻址过程大概如下： 先匹配到相同的网络号，然后再去找对应的主机号

##### IP协议的另一个功能，路由 routing
- **目的**：找到目标地址所在的子网，将数据包往对应的网络中发送

**所以，IP 协议的寻址作用是告诉我们去往下一个目的地该朝哪个方向走，路由则是根据「下一个目的地」选择路径。寻址更像在导航，路由更像在操作方向盘**

#### Link Layer
网络接口层会在IP头部前加上MAC头部，将数据封装成Data Frame（数据帧）发送到网络上

##### MAC地址是什么？
MAC（media access control）address 是一个独特的，用来标识网络设备位置的地址，又称Ethernet Address 或 Physical Address，MAC地址用来在网络中唯一的标识一个网卡

格式：48bit 十六进制表示，01:xx:xx:xx:xx:xx

**为什么用MAC地址呢？** 因为数据在以太网中的传输思路和网络层的寻址路由有些不同，我们可以把IP 地址看作软件支持，而MAC地址是硬件支持，设备的MAC地址通常保持不变，而设备的IP地址会根据设备所在的网络而变化

IP 网络会管理和支持一个从IP 地址到设备MAC 地址的映射表，这就是 **ARP 协议（address resolution protocol 地址解析协议）** 改表会一直更新

MAC 头部会在Link Layer 中被添加到数据头部，其中包含接收方和发送方的MAC地址

#### Summery
TCP/IP 总共有四层，每层都包含和负责一项特定的Functionality，他们分别是 应用层，传输层，网络层，网络接口层

### 2.2 键入网址到网页显示，其间发生了什么？

#### 大致过程

1. 第一件事就是浏览器解析键入的**URL**，生成**HTTP**消息
2. 真实地址查询**DNS** 域名系统（会新开一篇blog单独细说）确定服务器对应的IP 地址
3. 获得目标IP地址后，就可以吧HTTP的传输工作交给OS中的**协议栈**
