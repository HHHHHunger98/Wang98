---
layout: post
title: Cisco Commonly Used Commands
date: 2025-07-24
tags: network
---

<!--# <span style="color: blue;"></span>-->
## <span style="color: blue;">Cisco Commonly Used Commands</span>

This article will introduce the commonly used cmd for cisco devices.

<!--more-->
## <span style="color: blue;">Basic CMDs User Views</span>

- `enable`: 进入特权模式 access privileged `EXEC` mode
- `disable`: 退出特权模式 回到用户模式 exit privileged mode to user mode 
- `exit/logout`: exit current session 退出当前模式，登录对话
- `show version`: 查看设备型号，系统镜像，启动时间
- `show running-config` 查看当前运行的配置 
- `show startup-config` 查看开机启动的配置 
- `show history` 查看历史命令记录 
- `terminal length 0` 取消分页，适合查看长页

```sh
ping [ip]   #ICMP测试网络连通性
traceroute [ip] #路由路径跟踪
```

## <span style="color: blue;">Interface CMDs</span>

- `show ip interface brief` 快速查看所有端口状态和IP
- `show interfaces` 查看所有接口的详细信息
- `show interfaces GigabitEthernet 0/0/1` 查看某个接口的详细信息
- `show interfaces status` 查看接口状态，vlan，速率等
- `interface GigabitEthernet 0/0/1` 进入接口的配置模式
- `no shutdown` 启用接口
- `shutdown` 关闭接口
