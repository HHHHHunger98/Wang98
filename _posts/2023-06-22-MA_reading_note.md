---
layout: post
title: MA_reading_note
date: 2023-06-22
tags: learning papers MA
---

<!--# <span style="color: blue;">Computer Network 计算机网络</span>-->
## <span style="color: blue;">This post will have a look of 3 papers regarding my master thesis topic: Platooning </span>

> Keywords: Platooning, V2V Communication, C2X Communication 

Three papers:
1. Dynamical Modeling and Distributed Control of Connected and Automated Vehicles: Challenges and Opportunities
<!--more-->

### <span style="color: blue;">First Paper</span>


#### <span style="color: blue;">Abstrac</span>

platooning 有很多优点，比如提高安全性，提高道路的承载能力，提高交通公共服务，减少油耗，具体可以想象一下大雁为什么呈“人”字型飞行

platooning 的一般需求，只需要车辆的状态信息，可以应用分布式控制框架，而不是使用中心化的计算和控制服务，为控制提供了可靠性和高效性

本文探讨的主要内容：从多智能体系统控制的角度（multi-agent consensus control），本文介绍了一个分解的框架去设计，分析，并实现一个车队系统(platoon system)

一个什么样的框架呢？一个车队系统可以分解为以下四个模块：
1. node dynamics 动态点
2. information flow network 信息流网络
3. distributed controller 分布式控制器
4. geometry formation 几何队形

对于以上的四个模块，分别介绍了几种经典的模型框架，同样介绍了四大主要的系统好坏的评判标准：
1. internal stability 系统内部稳定性
2. stability margin 稳定的车间距
3. string stability 车队的线性稳定性
4. coherence behavior 车队的行为一致性

同时介绍了分布式控制技术的基本知识，包括：
1. linear consensus control 线性一致性控制
2. distributed robust control 分布式鲁棒控制
3. distributed sliding model control 分布式滑动模型控制
4. distributed model predictive control 分布式预测模型控制

#### <span style="color: blue;">Introduction</span>

> 为啥要有`platooning`？

主要就是这东西能够提高运输效率，提高安全性，就这么简单！！

> 为啥要研究控制系统？

你有了车队，要让他跑起来，正常工作起来的话，就是需要有一个控制器，这东西可以是控制网络，或者控制算法，最好是网络，控制的目的是保证车队中的成员之间能够保持正常距离，行为一致，最重要的就是安全性

> platooning 起源及近况

- 最早是 `PATH` 项目 1980年代，初步提出了控制任务的分割，控制系统的架构，车间距控制的准则
- 最近有些啥相关项目？
    - GCDC 荷兰
    - SARTRE 欧洲
    - Energy-ITS 日本

以前的研究：
- 最早的传感器为雷达，基于雷达控制系统为以前的主要关注方向，但讲道理雷达传感的信息传递方式太有限，说不定只能前后传哈哈（瞎猜的，别信，我胡说的）
- 传感器进步了，出现了`V2V`技术，比较有名的是`dedicated short range communication DSRS`技术，丰富了网络的拓扑结构，比如两先驱，多先驱的结构
- 网络拓扑结构增加，交流的方式和挑战也同样增加了，比如：
    - 包时延，packet delay, time delay
    - 丢包，packet loss
    - 量化误差，quantization error, 将连续信号转换为离散信号时产生的误差
- 在上一点中的这种情况下，人们更多的将车队系统看成一个动态网络来，利用多智能体控制系统，从而分布式控制整个系统，实现分布式控制器

具体谁实现了什么事？
- Wang 实现了引入一个权重和有限制的一致性寻求框架，并利用离散时间的马尔可夫链研究了多种时间的网络结构对车队的动态的影响
- Bernardo 从动态网络的一致性控制角度出发，分析了车辆车队
- Zheng 介绍了两种基础的方法通过拓扑选择和控制调整来提高车队车间距的稳定性
- Gao 引入H无限控制方法来解决不确定的车辆动态和时间延迟带来的问题
- 鲁棒性分析和分布式H无限控制综合体也在无向拓扑的车队网络种被研究

