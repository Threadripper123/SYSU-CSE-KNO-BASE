# SYS-04 网络 2025–2026：HTTP/3、QUIC、BBRv3、Wi-Fi 7、IPv6

> 主题库：现代软件与系统（2023–2026）　｜　源文件：（纯文本条目）　｜　7,555 字符　｜　30 个片段　｜　更新于 2026-11-15
> 资料截至 2026-11-15；覆盖 2023-11 至 2026-11，重点 2024–2026。

**一句话结论**：这三年的网络进步不在"更粗的管道"，而在"管道开始自己测量自己"——QUIC 把连接状态搬进用户态，HTTP/3 把多路复用从 TCP 的队头阻塞里解放出来，BBRv3 用带宽/RTT 模型替代"丢包即降速"，Wi-Fi 7 用多链路和子信道打孔把"不可靠"变成"可计算"。代价是可观测性、公平性和终端省电都更难做。

## 一、关键变化一览

| 主题 | 2023 年的状态 | 2026 年 11 月的状态 | 证据等级 |
|---|---|---|---|
| HTTP/3 | RFC 9114（2022-06）刚落地，CDN 先行 | 41.1% 的网站声明支持 HTTP/3（W3Techs，2026-11） | 一手统计 |
| QUIC 传输 | RFC 9000/9001/9002（2021-05）为基线 | 多路径扩展成为 RFC 10069（2026-11，Standards Track） | 一手标准 |
| 拥塞控制 | BBRv1 大规模部署，BBRv2 草案停在个人稿 | BBRv3 进入 CCWG 工作组草案 `draft-ietf-ccwg-bbr-07`（2026-11-02），目标 Experimental | 一手草案 |
| Wi-Fi | Wi-Fi 6E 铺开，Wi-Fi 7 认证刚起步 | IEEE 802.11 工作组里 TGbe（EHT）已转入 inactive，TGbn（UHR，即 Wi-Fi 8）在研，并新设 TGbt 做后量子密码 | 一手工作组页 |
| IPv6 | 双栈为主，NAT64/464XLAT 属边缘方案 | "IPv6-mostly" 与 IPv4-as-a-Service 进入 IESG 流程 | 一手草案 |

## 二、HTTP/3 与 QUIC：从"新协议"变成"默认选项"

QUIC 的协议底座是四份 2021 年的 RFC：`RFC 9000`（传输）、`RFC 9001`（TLS 1.3 集成）、`RFC 9002`（丢包检测与拥塞控制），HTTP/3 的映射是 `RFC 9114`，头部压缩是 `RFC 9204`（QPACK），不可靠数据报是 `RFC 9221`。这套东西的技术要点只有一句话：把"连接"从五元组里解耦出来——连接 ID 让手机从 Wi-Fi 切到蜂窝时不用重新握手，流（stream）让一个丢包只阻塞它自己那条流。

2023 年 5 月出的 `RFC 9369`（QUIC v2）和 `RFC 9368`（兼容版本协商）值得单独说：它们不是性能升级，而是**防僵化**（ossification）。QUIC v1 的报文头部有很强的可识别特征，中间盒一旦开始"认字节"，协议就再也改不动了。v2 换了一套盐和标签，目的是让网络设备无法假设自己看到的一定是 v1。

2026 年 11 月的变化是 **多路径**：`RFC 10069`《Managing Multiple Paths for a QUIC Connection》成为 Standards Track，作者来自阿里巴巴、UMONS、UCLouvain、Private Octopus 与 Ericsson。它引入显式的 path identifier，允许一条连接同时走 Wi-Fi 和蜂窝，并规定路径的创建、删除与密钥更新；但**故意不规定**地址发现和多路径调度策略——也就是说，标准只给管道，怎么分流是浏览器和操作系统的事。

工作组里还有几条线没走完，恰好能说明行业的关注点：

