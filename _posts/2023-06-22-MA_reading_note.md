---
layout: post
usemathjax: true
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
    
    单积分模型，最简单的模型就是通过汽车的速度信息作为控制输入，仅仅只关心汽车的位置，就是一个通过速度得到位置的公式，位置的导数为速度，速度的积分为位置：

    ![单积分模型]({{site.baseurl}}/assets/img/单积分模型.jpg)
    - 优点：方便控制器设计，模型简单，可以将structured optimal control 问题转化为凸优化问题
    - 缺点：这种凸优化问题有一定的局限性，而且这种只考虑速度的单积分模型忽略了很多其他的车辆动态信息，因此这种模型无法复现车队的串不稳定性
2. second-order model

    二阶模型，或者说双积分模型，对于复现车队的串不稳定性，有一点可行的改进是使用双积分模型，即将车辆看作一个点质量，考虑加速度带来的影响，即速度的导数是加速度，位置的导数是速度：

    ![双积分模型]({{site.baseurl}}/assets/img/双积分模型.jpg)
    
    - 优点：很多更详细的优化基本是建立在双积分模型上的，比如`去中心优化控制`，`稳定边界分析 stability margin analysis`，`行为一致性 coherence behavior`
    - 缺点：相对来说还是忽略了很多车辆动态参数，比如`传动系统的内部延迟 delay in powertrain dynamics`等

3. third-order model

    三阶模型，为了解决车辆动态信息缺少导致的建模问题，引入了一个状态来近似`输入/输出的传动系统的动态参数 input/output behaviours of powertrain dynamics`，大部分情况都是利用**引擎的转矩或刹车的转矩**来近似传动系统的动态参数

    大多使用的技术是：`线性化反馈`或者`低层控制`

    ![三阶模型]({{site.baseurl}}/assets/img/三阶模型.jpg)
   
   $u_i(t)$ 是`desired acceleration，期望加速度`，$\tau_i$是时间常数，用来近似传动系统动态参数，简单来说就是将加速度的变化也考虑进去了

4. single-input-single-output model

   单输入单输出模型，这是一种转移函数模型，是利用拉普拉斯变化，在频域中分析串稳定性`string stability`：

    ![三阶频域模型]({{site.baseurl}}/assets/img/三阶频域模型.jpg)

    $p_i(s), v_i(s), a_i(s)$ 分别是位置，速度和加速度的拉普拉斯变换，$u_i(s)$ 是控制输入，$H_i(s)$ 是线性单输入单输出的转移函数

##### <span style="color: red;">IFN</span>

对于车队集体的行为控制是基于车辆之间对于彼此状态的互知，这种互知是如何实现的呢？答案是**汽车内部的传感器和汽车间的交流方式** 

传感器得到的数据会作为汽车的控制输入，保证车队的安全及效率，因此，传感技术和汽车间的交流技术都是很重要的

主要在一下几方面：
1. 信息流的拓扑结构
    
    主要研究的是信息能够如何在汽车间传递，谁传给谁？即交流网络的连接

    以前大多是基于雷达感应的交流技术，因此拓扑结构很简单，现在，随着新的技术的出现，比如 dedicated short range communication(DSRC) 和 5G 技术的赋能，出现了越来越多不同的交流拓扑结构，不单单只是能和直接相邻的车辆进行交流，所以，对于拓扑网络的选择，需要考虑实际需求

    对车队的实际表现有很重要的影响：比如`string stability`,`stability margin`,`coherence behavior`等指标
    
    常见的拓扑结构有一下几种：
    1. predecessor following (PF)
    2. bidirectional (BD)
    3. predecessor following leader (PFD)

    ![拓扑网络类型]({{site.baseurl}}/assets/img/拓扑网络类型.jpg)

2. 信息交流质量，Quality of information exchange

    对于给定的拓扑网络，车辆间的感知和交流也十分重要，比如，雷达的精度影响车队控制的表现和鲁棒性，无线交流的质量也关系到车队控制的安全性和表现，因此**无线交流的质量**也需要考虑

    关于无线交流的质量控制有啥办法：
    1. scheduling
    2. power control
    3. rate control 等

    > 因此，无线交流和车队的整体需要一个大的控制系统
    
    存在那些解决方案，又有什么问题：
    1. IEEE 802.11p 标准，wireless access in vehicular environments(WAVE)，是一套基于无线通信的通讯协定，比如，车载通讯专用短距离通讯DSRC就是基于该标准，**缺点就是不支持关于同频率冲突的预测控制，predictable control，当出现同频率冲突时无法保证预测控制的质量**
    2. 但最近的无线技术的突破性发展，无线的交流质量能够在有预测控制的情况下保持，因此无线的车辆间交流网络和车队控制能够兼得

