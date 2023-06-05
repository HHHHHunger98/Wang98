---
layout: post
title: NetworkNote
date: 2023-06-03
tags: network
---


# Computer Network 计算机网络

## 1 学习计网的意义

> 对于计网这个学习计算机专业的人必须学习和掌握的东西，有其一个整体的大局观是很有必要的
>
> 因此第一个问题：为什么我要学他？

就个人理解，只有了解了计算机之间的信息传输共享的方法，计算机网络的运作底层机制，作为计算机从业人员才能更好的利用他。

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

> Application Layer

简单来说，我们使用的很多软件都在这一层，当不同设备上的应用通信的时候，就会把数据按当前层相应的协议处理后再交给下一层传输层来进一步加工

应用层的协议有：HTTP, FTP, Telnet, DNS, SMTP 等

> Transport Layer

在这一层有两个著名的协议:
1. TCP Transmission Control Protocol
2. UDP User Datagram Protocol
