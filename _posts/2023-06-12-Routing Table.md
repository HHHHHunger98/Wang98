---
layout: post
title: Routing Table
date: 2023-06-12
tags: network
---

# 路由表 Routing Table

本文回答了以下问题：
1. 什么是路由表？
2. 路由表有什么用？

## 什么是路由表，What are the Routing Tables?

> why are routing tables so important in a network?

**Routing tables are essential in the routing because they maintain a map of connected networks,** 简单来说，路由表记录了互相连接的网络的信息，没有路由表，那么路由器将不知道如何将网络数据包发送到他们的目标地址，路由表保证了网络数据包在网络中传递的效率，**因为没有routing table，整个网络数据包的传递过程将会特别耗时和混乱

<!--more-->

> 什么是路由器？

**A router is a device that acts as a gateway to a network and is also responsible for forwarding packets or messages to destination addresses.**
简单来说，路由器就是一个像门户一样的设备，他负责转发由网络中设备发来的网络数据包或者消息去到他们的目标地址，除此之外，路由器还有着最优路线选择的功能，该功能是基于我们要说的Routing Table实现的

> 什么是路由表？

**A routing table is a table or database that stores the location information( IP addresses ) of routers, This table acts as an address map to various networks** 一般路由表被保存在路由器或其他网络设备的RAM中

> 路由表生成的方法

有两种，分别是动态生成**dynamically**和静态生成**statically**：
- 一般根据路由器协议动态生成路由表
- 静态生成是指人工添加路由到路由表中

## 路由表中的项 Routing Table Entries

一个简单的路由表如下图所示：

![routingtableentries]({{site.baseurl}}/assets/img/routingtableentries.jpg)

> 各个项的都是什么？

- Network Destination：指目标网络的IP地址
- Netmask/Subnet Mask：子网掩码，用于匹配目标网络时的计算
- Gateway/Next Hop：指代下一跳，即路由器将会把数据包转发到哪一个地方去 **this refers to the next IP address to which the packet is forwarded.**
- Interface：由哪个网卡转发，数据包将由哪个网卡发送去目标地址
- Metric：用于优化路由选择的一个指标，一般是去到目标地址的跳数（Hop Number）或者经过的路由器数量，当有多个路径存在时，通常会让路由器选择该指标较小的路由进行包转发，this assigns a value to each route to ensure that optimal routes are chosen for sending packets. In some instances, the metric is the number of hops or number of routers to be crossed to get to the destination network. If multiple routes exist, the route with the lowest metric is usually chosen.

> 路由选择的例子:

![路由选择]({{site.baseurl}}/assets/img/路由选择1.jpg)
1. 假设我们有一个网络是如上图所示的，PC1想要给PC3发送一条数据，因为他们不在同一个网络下，因此，当路由器接收到PC1发来的包时，他需要查询路由表看看有没有匹配的项

2. 具体查询过程就是目标的IP地址和子网掩码的按位与操作，如果匹配，这选择该路由

3. 查询后发现，表中第一项符合，于是就通过路由器的网卡eth3(接口，interface)把包发送给下一跳的地址10.0.0.2（网关，gateway）

![路由选择]({{site.baseurl}}/assets/img/路由选择2.jpg)

如上图所示，如果路由表中没有匹配的项，那么该如何做呢？
- 路由表中会有一个默认网关，他的子网掩码为0.0.0.0，即没有其他项与目标IP相匹配的话，路由器便会将包发向默认网关
- 比如当PC1想要发一个数据包给PC25(IP：200.0.2.2)的时候，路由器A的路由表没有网段(200.0.2.0)的信息，于是将包由默认网关转发出去

> 默认网关

**The default gateway route is always present in any routing table. It’s used when there is no entry for a specific network on the routing table. The default gateway usually connects to other remote networks. For example, in a home environment, the default gateway is connected to the Internet.**

> Types of Routing Table Routes

除了上述的内容，还有一个比较重要的点是：路由表中的路由是有两种类型的：
1. 直接路由，指的是路由器的网卡直连到对应的目标网络，称直接路由，**directly connected route**
2. 远程路由，一般是通过路由协议动态学习得到的路由，与该路由器并不直接相连，为远程路由 **remote routes**

## 总结

本文简单介绍了路由表是什么，以及其相关的应用和机制
