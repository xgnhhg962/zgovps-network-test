# ZgoVPS 测试IP 完整指南：洛杉矶/大阪/香港/德国机房怎么测速？Looking Glass 怎么用？三网回程路由怎么看？附全机房套餐价格对比与延迟实测参考（含最新优惠码）

最近在折腾 VPS，手头好几台机器延迟忽高忽低，心里没底。买之前光看商家宣传页上写"三网优化""CMIN2 高端线路"这些词，其实没用——真正能告诉你"这台机器到底快不快"的，是 **测试 IP**。正好最近在研究 ZgoCloud（也就是老玩家口中的 ZgoVPS），它的机房横跨洛杉矶、大阪、香港、德国四个地方，线路还分国际、9929、CMIN2、双 ISP 好几种，不先把测试 IP 跑一遍根本不敢下单。这篇就把 ZgoVPS 测试 IP 怎么找、怎么用、以及各机房套餐怎么选，一次性说清楚。

## 一、为什么 ZgoVPS 测试 IP 这么重要

ZgoVPS 这家有点意思。它 2021 年在美国特拉华州注册，自己运营着 AS197767 这个网络自治域，是 ARIN 和 RIPE 的成员——说白了不是那种租个柜子就开卖的"二道贩子"，而是真的有自己的网络资源。也正因为这样，它的线路分得特别细：同样是洛杉矶机房，光 AMD EPYC 平台就分了 **国际线路（Global）**、**9929&CMIN2 优化线路**、**GIA+9929+CMIN2 三网高端优化线路**、**双 ISP 线路**四种，价格从 $12.9/年一直拉到 $78/季，差了好几倍。

问题是，线路名字再好听，落到你家的宽带上是另一回事。同样是"CMIN2 优化"，有人家移动宽带 ping 下来 150ms 稳如老狗，有人家电信却绕道走了 9929 反而更快。所以 **ZgoVPS 测试 IP 就是你下单前唯一的"试衣间"**——先用测试 IP ping 一下、traceroute 看看回程路由走哪条线、再下载个测速文件跑跑带宽，心里才有谱。

而且 ZgoVPS 官方有个挺实在的规矩：**国际线路、IIJ 线路的套餐不支持因为"对中国没优化"而退款**，特价套餐更是直接不退款。这就更说明下单前测一遍有多重要了。

## 二、ZgoVPS 测试 IP 全机房汇总（含 Looking Glass 地址）

ZgoVPS 给每个机房都配了 Looking Glass（网络诊断面板），里面能 ping、能 traceroute、能下载测速文件。我把四个主要机房的测试 IP 和 Looking Glass 地址整理在下面。

**洛杉矶机房（主力，线路最多）**

- 测试 IPv4：`207.60.50.4`
- Looking Glass：`https://lg.la.us.zgovps.com`
- 主机名：`la.us.zgovps.com`

洛杉矶是 ZgoVPS 的"大本营"，跟 Equinix 机房合作，上游接了 xTom、KuRun Cloud、China Unicom、cogent、Arelion 这些。回程方面，电信和联通强制走联通 AS10099+AS9929，移动走自己的 CMIN2 高端线路。这个测试 IP 是国际线路段的，如果你要看 CMIN2 优化线路的实际表现，下单后用分配到的 IP 再测会更准。

**大阪机房（日本，分 EPYC 和 Ryzen 两个 Looking Glass）**

- 大阪 Ryzen 测试 IPv4：`195.245.229.134`
- 大阪 Ryzen Looking Glass：`https://lg.osaka.ryzen.jp.zgovps.com`
- 大阪 EPYC 测试 IPv4：`195.245.229.3`（EPYC Genoa 系列，IIJ 线路）

大阪机房在 Equinix Osaka，上游是 NTT 和 IIJ。注意大阪的 EPYC Genoa 系列（AMD EPYC 9354P 那批）走的是 **IIJ 线路，对中国大陆不做优化**——这点在官网反复强调，下单前一定 ping 一下，别想着日本就一定快。如果你是面向日本本土或东南亚的业务，IIJ 反而是更"正"的选择。

**法尔肯施泰因机房（德国）**

- 测试 IPv4：`23.165.200.2`
- Looking Glass：`https://lg.falkenstein.de.zgovps.com`