- `draft-ietf-quic-reliable-stream-reset-11` 已在 RFC 编辑队列，做"流重置但仍交付已收到的部分数据"——对 HTTP 分块上传和游戏状态同步都是刚需。
- qlog 的三份草案（main schema / QUIC events / HTTP/3 events）都在 2026-11-02 出了新版本（-15/-14）。qlog 是目前唯一标准化的 QUIC 内部可观测格式，生产环境排障基本绕不开它。
- `draft-ietf-quic-ack-frequency-14` 已在 2026-02-05 过期。这说明"减少 ACK 开销"这条线在 WG 里没有形成足够共识——它是一个典型的"实现有效但标准难写"的议题。

部署面上，W3Techs 在 2026 年 11 月给出的数字是 **41.1% 的网站使用 HTTP/3**。这个数字要正确理解：它统计的是"站点是否声明支持"，不是"流量占比"；大厂的内容分发早已以 HTTP/3 为主，而长尾小站仍停在 HTTP/1.1。HTTP/3 的发现仍靠响应头 `Alt-Svc: h3=":443"` 或 HTTPS 记录，UDP 443 被防火墙挡掉时浏览器静默回落到 HTTP/2——这也是"我明明开了 H3 却感觉没变快"的最常见原因。

## 三、BBRv3：把"丢包"从信号降级为证据

BBR 的核心思想是显式建模：用交付速率估计瓶颈带宽 `max_bw`，用 RTT 估计传播时延 `min_rtt`，两者相乘就是"管道容量"（BDP），发送速率按这个容量来定，而不是等丢包发生再退让。它的状态机只有四个：Startup（指数探速）、Drain（排空启动期队列）、ProbeBW（周期性轻微过冲以维持带宽估计）、ProbeRTT（周期性压低在途数据以刷新 `min_rtt`）。

BBRv1 的老问题是只管带宽和 RTT、对丢包几乎不反应：和 CUBIC 共存时会抢走大部分带宽，在浅缓存的接入网里丢包率也偏高。BBRv2 引入了丢包/ECN 响应和 `inflight_hi` 上限，但实现复杂、在部分场景吞吐反而塌陷，草案长期停在个人稿。

2026 年的状态是：BBRv3 终于进了 IETF 拥塞控制工作组，成为 `draft-ietf-ccwg-bbr-07`（2026-11-02 更新，Intended status: Experimental，作者 Neal Cardwell、Ian Swett、Joseph Beshay，分别来自 Google、Google、Meta，并列入 IETF-127 议程）。草案正文明确写的是"version 3 of the BBR algorithm, BBRv3"。几个可直接引用的常量：

| 参数 | 值 | 含义 |
|---|---|---|
| `BBR.LossThresh` | 2% | 每 RTT 可容忍的丢包率上限，超过才认为"真的拥塞" |
| `BBR.Beta` | 0.7 | 检测到丢包后每 RTT 的乘性降速系数 |
| `BBR.Headroom` | 0.15 | 为交叉流量预留的空闲余量比例 |
| `BBR.MinPipeCwnd` | `4 * C.SMSS` | 兼容"隔包 ACK"的最小在途窗口 |

这套参数的含义是：BBR 不再把单个丢包当作拥塞证据（无线误码、ECN 误标都会造成丢包），只有当丢包率稳定超过 2% 才降速，且降速是渐进的 0.7 倍而不是腰斩。这对着眼于"5G/Wi-Fi 最后一公里"的部署很关键。

实践上最容易踩的坑是：Linux 主线内核里的 `tcp_bbr` 模块是 BBRv1 语义。想用 v3，要么用厂商内核（Google 的 QUIC/ TCP 分支），要么在用户态 QUIC 栈里实现——QUIC 的拥塞控制本来就是可插拔的，`RFC 9002` 只规定 NewReno 式基线，所以 QUIC 反而是 BBRv3 更好的灰度载体。

## 四、Wi-Fi 7 与 Wi-Fi 8：从峰值转向确定性

Wi-Fi 7 对应 IEEE 802.11be（Extremely High Throughput）。四个关键能力：320 MHz 信道（只有 6 GHz 频段凑得出来）、4096-QAM（比 1024-QAM 多 20% 理论速率，但要求更高的 SNR）、多链路操作 MLO（同一条流量可跨 2.4/5/6 GHz 并行或快速切换）、以及 MRU puncturing（把受干扰的 20 MHz 子信道打掉，剩下的频谱继续用，而不是整条 320 MHz 作废）。