3. 基于图的拓扑建模，Graph-based topological modeling
    
    讲道理，车间的信息流向图可以用有向图表示，比如`$G_N=\{ V_N,E_N,A_N \}$`，分别指代`点$V_N=\{ 1,2,...,N \}$`，`边$E_N \subseteq V_N \times V_N$`和`连接矩阵$A_N=[a_{ij}] \in R^{N \times N}$` ，简单的图论里面的东西，同时假设没有自己到自己的边，即邻接矩阵对角线为0
    
    相应的有向图$G_N$的拉普拉斯矩阵则是`$L=D_N-A_N，其中D_N=diag\{ deg_1,deg_2,...,deg_n \}$`，点的入度为`$deg_i=\sum\nolimits_{j=1}^Na_{ij}$`

    只有这有向图还不够，我们要研究从leader到followers的信息流，所以还需其他信息，因此，我们定义一个增强图`$G_{N+1}$`，多一个点`0`和相应的边，即为一个`N+1xN+1`的矩阵，边上存储的信息是每辆车和leader的连接性，`$P= diag\{ p_1,p_2,...,p_N \}$`，`$p_i=1$`指的是从0到i有一条边，否则`$p_i=0$`

    文章中有一个例子，PF和BD的拉普拉斯矩阵和边角矩阵pinning matrix
    
    ![拓扑建模例子]({{site.baseurl}}/assets/img/拓扑建模例子.jpg)

> 有一个关键的问题：为什么要这么建模呢？

拉普拉斯矩阵`L`和边角矩阵`P`其实概括了信息流网络的拓扑结构，在最初的框架当中,`L+P`非常重要，他揭示了闭环回路的动态参数，他的特征值可以用于分析`线性车队动态的边界稳定性 stability margin for platoons with linear node dynamics`，**如果增强图`$G_{N+1}$`有一个从leader出发的生成树的话，则`L+P`的所有特征值在开的左半平面**

并且，`L+P`的最小特征值在无向的拓扑结构中与和leader有连接的车辆数有关

当然，进一步基于图理论的信息流质量建模可以通过给每条边引入权重来扩展，本文没讲


##### <span style="color: red;">DC</span>

DC的部分，其实就是反馈控制，分布式控制器，distributed controller，一般是使用“邻居”的信息来进行控制，邻居是：

![DC信息来源]({{site.baseurl}}/assets/img/DC信息来源.jpg)


对于车队的全局协作控制，非结构化的DC一般基于完全图，所有车辆都互相连接

一般的DC是一个理论分析的综合结果和硬件实施的便捷性的线性组合，简单说就是考虑到两方面:
- 最后的效果
- 硬件的可实施性

一般的控制器可以被抽象成以下公式：

![linearcontroller]({{site.baseurl}}/assets/img/linearcontroller.jpg)

`$k_{ij,#}(# = p, v, a)$`是本地控制器增益，`$r_{ii}$`是达到自身状态的时延，`$r_{ij}$`是从信道接收到node j状态的时延，简单看这个公式就是，所有相连邻居的位置，速度，加速度的随时延的增益的和就是控制器的输出

> 线性控制器的变种和优化
很多其他的文章中都有这个线性控制器的变种公式，在这个公式的描述中，使用线性控制器的车队系统的内部稳定性有很大一部分取决于信息流网络的拓扑结构，对于某一类型的`IFT`有特定的线性控制器方程

对于串稳定性 string stability 也有人给出过相应的控制增益需求

具体的优化方法不是数学性的就是分析型的优化方法，也有人使用滑动模型控制`SMC`来满足串稳定性需求，一般使用后验控制调谐posterior controller tuning

> 以上各种方法的缺点

有两个主要的缺点：
1. 不能精确的指出线性稳定性
2. 不能应对状态或者控制的限制