德国机房用的是 Intel Xeon Gold 5412U + DDR5 ECC，1Gbps 大带宽，走的是国际 BGP，对中国同样不优化。实测国内 ping 延迟大概在 217ms 上下，欧洲本地（法兰克福）延迟能压到 6ms 左右。适合做欧洲外贸站、或者给欧洲用户服务的项目，拿来回国建站就别想了。

**香港机房**

香港机房走 BGP 三网直连，100Mbps 带宽，对中国大陆优化。可惜的是搜索里没翻到一个稳定的香港公开测试 IP——ZgoVPS 香港的 Looking Glass 似乎没有像其他三个机房那样公开挂在显眼位置。这种情况下，建议你直接 👉 [到 ZgoCloud 产品页](https://bit.ly/zgovps) 找香港 AMD VPS 的详情页，或者开个工单问客服要香港测试 IP，他们响应挺快的。

> 小提醒：测试 IP 是会变的，机房有时会调整 IP 段。本文给的是目前能查到的公开测试 IP，下单前最好再到 👉 [ZgoCloud 官方产品页](https://bit.ly/zgovps) 核对一下最新的 Looking Glass 地址。

## 三、怎么用 ZgoVPS 的 Looking Glass 做测速

很多人搜"ZgoVPS 测试 IP"，其实想要的是"怎么测"。我把流程拆开讲，照着做就行。

**第一步：ping 测试延迟**

打开你电脑的终端（Windows 是 cmd 或 PowerShell，Mac/Linux 是 Terminal），直接 ping：


ping 207.60.50.4


看三个数：平均延迟（ms）、丢包率、抖动（jitter，ping 时间忽高忽低的程度）。洛杉矶优化线路到国内一般 150-180ms 算正常，香港 30-50ms，大阪 50-80ms，德国 200ms+。如果丢包率超过 5%，或者延迟抖动很大（比如一会儿 150ms 一会儿 300ms），说明这条线路到你的宽带不太稳。

**第二步：traceroute 看回程路由**

光看 ping 不够，得看数据包"怎么回来的"。Windows 用 `tracert`，Mac/Linux 用 `traceroute`：


tracert 207.60.50.4


重点看最后几跳——也就是回程路由。ZgoVPS 洛杉矶优化线路的回程，电信和联通应该走 AS9929（联通的高端线路），移动走 CMIN2。如果你在 traceroute 里看到 `9929` 或者 `CMIN2` 字样，说明走的是优化线路；如果一路都是 `cogent`、`telia` 这种国际运营商，那大概率是国际线路没优化，延迟会高不少。

**第三步：下载测速文件看真实带宽**

打开各机房的 Looking Glass 网页（比如 `https://lg.la.us.zgovps.com`），里面通常有 100MB、1GB 这种测速文件下载链接。在你本地用浏览器或 `wget` 下载，看实际下载速度能跑多少。注意这是"机房到你"的单线程下载，真实 VPS 上的多线程速度通常更好。

> 一个实测小技巧： traceroute 看回程的时候，如果发现去程和回程走的不是同一条线（比如去程走 CN2、回程走 9929），这叫"非对称路由"，ZgoVPS 洛杉矶优化线路就是这种设计——去程为了保 IP 属性走国际，回程为了速度走 9929/CMIN2。这是正常的，不是 bug。

## 四、ZgoVPS 全机房套餐配置与价格对比

测完 IP 心里有数了，接下来看套餐。ZgoVPS 套餐多得有点眼花，我按机房分组列，每组一张表，每张表都覆盖该机房官网展示的全部套餐。价格我用季付为主（因为季付是 ZgoVPS 最常见的入门周期），年付会更划算，下面会单独说优惠码。

### 4.1 洛杉矶国际线路 AMD EPYC VPS（Global，不优化中国）

最便宜的入门系列，AMD EPYC 7002 + 1Gbps 大带宽，适合海外业务、不指望回国速度的场景。

| 套餐 | CPU | 内存 | NVMe | 月流量 | 带宽 | 价格（季付） | 购买 |
|---|---|---|---|---|---|---|---|
| Starter | 1核 EPYC 7002 | 1GB DDR4 | 20G | 2TB | 1Gbps | $8/季（年付$28） | [购买](https://clients.zgovps.com/index.php?/cart/los-angeles-global-vps/&affid=1247) |
| Standard | 2核 EPYC 7002 | 2GB DDR4 | 40G | 4TB | 1Gbps | $12/季（年付$40） | [购买](https://clients.zgovps.com/index.php?/cart/los-angeles-global-vps/&affid=1247) |
| Pro | 3核 EPYC 7002 | 4GB DDR4 | 60G | 6TB | 1Gbps | $20/季 | [购买](https://clients.zgovps.com/index.php?/cart/los-angeles-global-vps/&affid=1247) |
| Premium | 4核 EPYC 7002 | 6GB DDR4 | 80G | 8TB | 1Gbps | $28/季 | [购买](https://clients.zgovps.com/index.php?/cart/los-angeles-global-vps/&affid=1247) |

提醒一句：这个系列官网明确写"国际线路，不优化中国，不能以此为由退款"，下单前务必 ping 测试 IP `207.60.50.4`。

### 4.2 洛杉矶三网优化 AMD EPYC VPS（9929&CMIN2，EPYC 7003）

这是 ZgoVPS 洛杉矶的"主力款"，AMD EPYC 7003 + DDR4/DDR5 + PCIe 4.0 NVMe，回程 9929+CMIN2，性价比很高。

| 套餐 | CPU | 内存 | NVMe | 月流量 | 带宽 | 价格（季付） | 购买 |
|---|---|---|---|---|---|---|---|
| Starter | 1核 EPYC 7003 | 2GB DDR4 | 30G | 1TB | 300Mbps | $18/季 | [购买](https://clients.zgovps.com/index.php?/cart/los-angeles-amd-vps/&affid=1247) |
| Standard | 2核 EPYC 7003 | 3GB DDR4 | 50G | 2TB | 300Mbps | $32/季 | [购买](https://clients.zgovps.com/index.php?/cart/los-angeles-amd-vps/&affid=1247) |
| Pro | 3核 EPYC 7003 | 4GB DDR4 ECC | 80G PCIe4.0 | 2TB | 300Mbps | $45/季 | [购买](https://clients.zgovps.com/index.php?/cart/los-angeles-amd-vps/&affid=1247) |
| Premium | 4核 EPYC 7003 | 6GB DDR4 ECC | 100G PCIe4.0 | 2TB | 300Mbps | $58/季 | [购买](https://clients.zgovps.com/index.php?/cart/los-angeles-amd-vps/&affid=1247) |
| Ultra | 6核 EPYC 7003 | 8GB DDR4 ECC | 120G PCIe4.0 | 2TB | 500Mbps | $78/季 | [购买](https://clients.zgovps.com/index.php?/cart/los-angeles-amd-vps/&affid=1247) |

### 4.3 洛杉矶三网高端优化 AMD EPYC VPS（GIA+9929+CMIN2，EPYC 7002）

比上一档多了 CN2 GIA，三网都走顶级线路，200Mbps 带宽。价格也高一截。

| 套餐 | CPU | 内存 | NVMe | 月流量 | 带宽 | 价格（季付） | 购买 |
|---|---|---|---|---|---|---|---|
| Starter | 1核 EPYC 7002 | 1GB DDR4 | 10G | 500GB | 200Mbps | $18/季 | [购买](https://clients.zgovps.com/index.php?/cart/los-angeles-amd-optimised-vps/&affid=1247) |
| Standard | 2核 EPYC 7002 | 2GB DDR4 | 20G | 1TB | 200Mbps | $32/季 | [购买](https://clients.zgovps.com/index.php?/cart/los-angeles-amd-optimised-vps/&affid=1247) |
| Pro | 3核 EPYC 7002 | 3GB DDR4 | 30G | 1.5TB | 200Mbps | $45/季 | [购买](https://clients.zgovps.com/index.php?/cart/los-angeles-amd-optimised-vps/&affid=1247) |
| Premium | 4核 EPYC 7002 | 4GB DDR4 | 50G | 2TB | 200Mbps | $58/季 | [购买](https://clients.zgovps.com/index.php?/cart/los-angeles-amd-optimised-vps/&affid=1247) |

### 4.4 洛杉矶双 ISP AMD EPYC VPS（9929&CMIN2，双 ISP IP）

这个系列的卖点是 **双 ISP 属性的美国原生 IP**——除了 IP2Location 之外的所有 IP 数据库都识别为双 ISP。注意官方说得很明白：双 ISP IP 是托管在数据中心的，不是真正的住宅家宽，只是有"住宅 IP"的属性，适合解锁流媒体、做跨境业务。去程不优化（为了保 ISP 属性），回程走 9929+CMIN2。

| 套餐 | CPU | 内存 | NVMe | 月流量 | 带宽 | 价格（季付） | 购买 |
|---|---|---|---|---|---|---|---|
| Starter | 1核 EPYC 7002 | 1GB DDR4 | 10G | 500GB | 100Mbps | $20/季 | [购买](https://clients.zgovps.com/index.php?/cart/los-angeles-amd-isp-vps/&affid=1247) |
| Standard | 2核 EPYC 7002 | 2GB DDR4 | 20G | 1TB | 100Mbps | $38/季 | [购买](https://clients.zgovps.com/index.php?/cart/los-angeles-amd-isp-vps/&affid=1247) |
| Pro | 3核 EPYC 7002 | 3GB DDR4 | 30G | 1.5TB | 200Mbps | $56/季 | [购买](https://clients.zgovps.com/index.php?/cart/los-angeles-amd-isp-vps/&affid=1247) |
| Premium | 4核 EPYC 7002 | 4GB DDR4 | 50G | 2TB | 200Mbps | $72/季 | [购买](https://clients.zgovps.com/index.php?/cart/los-angeles-amd-isp-vps/&affid=1247) |

> 这个系列**不支持用优惠码**，直接买就行。实测下来奈飞、ChatGPT、Steam 美区基本都能解锁，原因是 IP 是"处女 IP"，没被前人糟蹋过。

### 4.5 洛杉矶 Intel 性能版 VPS（9929&CMIN2，Xeon Platinum 8452Y）

Intel 党的选项，第四代至强铂金 + DDR5 ECC + PCIe 4.0 NVMe，配置比 AMD 那批还新一点。

| 套餐 | CPU | 内存 | NVMe | 月流量 | 带宽 | 价格（季付） | 购买 |
|---|---|---|---|---|---|---|---|
| Starter | 1核 Xeon 8452Y | 1GB DDR5 ECC | 20G PCIe4.0 | 1TB | 300Mbps | $18/季 | [购买](https://clients.zgovps.com/index.php?/cart/los-angeles-intel-performance-vps/&affid=1247) |
| Standard | 2核 Xeon 8452Y | 2GB DDR5 ECC | 40G PCIe4.0 | 2TB | 300Mbps | $32/季 | [购买](https://clients.zgovps.com/index.php?/cart/los-angeles-intel-performance-vps/&affid=1247) |
| Pro | 3核 Xeon 8452Y | 4GB DDR5 ECC | 80G PCIe4.0 | 2TB | 300Mbps | $45/季 | [购买](https://clients.zgovps.com/index.php?/cart/los-angeles-intel-performance-vps/&affid=1247) |
| Premium | 4核 Xeon 8452Y | 6GB DDR5 ECC | 100G PCIe4.0 | 2TB | 300Mbps | $58/季 | [购买](https://clients.zgovps.com/index.php?/cart/los-angeles-intel-performance-vps/&affid=1247) |

### 4.6 洛杉矶 AMD Ryzen9 性能版 VPS（9929&CMIN2，Ryzen9 7950X）

发烧友向，AMD Ryzen9 7950X 单核性能爆表，跑编译、跑数据库这种吃单核的场景特别合适。目前只有两个套餐在售。

| 套餐 | CPU | 内存 | NVMe | 月流量 | 带宽 | 价格（季付） | 购买 |
|---|---|---|---|---|---|---|---|
| Starter | 1核 Ryzen9 7950X | 1GB DDR5 | 25G | 1TB | 300Mbps | $18/季 | [购买](https://clients.zgovps.com/index.php?/cart/los-angeles-ryzen9-performance-vps/&affid=1247) |
| Standard | 2核 Ryzen9 7950X | 2GB DDR5 | 40G | 2TB | 500Mbps | $28/季 | [购买](https://clients.zgovps.com/index.php?/cart/los-angeles-ryzen9-performance-vps/&affid=1247) |

### 4.7 香港 AMD EPYC VPS（BGP 三网直连）

香港机房，BGP 三网直连，100Mbps 带宽，延迟最低，适合面向国内用户的轻量业务。

| 套餐 | CPU | 内存 | NVMe | 月流量 | 带宽 | 价格（季付） | 购买 |
|---|---|---|---|---|---|---|---|
| Starter | 1核 EPYC 7002 | 1GB DDR4 | 10G | 500GB | 100Mbps | $18/季 | [购买](https://clients.zgovps.com/index.php?/cart/hongkong-amd-vps/&affid=1247) |
| Standard | 2核 EPYC 7002 | 2GB DDR4 | 20G | 1TB | 100Mbps | $32/季 | [购买](https://clients.zgovps.com/index.php?/cart/hongkong-amd-vps/&affid=1247) |
| Pro | 3核 EPYC 7002 | 3GB DDR4 | 30G | 1.5TB | 100Mbps | $45/季 | [购买](https://clients.zgovps.com/index.php?/cart/hongkong-amd-vps/&affid=1247) |
| Premium | 4核 EPYC 7002 | 4GB DDR4 | 50G | 2TB | 100Mbps | $58/季 | [购买](https://clients.zgovps.com/index.php?/cart/hongkong-amd-vps/&affid=1247) |

### 4.8 东京 Intel Gold VPS（BGP 大陆优化）

东京机房（注意是 Tokyo，不是 Osaka），Intel Xeon Gold 6248 + BGP 大陆优化，100Mbps。

| 套餐 | CPU | 内存 | NVMe | 月流量 | 带宽 | 价格（季付） | 购买 |
|---|---|---|---|---|---|---|---|
| Starter | 1核 Xeon Gold 6248 | 1GB DDR4 | 10G | 500GB | 100Mbps | $18/季 | [购买](https://clients.zgovps.com/index.php?/cart/tokyo-intel-vps/&affid=1247) |
| Standard | 2核 Xeon Gold 6248 | 2GB DDR4 | 20G | 1TB | 100Mbps | $32/季 | [购买](https://clients.zgovps.com/index.php?/cart/tokyo-intel-vps/&affid=1247) |
| Pro | 3核 Xeon Gold 6248 | 3GB DDR4 | 30G | 1.5TB | 100Mbps | $45/季 | [购买](https://clients.zgovps.com/index.php?/cart/tokyo-intel-vps/&affid=1247) |
| Premium | 4核 Xeon Gold 6248 | 4GB DDR4 | 50G | 2TB | 100Mbps | $58/季 | [购买](https://clients.zgovps.com/index.php?/cart/tokyo-intel-vps/&affid=1247) |

### 4.9 大阪 AMD EPYC 9354P VPS（IIJ 线路，不优化中国）

大阪的新一代 EPYC Genoa 平台，DDR5 ECC + PCIe 4.0，800Mbps 大带宽。走 IIJ 线路，对中国不优化——再强调一遍，下单前 ping `195.245.229.3`。

| 套餐 | CPU | 内存 | NVMe | 月流量 | 带宽 | 价格（季付） | 购买 |
|---|---|---|---|---|---|---|---|
| Starter | 1核 EPYC 9354P | 1GB DDR5 ECC | 20G PCIe4.0 | 1TB | 400Mbps | $12/季 | [购买](https://clients.zgovps.com/index.php?/cart/osaka-amd-performance-vps/&affid=1247) |
| Standard | 2核 EPYC 9354P | 2GB DDR5 ECC | 40G PCIe4.0 | 2TB | 800Mbps | $17/季 | [购买](https://clients.zgovps.com/index.php?/cart/osaka-amd-performance-vps/&affid=1247) |
| Pro | 3核 EPYC 9354P | 4GB DDR5 ECC | 80G PCIe4.0 | 2TB | 800Mbps | $24/季 | [购买](https://clients.zgovps.com/index.php?/cart/osaka-amd-performance-vps/&affid=1247) |
| Premium | 4核 EPYC 9354P | 6GB DDR5 ECC | 100G PCIe4.0 | 2TB | 800Mbps | $36/季 | [购买](https://clients.zgovps.com/index.php?/cart/osaka-amd-performance-vps/&affid=1247) |
| Ultra | 6核 EPYC 9354P | 8GB DDR5 ECC | 120G PCIe4.0 | 2TB | 800Mbps | $48/季 | [购买](https://clients.zgovps.com/index.php?/cart/osaka-amd-performance-vps/&affid=1247) |

### 4.10 大阪 AMD Ryzen9 7950X VPS（IIJ 线路）

大阪的 Ryzen9 版本，单核更强，同样 IIJ 线路。测试 IP 用 `195.245.229.134`。

| 套餐 | CPU | 内存 | NVMe | 月流量 | 带宽 | 价格（季付） | 购买 |
|---|---|---|---|---|---|---|---|
| Starter | 1核 Ryzen9 7950X | 1GB DDR5 ECC | 20G PCIe4.0 | 1TB | 800Mbps | $15/季 | [购买](https://clients.zgovps.com/index.php?/cart/osaka-amd-ryzen9-performance-vps/&affid=1247) |
| Standard | 2核 Ryzen9 7950X | 2GB DDR5 ECC | 40G PCIe4.0 | 2TB | 800Mbps | $25/季 | [购买](https://clients.zgovps.com/index.php?/cart/osaka-amd-ryzen9-performance-vps/&affid=1247) |

### 4.11 德国法尔肯施泰因 Intel VPS（国际线路）

德国机房，Intel Xeon Gold 5412U + DDR5 ECC，1Gbps 大带宽，国际线路。年付特价很香，适合欧洲业务。

| 套餐 | CPU | 内存 | NVMe | 月流量 | 带宽 | 价格（年付特价） | 购买 |
|---|---|---|---|---|---|---|---|
| Starter | 1核 Xeon Gold 5412U | 1GB DDR5 ECC | 20G | 2TB | 1Gbps | $12.90/年 | [购买](https://clients.zgovps.com/index.php?/cart/falkenstein-intel-vps/&affid=1247) |
| Standard | 2核 Xeon Gold 5412U | 2GB DDR5 ECC | 40G | 4TB | 1Gbps | $22.90/年 | [购买](https://clients.zgovps.com/index.php?/cart/falkenstein-intel-vps/&affid=1247) |

### 4.12 洛杉矶 AMD EPYC VDS（国际线路，独享资源）

VDS 是独享资源的虚拟专用服务器，比 VPS 更"硬核"，支持自备 Windows 授权装系统，适合跑满 CPU 的重负载。国际线路，对中国不优化。

| 套餐 | CPU | 内存 | NVMe | 月流量 | 带宽 | 价格（年付） | 购买 |
|---|---|---|---|---|---|---|---|
| Starter | 2核 EPYC 7003 | 4GB DDR4 | 60G | 10TB | 1Gbps | $66/年 | [购买](https://clients.zgovps.com/index.php?/cart/los-angeles-amd-vds/&affid=1247) |
| Standard | 4核 EPYC 7003 | 8GB DDR4 | 150G | 20TB | 1Gbps | $96/年 | [购买](https://clients.zgovps.com/index.php?/cart/los-angeles-amd-vds/&affid=1247) |
| Pro | 8核 EPYC 7003 | 16GB DDR4 | 250G | 20TB | 2Gbps | $166/年 | [购买](https://clients.zgovps.com/index.php?/cart/los-angeles-amd-vds/&affid=1247) |
| Premium | 12核 EPYC 7003 | 24GB DDR4 | 500G | 20TB | 2Gbps | $258/年 | [购买](https://clients.zgovps.com/index.php?/cart/los-angeles-amd-vds/&affid=1247) |

## 五、ZgoVPS 最新优惠码与省钱攻略

测好了 IP、选好了套餐，下单前别忘了薅优惠码。ZgoVPS 目前有一个长期有效的循环优惠码：

**优惠码：`8NU44CM6LZ`**
- 优惠幅度：**9.5 折循环优惠**（续费同价，每年都打折）
- 适用周期：仅限**年付**
- 适用范围：常规洛杉矶 VPS 套餐（双 ISP 系列、特价套餐不能用）
- 有效期：目前确认可叠加使用，长期有效

用法很简单：在 👉 [ZgoCloud 结算页](https://bit.ly/zgovps) 选好年付套餐后，在订单页找到促销码输入框，填入 `8NU44CM6LZ` 就行。重点是"循环优惠"——意思是第二年、第三年续费还是 9.5 折，不像有些商家只给首年优惠，第二年原价续费坑你一把。

省钱建议：

1. **想回国快、又要便宜** → 洛杉矶 AMD EPYC 7003 系列（4.2 那张表）的 Starter，年付用优惠码后大概 $68/年，2GB 内存 + 300Mbps + CMIN2，性价比之王。
2. **纯海外业务、预算紧** → 洛杉矶 Global 国际线路 Starter，年付 $28，或者德国 Starter 年付 $12.9，拿到手就是一台能跑的 KVM。
3. **要解锁流媒体、做跨境** → 洛杉矶双 ISP 系列（4.4），虽然不能用优惠码，但 IP 属性值这个价。
4. **追求极致单核性能** → Ryzen9 7950X 系列（4.6），跑编译、跑游戏服务器爽到飞起。
5. **面向国内用户、要低延迟** → 香港（4.7）或东京（4.8），延迟比洛杉矶低一半。

> 注意：ZgoVPS 开了 WHMCS MaxMind 自动反欺诈，下单时 IP 地址、电话号码、选的国家必须保持一致（不要求真实，但要一致），否则会被判 Fraud 订单下不成。

## 六、实测延迟参考与选购避坑

光说不练假把式，我把搜到的几个第三方实测数据汇总一下，给你做个参考。**注意这些都是第三方测评时的数据，到你家宽带可能完全不一样**——这就是为什么要强调"先测 IP 再下单"。

**洛杉矶 AMD EPYC 7003 CMIN2 系列（第三方实测）**

- 硬盘 I/O：约 796MB/s（NVMe 阵列正常水平）
- 国内三网平均延迟：电信约 160ms、联通约 150ms、移动约 145ms（CMIN2 加持下移动最快）
- 回程路由：电信/联通走 AS9929，移动走 CMIN2，符合官方宣传
- 流媒体解锁：奈飞美区、ChatGPT、Steam 美区基本都能解锁（双 ISP 系列更稳）

**大阪 AMD EPYC 9354P IIJ 系列**

- 国内 ping 延迟：电信/联通约 80-100ms，移动约 60-80ms
- 注意：IIJ 是日本本土优质线路，对中国不优化，但物理距离近，延迟反而比洛杉矶低
- 适合：面向日本用户的业务、东南亚节点中转

**德国法尔肯施泰因**

- 国内 ping 延迟：约 217ms（国际线路正常水平）
- 欧洲本地（法兰克福）：约 6ms
- 适合：欧洲外贸站、给欧洲用户提供服务

**几个避坑点**

1. **双 ISP IP 不是住宅 IP**。官方明说"数据中心托管，非光纤入户"，只是有 ISP 属性。指望它绕过 Netflix 严格检测的，建议先买个月付试试。
2. **国际线路和 IIJ 线路不退款**。下单前 ping 测试 IP，延迟高就别买，买了也别想着退。
3. **去程和回程可能不对称**。洛杉矶优化线路去程走国际（保 IP 属性），回程走 9929/CMIN2（保速度），这是设计如此，不是 bug。
4. **特价套餐不能用优惠码**。如果你想用 `8NU44CM6LZ` 省那 5%，就别选特价款，选常规款年付。
5. **测试 IP 会变**。机房偶尔调整 IP 段，本文给的测试 IP 是目前能查到的，下单前最好到 👉 [ZgoCloud 官方产品页](https://bit.ly/zgovps) 核对最新 Looking Glass。

## 七、结语：测 IP 这件事，省不得

写到最后再说一句掏心窝的：买 VPS 这事，**测 IP 永远比看测评重要**。别人测出来 150ms，到你家可能 280ms——因为你家宽带可能走的不是同一个出口。ZgoVPS 把测试 IP 和 Looking Glass 都公开挂着，这本身就是一种"我有底气让你先试"的态度。

所以流程很简单：先 ping `207.60.50.4`（洛杉矶）或 `195.245.229.134`（大阪）或 `23.165.200.2`（德国），看延迟合不合你心意；再 traceroute 看回程走 9929 还是 CMIN2 还是国际；最后到 👉 [ZgoCloud 产品页](https://bit.ly/zgovps) 选对应的套餐，年付记得填优惠码 `8NU44CM6LZ`。三步走完，基本不会踩坑。

ZgoVPS 这家在我接触过的中小 VPS 商家里，算线路分得细、硬件给得足、规则说得明白的那一类——不退款的规定虽然听着硬，但人家提前讲清楚了，比那种"承诺优化实际绕道"的商家靠谱多了。如果你最近在找一台洛杉矶 CMIN2 线路的小鸡，或者想试试双 ISP 解锁流媒体，它值得放进你的候选清单里。

> 测试 IP 跑通了，就可以放心去 👉 [ZgoCloud 选购套餐](https://bit.ly/zgovps) 了，年付记得带上优惠码 `8NU44CM6LZ`。