在 IEEE 802.11 工作组首页上，TGbe 已经被归入"Inactive Task Groups"，意味着标准制定工作完成；在研的是 TGbn（Ultra High Reliability，即业界口中的 Wi-Fi 8），此外还有 TGbp（环境能量通信）、TGbq（集成毫米波）、TGbr（增强光通信）、TGbi（增强隐私保护）、TGbh（随机 MAC）等。**值得注意的是新设的 TGbt：Post-Quantum Cryptography**——Wi-Fi 的管理帧保护和认证握手也要做后量子迁移，这条线与 SYS-06 的 PQC 主线是同一件事。

Wi-Fi 8 的定位变化很值得琢磨：它的名字不是"更高速"而是"超高可靠"。这说明 Wi-Fi 联盟和 IEEE 都判断，家庭/企业场景的瓶颈已经从"峰值速率"变成"时延抖动、漫游断流、边缘覆盖"。对普通用户，这意味着 Wi-Fi 7 到 Wi-Fi 8 的升级感知会远小于 Wi-Fi 5 到 Wi-Fi 6。

现实约束也要说清楚：320 MHz 在 6 GHz 上意味着一条信道吃掉整段频谱，邻居 AP 只能打孔共存；6 GHz 穿墙差，实际部署几乎一定是"6 GHz 供近场高吞吐 + 5 GHz 兜底覆盖"；MLO 的省电模式（单链路空闲）直接决定手机续航，早期终端在这块的表现差异很大。

## 五、IPv6：从"双栈过渡"走向"IPv6-mostly"

IPv6 的地址结构（128 位、每子网 `/64`、SLAAC 无状态自动配置、DHCPv6 有状态配置）十几年没变，变的是**部署策略**。过去的标准做法是双栈全开，IPv4 和 IPv6 各跑一套；2025–2026 年 IETF v6ops 工作组的主线变成了"IPv6 默认、IPv4 按需"：

| 草案 | 日期 | 作用 |
|---|---|---|
| `draft-ietf-v6ops-6mops-12` IPv6-mostly Networks | 2026-10-29 提交 IESG（Informational） | 定义"IPv6 为主、IPv4 仅通过特定服务可达"的网络运维模型 |
| `draft-ietf-v6ops-framework-md-ipv6only-underlay-32` | 在 RFC 编辑队列 | 多域 IPv6-only 承载网 + IPv4-as-a-Service 的框架 |
| `draft-ietf-v6ops-claton-16` 464XLAT CLAT | 2026-03-05 进 RFC 编辑队列（Proposed Standard） | 终端侧 464XLAT 的行为规范，让只有 IPv4 的应用也能跑在 IPv6-only 网络上 |
| `draft-ietf-v6ops-5g-ipv6only-00` | 2026-10-14 | 5G 移动网络 IPv6-only 部署的注意事项 |
| `draft-ietf-v6ops-ipv6-app-testing-08` | Last Call 至 2026-11-23（BCP） | 给"应用是否真的支持 IPv6"定检查项 |

过渡工具箱没变：NAT64（`RFC 6146`）做 IPv6→IPv4 的地址族转换，DNS64（`RFC 6147`）合成 AAAA，464XLAT（`RFC 6877`）在终端放一个 CLAT、在运营商侧放一个 PLAT，让老应用以为自己在用 IPv4。真正的变化是**这些从边缘方案变成了主力方案**。

对开发者的直接后果：如果你的 SDK 硬编码 IPv4、或者服务端只监听 IPv4，在 IPv6-only 的 5G 切片或企业访客网络里会直接不可达——而且报错通常是"连接超时"，很难定位。检查清单很短：`getaddrinfo` 是否用了 `AF_UNSPEC`、监听是否绑了 `::`（双栈）、日志里是否记录了客户端地址族、健康检查是否只打 IPv4。

## 六、对开发者与运维的实际影响