对于以上缺点，有人用`$H_\infty$`控制器综合了各种需求，有人用预测模型控制的方式解决上述问题，通过预测车队的动态参数来精准的控制执行器和状态限制

##### <span style="color: red;">FG</span>

车队形状的几何学，简单来说，车队控制的目的就是控制排头兵的车速，控制车队的几何结构

![车队几何结构]({{site.baseurl}}/assets/img/车队几何结构.jpg)

就是速度保持想要的速度，位置保持想要的位置

> 主要有三个车队形状几何学的策略：

1. 距离常量策略 constant distance
    车间距固定，可增加道路的容量
2. 进步常量策略 constant time headway
    距离随着速度的变化而变化，与司机的行为相符，限制了道路容量
3. 非线性距离策略 nonlinear distance
    车间距由非线性方程决定，好的方程可以提高交通流稳定性和道路容量

#### <span style="color: blue;">Performance of a Platoon of CAVs</span>

主要就两个：
1. 稳定性，stability
2. 鲁棒性，robustness

四个重要指标评价车队控制：
1. internal stability
2. stability margin
3. string stability
4. coherence behavior

前两个主要关注的是车队的稳定性，后两个关注车队的鲁棒性，即有干扰情况下的表现

1. 内部稳定性 internal stability
    
    不管拓扑结构是什么，车队内部的稳定性都必须保证，大致上有两种方法：
    1. 全局 global
        
        第一种就是简单的把车队当成一个整体，并设计一个中心的控制器集中控制，这种做法使得信息流向拓扑IFT在设计过程中没那么重要了，比如，设计一个线性矩阵不定式，linear matrix inequality,来保证全局的动态参数稳定

        缺点是什么：计算复杂度随着车队大小的提升而升高，车越多，计算越慢
    2. 本地 local
        
        因为集中计算的缺点，很多人将控制器解耦拆分，设计成子系统，比如，将拓扑结构看作是瀑布状系统的控制，从而使车辆只用计算较少的相邻车辆信息，就可以达到不错的效果和稳定性

        此外，设计解耦的控制器时可以使用以下方法：
        1. 包含定理：设计叠加的控制器时使用，无法在BD拓扑结构上应用，因为错误能双向传递
        2. 偏微分方程：用来近似车队的动态参数，对于BD拓扑可以使用，可以避免高维度的动态分析，基于Lyapunov方法的energy function 可以用来在纵向和横向上保证车队的稳定性
        3. Lasalle's不变量原理可以被用来证明时不变车队系统的渐近线稳定性

**需要补充知识**

2. stability margin

    边界稳定性，用来评价车队的空间误差的收敛速度的指标，大部分研究都是基于constant distance原则，边界稳定性可以被抽象成一个关于`platoon size, node dynamics, IFN, DC structure`的函数

    将ND点动态参数看成一个点质量，边界稳定性在对称的双向控制系统中以O(1/N^2)趋向于0，N为车队内车的数量

    同时可以证明，在引入小数量的失调mistuning 后，边界稳定性的收敛行为可以被提升到O(1/N)

    结果可以基于线性三次偏微分的动态方程（该方程包含了传动系统的延迟），在D维的IFT中，边界稳定性的收敛状况可以被概括为O(1/N^(2/D))

    以上都是讨论的对称控制，在非对称控制中，边界稳定性可以远离0,即不受车队大小影响，也可以通过选择适当的拓扑结构和控制调整来提高边界稳定性

3. String Stability
    
    串稳定性的实现很大程度上基于车队的FG和IFN，即几何结构和信息网络结构

    在同质的BD车队中，线性统一控制器有着局限性，无法做到串稳定

    在非同质的车队中，采用非零时间进尺的策略，在前向传播范围和较小的时间进尺也无法改变串不稳定的现状

    有相应的一些方法提高串稳定性：

    1. 减少几何结构的刚体性，比如引入足够的时间进尺，或者使用非线性策略
    2. 采用非统一的控制器
    3. 采用更复杂的IFT

    最近有采用比较复杂的控制器来保证串稳定性，比如滑动模型控制，模型预测控制，H无限控制等，以上基本采用Constant time headway 策略，或者使用leader的信息

    当出现多个车受到影响的情况，车队的串稳定性需要相应的车间隙策略来保证

