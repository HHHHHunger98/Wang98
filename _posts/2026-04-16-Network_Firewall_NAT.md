---
layout: post
title: Network_Firewall_NAT
date: 2026-04-16
tags: network
---

<!--# <span style="color: blue;"></span>-->
## <span style="color: blue;">Firewall NAT Policy</span>

This article is about the NAT policy on firewall device

<!--more-->
## <span style="color: blue;">Type of NAT</span>

有两种不同的NAT策略，分别是：
1. DNAT: Destination NAT 目标地址转换
2. SNAT: Source NAT 源地址转换

### <span style="color: blue;">DNAT</span>

> 目的：修改“目的 IP 地址”

外网用户 → 公网IP(1.2.3.4)
↓ DNAT
内网服务器(192.168.1.10)

> 作用：

当数据包进入网络时，把目标地址改成另一个地址
常用于把公网请求转发到内网服务器（端口映射）

> 典型场景：

你有一台内网服务器（比如 192.168.1.10）
外网用户访问你的公网 IP（比如 1.2.3.4:80）
路由器通过 DNAT 把请求转发到内网服务器

### <span style="color: blue;">SNAT</span>

> 目的：修改“源 IP 地址”

内网主机(192.168.1.10)
↓ SNAT
公网IP(1.2.3.4) → 互联网

> 作用：

当数据包离开网络时，把源地址改成另一个地址
常用于内网设备访问互联网（共享公网 IP）

> 典型场景：

内网主机（192.168.1.10）访问互联网
路由器把源 IP 改成公网 IP（1.2.3.4）
外网服务器只看到公网 IP

## <span style="color: blue;">PA Firewall Setting up DNAT and SNAT</span>

### <span style="color: blue;">Scenarios</span>

```
公网IP（ethernet1/1）：1.2.3.4
内网服务器：192.168.2.10（Web服务器）

内网网段：192.168.2.0/24
```

> 需求：

1. 外网访问 1.2.3.4:80 → 转发到 192.168.2.10（DNAT）
2. 内网访问互联网 → 使用公网 IP 上网（SNAT）

> DNAT: 把公网的访问转发到内网服务器

1. 把公网的访问转发到内网服务器 DNAT
```bash
set rulebase nat rules DNAT-Web from untrust to untrust
set rulebase nat rules DNAT-Web source any
set rulebase nat rules DNAT-Web destination 1.2.3.4
set rulebase nat rules DNAT-Web service service-http

set rulebase nat rules DNAT-Web to-interface ethernet1/1

set rulebase nat rules DNAT-Web destination-translation translated-address 192.168.2.10
set rulebase nat rules DNAT-Web destination-translation translated-port 80
```

2. 对应的防火墙安全策略 允许外网流量访问转换后的地址
```bash
set rulebase security rules Allow-Web from untrust to trust
set rulebase security rules Allow-Web source any
set rulebase security rules Allow-Web destination 192.168.2.10
set rulebase security rules Allow-Web application web-browsing
set rulebase security rules Allow-Web service service-http
set rulebase security rules Allow-Web action allow
```

**至此实现了外网流量访问内部地址的功能**

> SNAT：源地址转换，让内部主机共享IP上网

1. 创建SNAT规则
```bash
set rulebase nat rules SNAT-Internet from trust to untrust
set rulebase nat rules SNAT-Internet source 192.168.2.0/24
set rulebase nat rules SNAT-Internet destination any

set rulebase nat rules SNAT-Internet source-translation dynamic-ip-and-port interface-address interface ethernet1/1
```

2. 安全策略放行内部流量去外网
```bash
set rulebase security rules Allow-Internet from trust to untrust
set rulebase security rules Allow-Internet source 192.168.2.0/24
set rulebase security rules Allow-Internet destination any
set rulebase security rules Allow-Internet application any
set rulebase security rules Allow-Internet service any
set rulebase security rules Allow-Internet action allow
```

> 提交配置

PA防火墙提交配置
```bash
commit
```

## <span style="color: blue;">DNAT和SNAT本质</span>

> DNAT是把外部流量引进来，外部访问的是公网地址，公网地址到私网地址的转换，方便外网访问内网应用，必须配置安全策略

> SNAT是把内部流量放出去，内部地址访问外部公网，私网地址到公网地址的转换，方便内部访问外部网站，常用端口复用 PAT


## <span style="color: blue;">检查命令</span>

```bash
show running nat-policy
show session all filter destination 192.168.2.10
show session all filter source 192.168.2.10
```
