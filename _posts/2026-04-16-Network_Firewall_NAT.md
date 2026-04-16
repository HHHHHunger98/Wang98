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

### Scenarios

```
公网IP（ethernet1/1）：1.2.3.4
内网服务器：192.168.2.10（Web服务器）

内网网段：192.168.2.0/24
```

> 需求：

1. 外网访问 1.2.3.4:80 → 转发到 192.168.2.10（DNAT）
2. 内网访问互联网 → 使用公网 IP 上网（SNAT）

> DNAT: 把公网的访问转发到内网服务器