4. Coherence Behavior 行为一致性

    行为一致性是一个基于闭环系统的H2范数的标量矩阵，来表征一个车队系统受到外界干扰以后的鲁棒性

    有人研究了一致性行为的上边界的渐近线的缩放性，得出的结论是IFN比DC更重要

    有人通过将行为一致性抽象为成本函数（基于增广的拉格朗日乘数法）来优化本地控制的增益


#### <span style="color: blue;">Controller Design of a Platoon of CAVs</span>

> 分布式控制器的设计

主要介绍了4种：
1. 线性一致性控制
2. 鲁棒控制
3. 分布式滑动模型控制
4. 分布式模型预测控制

前两个是基于线性模型的，后两个可以用在非线性车辆上的

主要的思想都是将车队系统的动态参数基于L+P的特征值解构，解构后，许多方法都可以通过解线性矩阵不等式被解决

1. linear consensus control

    优点，便于分析系统，便于后续的硬件实施
    
    简单介绍一下方法：对于车队点动态的描述，利用三阶空间状态方程，即上面提到方程，同质车队，CD策略

    简而言之就是将IFT和ND解构，将车队动态抽象成下图所示内容：

    ![线性一致性控制器设计]({{site.baseurl}}/assets/img/线性一致性控制器设计.jpg)

    ![稳定性充要条件]({{site.baseurl}}/assets/img/稳定性充要条件.jpg)

    稳定性充要条件如上图所示

    > 为什么说以上公式15将车队解构了？`A和B`代表点动态，`L+P`表征IFT，`X`中内含FG的状态，`k`表示分布式控制器的参数

    虽然车队的动态被解构成四个主要的模块，但是很难得出车队的表现和四个模块的关系，基本上都是关注于特定的场景

2. Distributed Robust Control 分布式鲁棒控制

    为了避免模型的不匹配，统一使用精确的一致的输入来控制点动态，但是对于非同态的车队就不能够统一输入了，容易出现模型不匹配

    对于不同质的车队，有人利用H无限的控制方法，将车队整个视为一个整体，考虑不确定性和统一的时延，解决了这个非同质车队的控制问题，但是，缺点是当车队大小或者结构发生改变的时候，需要重新建模

    还有人借鉴解构的思想，利用解耦来解决不同质车队的点动态问题，基本思想是利用`L+P`矩阵的特征值，动态平衡车队表现和鲁棒性，如下图所示，信息流带来的高度耦合可以转化为多个不确定因子`delta_i`的线性变换

    简单来说基于H无限的鲁棒模型控制就是将不确定性抽象出来为单独的参数，在频域去分析系统，因为在频域中传递函数可以线性组合，方便解构，此为H无限控制设计思路

![线性变换频域解耦]({{site.baseurl}}/assets/img/线性变换频域解耦.jpg)

3. 滑动模型控制 distributed sliding model control

    简称`SMC`，是一个用来解决车队系统中非线性的动态参数和执行者的饱和问题

    接下来介绍的是在同质车队中，线性动态参数和无向IFT的SMC框架

    > L+P是正定的，如果存在一颗无向生成树在IFT中

    设计SMC分为两步：
    1. 拓扑的互动表面设计
    2. 拓扑可达规则设计

    讲道理，这东西需要太多数学的内容基础，我暂时不需要研究太深，只要了解就好，知道是个什么回事就行，需要的时候再进行补充

4. 分布式模型预测控制 distributed model predictive control

    预测控制是一个基于优化的控制技术，方法就是预测未来的一个行为，通过预测来进行相应的控制

    利用MPC 技术，就是在解决一个有限时域最优化控制问题。具体是什么，我也不清楚，需要的时候深入了解就行

    他能够解决非线性和有约束的控制问题，这种控制方法被运用到了很多领域，比如碰撞检测，比如车辆稳定性

    大多数的MPC 主要是集中式的计算模式，收集所有信息之后在计算预测结果，在车队系统中就有些不一样，毕竟局限性以及时效性在那摆着，集中式的计算模式不适合，需要更高效的模型，因此，大部分研究都是考虑相邻车辆的信息来进行预测分析，比如ACC, adaptive cruise control:q

    
![equation]({{site.baseurl}}/assets/img/equation.jpg)
