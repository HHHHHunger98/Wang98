---
layout: post
title: PA Firewall IPSec
date: 2026-04-16
tags: network PAFirewall
---

<!--# <span style="color: blue;"></span>-->
## <span style="color: blue;">PA Firewall IPSec Setup</span>

This article is about the configuration of IPSec Tunnel on the PA firewall device

**Palo Alto（PA）防火墙配置 IPSec VPN** 的完整思路: 从**需求分析 → 配置步骤 → 验证排错**一步一步来。

<!--more-->
### <span style="color: blue;">Basic Requirement</span>

#### 1. 基本参数

* 我方公网IP：`84.4.96.19`
* 对方公网IP：`1.2.3.4`
* 我方内网：`192.168.5.0/24`
* 对方内网：`10.0.0.0/8`

👉 目标：
建立 **Site-to-Site IPSec VPN**，让两个内网可以互通。

---

#### 2. VPN结构（逻辑）

```
我方内网 (192.168.5.0/24)
        │
   [PA防火墙]
        │ 公网84.4.96.19
======== IPSec VPN ========
        │ 公网1.2.3.4
   [对方设备]
        │
对方内网 (10.0.0.0/8)
```

---

#### 3. 配置要点总结

PA IPSec VPN需要4大核心组件：

1. IKE Gateway（第一阶段）
2. IPSec Tunnel（第二阶段）
3. Tunnel Interface（虚拟接口）
4. Security Policy（放行流量）

---

### 二、配置步骤（重点）

#### Step 1：创建 Tunnel Interface

路径：

```
Network → Interfaces → Tunnel
```

创建：

* Name：`tunnel.1`
* Virtual Router：默认（或你自己的VR）
* Security Zone：建议新建 `vpn-zone`

👉 注意：

* Tunnel接口必须加入一个 Zone，否则策略无法匹配

---

#### Step 2：配置 IKE Gateway（Phase 1）

路径：

```
Network → IKE Gateways
```

##### 基本参数

| 项目        | 配置                  |
| --------- | ------------------- |
| Name      | ike-gw-peer         |
| Version   | IKEv1（或IKEv2，需双方一致） |
| Interface | 外网接口（如 ethernet1/1） |
| Local IP  | 84.4.96.19        |
| Peer IP   | 1.2.3.4             |

---

##### 认证方式

一般用预共享密钥（PSK）：

```
Authentication → Pre-shared Key
```

示例：

```
Key: MyStrongPSK123!
```

👉 必须和对端完全一致

---

##### Phase 1 加密参数（要双方一致）

建议：

* Encryption：AES-256
* Authentication：SHA256
* DH Group：Group14
* Lifetime：28800

---

#### Step 3：配置 IPSec Tunnel（Phase 2）

路径：

```
Network → IPSec Tunnels
```

##### 基本配置

| 项目               | 配置           |
| ---------------- | ------------ |
| Name             | ipsec-tunnel |
| Tunnel Interface | tunnel.1     |
| Type             | Auto Key     |
| IKE Gateway      | ike-gw-peer  |

---

##### Phase 2 加密参数

* Encryption：AES-256
* Authentication：SHA256
* PFS：Group14（建议开启）
* Lifetime：3600

---

#### Step 4：配置 Proxy ID（关键！）

路径：

```
IPSec Tunnel → Proxy IDs
```

👉 相当于定义“允许通过VPN的网段”

新增：

| 类型       | 配置             |
| -------- | -------------- |
| Local    | 192.168.5.0/24 |
| Remote   | 10.0.0.0/8     |
| Protocol | Any            |

👉 如果对方设备是策略型VPN，这一步必须配置

---

#### Step 5：配置静态路由

路径：

```
Network → Virtual Routers → default → Static Routes
```

新增：

| 项目          | 配置         |
| ----------- | ---------- |
| Destination | 10.0.0.0/8 |
| Interface   | tunnel.1   |
| Next Hop    | None       |

👉 告诉防火墙：访问对方网段走VPN

---

## Step 6：配置安全策略（Security Policy）

路径：

```
Policies → Security
```

---

##### 策略1：内网 → VPN

| 项目               | 配置             |
| ---------------- | -------------- |
| Source Zone      | trust          |
| Destination Zone | vpn-zone       |
| Source           | 192.168.5.0/24 |
| Destination      | 10.0.0.0/8     |
| Application      | any            |
| Service          | any            |
| Action           | allow          |

---

##### 策略2：VPN → 内网

| 项目               | 配置             |
| ---------------- | -------------- |
| Source Zone      | vpn-zone       |
| Destination Zone | trust          |
| Source           | 10.0.0.0/8     |
| Destination      | 192.168.5.0/24 |
| Action           | allow          |

---

#### Step 7：NAT策略（容易踩坑）

👉 重点：VPN流量通常**不做NAT**

路径：

```
Policies → NAT
```

新增一条 **No-NAT规则**：

| 项目               | 配置             |
| ---------------- | -------------- |
| Source Zone      | trust          |
| Destination Zone | vpn-zone       |
| Source           | 192.168.5.0/24 |
| Destination      | 10.0.0.0/8     |
| Translation      | None           |

👉 必须放在普通上网NAT规则之上！

---

### 三、提交配置

```
Commit
```

---

### 四、验证VPN状态

#### 1. 查看IKE状态

```
Network → IPSec Tunnels → Tunnel Info
```

或者CLI：

```bash
show vpn ike-sa
```

---

#### 2. 查看IPSec状态

```
show vpn ipsec-sa
```

---

#### 3. 抓包排查

```
Monitor → Packet Capture
```

或CLI：

```
debug ike global on
```

---

### 五、常见问题排查

#### ❌ Phase1不通

原因：

* PSK不一致
* 加密算法不一致
* 公网IP错误

---

#### ❌ Phase2不通

原因：

* Proxy ID不匹配
* 子网写错

---

#### ❌ 能建隧道但不通

重点检查：

1. ❗ 安全策略（最常见）
2. ❗ NAT是否关闭
3. ❗ 路由是否正确
4. ❗ Zone是否正确

---

#### ❌ 单向通信

可能：

* 对端没有回程路由
* 对端策略没放行

---

### 六、总结（核心记住）

配置IPSec VPN = 5件事：

1. IKE Gateway（建立连接）
2. IPSec Tunnel（加密数据）
3. Tunnel Interface（转发流量）
4. 路由（指向Tunnel）
5. 安全策略 + No-NAT

---

