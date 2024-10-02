---
layout: post
usekatex: false
title: NetworkControlSystem_CLC
date: 2023-06-20
tags: learning CLC
---

<!--# <span style="color: red;">Computer Network 计算机网络</span>-->

# Learning Note for Closed-Loop Control

This article is the note for learning the course `Closed-Loop Control in Networked Control System`

## why should we learn it? 学习的理由

It is not only for my master thesis, the `closed-loop control(CLC)` is an essential part of the control theory and it is widely used in system design and evaluation. In order to build a rubust, health and low-cost system, it is necessary to know the control theory and CLC.

<!--more-->

## Types of control systems

Generally, there are two types of control systems:
1. Open-loop control system
2. Closed-loop 

### <span style="color: blue;">Open-Loop 开环控制系统</span>

> 何为开环？

简单说就是，系统自己的输出不会对系统的控制产生影响，如下图所示，开环控制就是前向反馈控制系统(Feedforward Control)

![开环系统]({{site.baseurl}}/assets/img/openloopsys.jpg)

举个例子：
洗衣机，他的运行只是根据初始设定的时间，而不是衣服干湿度

![开环例子]({{site.baseurl}}/assets/img/openloopexample.jpg)

> 优点 Advantage
1. architecture of the system is very simple, low cost
2. 控制器与被控制对象只有顺序作用而没有反向联系，the control is one-directional

> 缺点 Disadvantage
1. lack of accuracy
2. unable to correct themselves

### <span style="color: blue;">Closed-Loop 闭环控制系统</span>

> 何为闭环？

简单来说就是，系统的输出端和输入端有反馈回路，系统的输出对控制过程有直接影响(Feedback Control)

如下图所示：

![闭环系统]({{site.baseurl}}/assets/img/closedloopsys.jpg)

### <span style="color: blue;">Networked Control System 网络化控制系统</span>

> 什么是网络化控制系统 NCS?

简单来说，控制回路是利用网络实现的，系统中的反馈以及信号的传递是通过网络包的形式实现的

> 基本的概念 basic concepts

基本的NCS有以下基础元素：
1. Sensors, 传感器，收集信息
2. Controllers, 控制器，作为决策者下达指令
3. Actuators, 执行器，执行控制器下达的指令
4. Communication network, 交流网络，为各个元件之间信息交流提供服务

![ncs]({{site.baseurl}}/assets/img/ncs.jpg)

> 有什么作用？

- connect the cyberspace and the physical space 连接网络世界和现实世界
- enable the tasks to be executed from long distance 支持分布式处理任务
- 减少了不必要的连接线路的铺设，reduce the unnecessary wiring
- 降低了系统的复杂度和设计难度，
- 降低了搭建系统的成本
- 易于控制，简单来说可以在不修改系统结构的情况下通过添加传感器来使系统具备更多的功能

> 优缺点
- 优点：
    - low cost of wiring
    - decoupling the control hardware from the system, easy to maintenance
    - more flexible
- 缺点：
    - more complex management, more error sources
    - additional time delay
    - security issues