> 简单来说一下`多智能体网络的一致性控制`，在它的概念里，一组CAV组成的车队其实就是动态系统中的一个一维网络，`网络中的车一般只利用相邻车传来的信息作为控制反馈`，从这一点出发，一个车队网络能够被解耦成四个相关的部分：
1. node dynamics 点动态，ND
2. information flow network， 信息流网络，IFN
3. distributed controller 分布式控制器，DC
4. formation geometry 组成几何结构，FG

> 这种解构有什么好处？

简单来说就是提供了一个统一的框架去分析，设计甚至组合车队系统，以及进一步的现实意义上的系统实现

> 这篇论文到底做了什么？

1. 对于上述4-元件框架中每个解构部分的建模方法有一个总结
2. 展开了关于四个车队表现评价标准的技术的讨论，四个车队表现评价标准：
    1. 内部稳定性，internal stability
    2. 车间距稳定性 margin stability
    3. 串稳定性 string stability
    4. 一致性行为 coherence behavior
3. 介绍了几种基础的分布式车队构建方法，包括：
    1. 线性一致性控制 linear consensus control
    2. 分布式鲁棒控制 distributed robust control
    3. 分布式滑动模型控制 distributed sliding model control
    4. 分布式模型的预测控制 distributed model predictive control


#### <span style="color: blue;">Four-Components Framework of platooning</span>

as mentioned before, the platooning modelcan be decomposed into four interrelated components: `ND, IFN, DC, FG`

我们的目标是: 使车队保持相同的速度和适当的车间距

稍微解释一下各个子部分是什么：
1. ND，点动态，描述每个有关的车辆的行为
2. IFN，信息流网络，描述车辆间如何进行信息交换的，包括信息网络的拓扑结构以及信息流的质量
3. DC，分布式控制器，通过利用相邻车辆信息实现了反馈控制
4. FG, 组成的几何结构，规定了车间距

再稍微解释一下，什么是车队系统的表现评判标准：
1. internal stability，闭环系统的特征值有严格的负实部，如下图
![系统稳定性1]({{site.baseurl}}/assets/img/系统稳定性1.jpg)
![系统稳定性2]({{site.baseurl}}/assets/img/系统稳定性2.jpg)
![系统稳定性3]({{site.baseurl}}/assets/img/系统稳定性3.jpg)
2. stability margin，边界的稳定性被定义为最不稳定的特征值实部的绝对值，他指代初始误差的收敛速度
3. string stability，车队是串稳定的是指，在当出现干扰时，干扰不会随着在车辆串中传递而扩大
4. coherence behavior，一致性行为，该指标是来评价整个系统对于外界干扰来说是不是更像一个刚体，该指标被用闭环系统的一个`确定的H2范式`来量化


##### <span style="color: red;">ND node dynamics</span>

不管是纵向的，还是横向的动态行为，都要研究，之前大部分聚焦于纵向的动态行为的研究，因为毕竟在设定当中，车队是看作在平坦地面的车辆队列，纵向的动态控制为主要的部分，少部分研究有将横向和纵向的控制一起研究
，单车模型用于描述横向的动态控制

本文主要研究纵向的车辆动态控制的建模方法

> 纵向车辆动态模型的非线性属性，一般包括，drive line, brake system, aerodynamics drag, rolling resistance 等，以下方程用于描述非线性的纵向车辆动态的建模：

![车辆动态纵向建模]({{site.baseurl}}/assets/img/车辆动态纵向建模.jpg)

$p_i(t)$ 为位置
$v_i(t)$ 为速度
$T_i(t)$ 为转矩
$C_{A,i}$ 为空气动力学阻力系数
$m_i$ 为质量
$g$ 为重力加速度
$r_{w,i}$ 为轮胎半径
$\eta_{T,i}$ 为动力传动系统的机械参数

有些研究中就直接使用这种非线性模型用于控制车队，渐近线的稳定性和串稳定性可以调整参数来保证，但详细的系统表现比较难通过spacing policy 和 communication topology 来确定

> 通常，线性模型是在确定可跟踪问题时使用的，常用的线性模型有：

1. single integrator model
2. second-order model
3. third-order model
4. single-input-single-output model



##### <span style="color: red;">IFN</span>
##### <span style="color: red;">DC</span>
##### <span style="color: red;">FG</span>