1. **别把 UDP 当二等公民**。QUIC 需要 UDP 443 放行、足够的 conntrack 表项、以及合理的 socket 缓冲区（`net.core.rmem_max`）。很多"HTTP/3 不稳定"的案例最后都指向中间设备对 UDP 的限速或状态跟踪超时。
2. **拥塞控制要按路径选**。跨国长肥管道上 BBR 系优势明显；数据中心内部 CUBIC/DCQCN 仍是主流，因为那里的丢包几乎只来自拥塞。
3. **观测先行**。QUIC 侧用 qlog，TCP 侧用 `ss -ti` 看 `cwnd`/`rtt`/重传，队列侧用 `tc -s qdisc` 看 backlog 与 drop。
4. **IPv6 不是"以后再说"**。2027 年起，IPv6-only 网络覆盖的用户比例会开始明显影响可用性指标。
5. **Wi-Fi 侧先对齐能力**。AP 开了 MLO 但终端不支持，会退回单链路；反过来，终端支持而 AP 未配置，也拿不到多链路收益。

一条常用的排查命令组合：

```bash
curl -sI --http3 https://example.com | head -5      # 确认服务端是否给 h3
ss -ti dst :443                                     # 看 cwnd / rtt / retrans
tc -s qdisc show dev eth0                           # 看排队与丢包
```

## 七、常见坑与局限

- **把 HTTP/3 当成"一定更快"**。高丢包、小文件、短连接场景下，0-RTT 与 QPACK 的收益有限；队头阻塞只是从 TCP 层挪到了"丢包恢复策略"这一层。
- **以为开了 `tcp_bbr` 就是 BBRv3**。主线内核该模块是 v1 语义，v3 需要额外补丁或用户态实现。
- **320 MHz 的共存代价**。一条信道吃满整段频谱时，邻居只能打孔，密集公寓场景的实际收益远小于实验室数字。
- **IPv6 安全组漏配**。云厂商默认安全组往往只有 IPv4 规则，表现为"能 ping 通但端口不通"。
- **DNS64 与 DNSSEC 冲突**。合成 AAAA 会破坏签名链，需要显式配置例外。
- **本文的证据边界**：Wi-Fi 7 认证启动时间、802.11be 正式出版日期、全球 IPv6 采用率的具体百分比，本次未能取到一手页面（详见"不确定项"），文中只给结构性判断。

## 来源

访问日期均为 2026-11-15。

- RFC 9114 HTTP/3 — https://www.rfc-editor.org/rfc/rfc9114.html
- RFC 9000 QUIC 传输 — https://www.rfc-editor.org/rfc/rfc9000.html
- RFC 9369 QUIC v2 — https://www.rfc-editor.org/rfc/rfc9369.html
- RFC 10069 Multipath QUIC（2026-11） — https://www.rfc-editor.org/rfc/rfc10069.html
- BBRv3 草案与状态 — https://datatracker.ietf.org/doc/draft-ietf-ccwg-bbr/ ；正文 https://www.ietf.org/archive/id/draft-ietf-ccwg-bbr-07.html
- QUIC 工作组文档列表 — https://datatracker.ietf.org/wg/quic/documents/
- HTTP/3 站点使用率 — https://w3techs.com/technologies/details/ce-http3
- IEEE 802.11 工作组（任务组状态） — https://www.ieee802.org/11/
- v6ops 工作组文档列表 — https://datatracker.ietf.org/wg/v6ops/documents/

## 不确定项

- **Wi-Fi CERTIFIED 7 认证启动日期**：业界普遍记为 2024 年 1 月，但 Wi-Fi Alliance 站点正文由 JS 渲染，本次只取到导航框架，未拿到公告原文，故正文只写"2024 年"。
- **IEEE 802.11be-2024 的正式出版日期**：IEEE Xplore 需登录，未能核实；本文只依据工作组页面判定"TGbe 已 inactive、标准工作完成"。
- **Wi-Fi 8（802.11bn）时间表**：公开讨论中有"2028 年定稿"的说法，未见工作组正式里程碑，因此未写入正文数值。
- **全球 IPv6 采用率**：APNIC Labs 的页面数据内嵌在图表数组里且抓取被截断，未取到 2026 年数值；本文因此不给单一百分比。
- **BBRv3 的实际部署规模**：缺少 2026 年的一手披露，本文只描述草案状态、参数与实验性定位。
- **Wi-Fi 7 的实测增益**：未引用任何厂商或第三方的吞吐测试数据，仅描述机制。
