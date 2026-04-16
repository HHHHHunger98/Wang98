---
layout: post
title: PA_Firewall_Basic
date: 2026-04-16
tags: network PAFirewall
---


<!--# <span style="color: blue;"></span>-->
## <span style="color: blue;">PA Firewall Basic</span>

This article is about the basic knowledge of the PA firewall device

<!--more-->
## <span style="color: blue;">PA firewall CLI Command</span>

> enter the configuration mode

```bash
admin@PA-820>
# enter the configure mode
admin@PA-820> configure
#Entering configuration mode
#[edit]
admin@PA-820# 
```
> Test the ping

1. 默认ping行为：如果发生ping不通的情况

```bash
ping 8.8.8.8
```

来源可能为管理口或者错误接口

正确ping命令的方式：
```bash
ping source 10.0.0.2 host 8.8.8.8
# source = 出口接口IP
```

这个问题很典型，而且**“有默认路由但 ping 不通外网”在 PA 上非常常见**。先直接说结论：

👉 **默认路由只是“指路”，不是“放行”和“能出去”**
你还缺少其他关键条件。

---

# 一、先给你快速结论（最常见原因 TOP 4）

在 Palo Alto Networks 防火墙（如 Palo Alto Networks PA-820）上：

👉 有默认路由但不能 ping 外网，通常是：

1. ❌ 没有 SNAT（最常见）
2. ❌ 安全策略没放行
3. ❌ 上游网关不回包
4. ❌ ping 命令来源接口不对

---

# 二、逐个拆解（你对照排查）

---

## 1️⃣ ❗没有 SNAT（90%就是这个）

### 现象：

* 防火墙能发包
* 但外网不回

👉 原因：
外网看到你的源 IP 是：

```
192.168.x.x（私网地址）
```

→ 直接丢弃

---

### ✔ 解决（必须有 SNAT）：

```bash
set rulebase nat rules SNAT-Internet from trust to untrust
set rulebase nat rules SNAT-Internet source any
set rulebase nat rules SNAT-Internet destination any
set rulebase nat rules SNAT-Internet source-translation dynamic-ip-and-port interface-address interface ethernet1/1
```

---

## 2️⃣ ❗安全策略没放行

👉 PA 默认是 **拒绝所有流量**

---

### ✔ 必须有策略：

```bash
set rulebase security rules Allow-Internet from trust to untrust
set rulebase security rules Allow-Internet source any
set rulebase security rules Allow-Internet destination any
set rulebase security rules Allow-Internet application any
set rulebase security rules Allow-Internet service any
set rulebase security rules Allow-Internet action allow
```

---

## 3️⃣ ❗你 ping 的方式不对（非常关键）

### 默认 ping 行为：

```bash
ping 8.8.8.8
```

👉 来源 IP 是：

* 管理口（management）
  或
* 错误接口

---

### ✔ 正确方式：

```bash
ping source 10.0.0.2 host 8.8.8.8
```

👉 必须指定：

* **source = 出口接口 IP**

---

## 4️⃣ ❗上游网关没回包

👉 你的配置：

```id="x1"
next-hop 10.0.0.1
```

但如果：

* 路由器不知道如何回到你
* 或禁止 ICMP

👉 也会失败

---

### ✔ 验证：

```bash
ping source 10.0.0.2 host 10.0.0.1
```

如果这个都不通：
👉 问题在二层/网关

---

# 三、最标准排查步骤（强烈建议照做）

---

## ✅ Step 1：检查路由

```bash
show routing route
```

确认：

* 有 `0.0.0.0/0`
* next-hop 正确

---

## ✅ Step 2：测试网关

```bash
ping source 10.0.0.2 host 10.0.0.1
```

---

## ✅ Step 3：测试外网（带 source）

```bash
ping source 10.0.0.2 host 8.8.8.8
```

---

## ✅ Step 4：看 session（关键）

```bash
show session all filter destination 8.8.8.8
```

看有没有：

* NAT 命中
* session 建立

---

## ✅ Step 5：看 NAT 是否命中

```bash
show running nat-policy
```

---

# 四、一个非常重要的理解

👉 在 PA 上，流量出去必须经过：

```
路由 ✔
+ 安全策略 ✔
+ NAT ✔
= 才能真正上网
```

少一个都不行

---

# 五、最常见真实原因排序（经验）

1. 🥇 没做 SNAT
2. 🥈 ping 没指定 source
3. 🥉 策略没放行
4. 网关问题

---

# 六、一句话总结

👉 **默认路由只负责“往哪走”，
SNAT 才负责“能不能回来”**

---

# 七、我可以帮你快速定位

你把下面3个输出发我👇

```bash
show routing route
show running nat-policy
show session all filter destination 8.8.8.8
```

