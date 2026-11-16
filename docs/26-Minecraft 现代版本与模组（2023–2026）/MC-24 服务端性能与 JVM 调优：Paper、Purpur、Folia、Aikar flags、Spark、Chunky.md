# MC-24 服务端性能与 JVM 调优：Paper、Purpur、Folia、Aikar flags、Spark、Chunky

> 主题库：Minecraft 现代版本与模组（2023–2026）　｜　源文件：（纯文本条目）　｜　7,575 字符　｜　38 个片段　｜　更新于 2026-11-15
> 资料截至 2026-11-15；覆盖 2023-11 至 2026-11，重点 2024–2026。

# MC-24 服务端性能与 JVM 调优：Paper、Purpur、Folia、Aikar flags、Spark、Chunky

## 一句话结论

2025–2026 的服务端调优里，真正决定成败的不是"哪个 fork 更快"，而是三件基础事：**Java 版本对上没有**（26.1 起必须 Java 25）、**堆内存有没有给操作系统留出余量**、**有没有先用 spark 定位再动手**。fork 的选择（Paper / Purpur / Folia）是在这三件事之后才有意义的加分项，顺序颠倒就会把时间花在调一个本来不是瓶颈的地方。

## 版本与 Java 要求

2026 年起 MC 改用年份版本号，Paper 完全跟上了这个节奏：官方下载接口里同时存在 `26.3`、`26.2`、`26.1.2` 与 `1.21.11`、`1.21.10` 等版本族。

| Paper 版本 | 推荐 Java | 说明 |
| --- | --- | --- |
| 1.20 – 1.21.11 | Java 21 | 2024–2025 的主流长期线 |
| 26.1 及以上 | Java 25 | 官方页面写明"至少 Java 25" |
| 1.17 – 1.19 | Java 17 | 老整合包常见 |
| 1.12 – 1.16.4 | Java 11 | 仅维护存量 |

这张表是官方 Getting started 页面的原样整理。它的实际价值在于排错顺序：服务器起不来、报 `UnsupportedClassVersionError` 时，先看这一列，再怀疑插件。

## 各服务端当前状态

| 项目 | 当前状态（2026-11-15） | 关键节点 |
| --- | --- | --- |
| Paper | 26.3 已到 `build 252`，2026-11-14 发布，STABLE 通道 | 首个 26.3 构建 2026-09-15，与 MC 26.3 同日；两个月内推了 164 个构建 |
| Folia | 提供 26.3 / 26.2 / 26.1.2 | 区域化多线程，插件兼容面窄 |
| Purpur | 主干开发分支为 `ver/26.2`，另存 `ver/1.21.11` | 26.3 支持晚于 Paper |
| Velocity | 4.x 线（`4.3.1`）与 3.x 线（`3.5.1`）并存 | 见 MC-25 |
| Chunky | `1.5.66`（2026-11-04） | 支持 Spigot/Paper/Fabric/Forge/NeoForge/Sponge |
| spark | `1.10.187`（2026-09-19） | Paper 1.21 起内置 |

Paper 的构建速度值得单独说一句：26.3 从 2026-09-15 的第一个 alpha 构建到 2026-11-14 的 `build 252`，两个月 164 个构建，平均每天两个多。这个节奏意味着**"用最新构建"本身是有风险的**——修 bug 快，引入回归也快。生产服应该固定一个已知稳定的构建号，而不是每天拉最新。

## Aikar's flags：原文与内存公式

这是 PaperMC 官方文档给出的完整启动命令，直接可复制（把堆大小改成你的值）：

```
java -Xms10G -Xmx10G -XX:+UseG1GC -XX:+ParallelRefProcEnabled \
  -XX:MaxGCPauseMillis=200 -XX:+UnlockExperimentalVMOptions \
  -XX:+DisableExplicitGC -XX:+AlwaysPreTouch -XX:G1NewSizePercent=30 \
  -XX:G1MaxNewSizePercent=40 -XX:G1HeapRegionSize=8M -XX:G1ReservePercent=20 \
  -XX:G1HeapWastePercent=5 -XX:G1MixedGCCountTarget=4 \
  -XX:InitiatingHeapOccupancyPercent=15 -XX:G1MixedGCLiveThresholdPercent=90 \
  -XX:G1RSetUpdatingPauseTimePercent=5 -XX:SurvivorRatio=32 \
  -XX:+PerfDisableSharedMem -XX:MaxTenuringThreshold=1 \
  -Dusing.aikars.flags=https://mcflags.emc.gs -Daikars.new.flags=true \
  -jar paper.jar --nogui
```

三件事必须记住：

**第一，`-Xms` 与 `-Xmx` 设成同一个值。** 文档的示例里两个都是 10G。固定堆避免运行期反复扩缩容，`-XX:+AlwaysPreTouch` 会在启动时把内存真正摸一遍，代价是启动变慢、换来运行期没有缺页抖动。

**第二，不要把物理内存全给 JVM。** 官方原话是：主机说 8GB，就不要用 8GB；Minecraft 和 Java 在 `-Xmx` 之外还需要内存，建议把 `-Xms`/`-Xmx` 减少约 1000–1500MB，8GB 主机用 `6500MB` 比较安全。否则要么 OOM，要么被系统的 OOMKiller 直接杀掉进程——后者在容器里尤其常见，表现为"服务器毫无征兆地消失"。

**第三，多给内存不等于更快。** 官方建议至少 6–10GB，并明确说超过某个点就是收益递减，买 32GB 内存给一个服是浪费。G1GC 确实偏好更大的堆，但"偏好"不等于"越大越好"。

这套参数的内核是 G1 的三个旋钮：`G1NewSizePercent=30` / `G1MaxNewSizePercent=40` 把新生代锁在 30%–40%，`MaxGCPauseMillis=200` 把停顿目标定在 200ms，`InitiatingHeapOccupancyPercent=15` 让并发标记提前启动。MC 的分配模式是"大量短命对象 + 少量长命区块数据"，所以新生代给大、晋升阈值压到 `MaxTenuringThreshold=1`，让对象尽快死在新世代里。

## spark：先定位，再动手

Paper 从 1.21 起内置 spark，官方文档把它列为"首选"的性能分析方式：

`/spark profiler start --timeout 600`

十分钟后命令会返回一个网页链接，火焰图、TPS 明细、GC 与线程状态都在里面。要点是**问题正在发生时采样**——文档特意加了这条警告，事后采一段平静期等于白采。

两个细节容易被忽略：

- 想用比内置版本更新的 spark，把独立 spark 插件丢进 `plugins`，再把系统属性 `paper.preferSparkPlugin` 设为 `true`。
- 想在 PlaceholderAPI 里用 spark 的占位符，得先 `/papi ecloud download spark` 再 `/papi reload`，内置版不会自动带扩展。

同时要注意 Timings 的命运：它在 1.21 已被关闭默认开启，官方明确说它"多年无人维护、报告对新手不友好"。**现在还拿 Timings 报告去求助，等于交了一份没人会看的材料。**

## Folia：区域化多线程的边界

Folia 是 Paper 的分支，加的是 regionized multithreading——把世界切成若干区域，每个区域独立在主线程之外 tick。它的核心是 `ThreadedRegionizer`：每个世界一个 regionizer，负责创建、合并、拆分区域，并决定哪些区域可以被 tick。

官方文档给出的不变量解释了它为什么能并行而不出竞态：任何一个区块持有者**有且仅有一个**所属区域；相邻区块会被合并进同一区域，因此跨区域的方块更新不会在 tick 中途互相踩踏。

这决定了适用边界：**普通 Bukkit 插件在 Folia 上大概率不能用**，因为它们默认"整个世界在一个线程里跑"。要上 Folia，插件需要按区域调度 API 重写。所以判断标准不是"我的服大不大"，而是"我的插件生态有没有 Folia 版本"。Folia 已经跟到 26.3，说明它在维护，但这不等于你的插件包能搬过去。

## Purpur 与 Chunky：两个不同方向的补强

**Purpur** 是 Paper 的下游分支，方向是"把原版里写死的行为变成可配置项"，而不是改性能。它当前的开发主干在 `ver/26.2`，另有 `ver/1.21.11` 分支——也就是说 26.3 线的支持会比 Paper 晚一步。用它之前先确认你要的版本有分支，别把 26.2 的配置直接抄到 26.3。它的文档站在 2026-11-15 访问时发生重定向，本文没有取到 `purpur.yml` 的具体键名，这一项标为待核实。

**Chunky** 解决的是另一个问题：世界生成。新服开图时，玩家每走一步都在触发生成，TPS 会被拖成锯齿。Chunky 支持 Spigot、Paper、Fabric、Forge、NeoForge、Sponge 六种平台，当前版本 `1.5.66`（2026-11-04），累计下载 2032 万次。标准流程是设半径再启动，例如 `/chunky radius 5000` 配合 `/chunky start`，让它在开服前把地图跑完。预生成的收益不在"生成更快"，而在"把生成的开销挪到没人玩的时候"。

## 从症状到动作：排错顺序表

调优最容易犯的错是"看到卡就加内存"。先把症状分类，再决定动哪一层：

| 症状 | 先看什么 | 大概率原因 | 该动的旋钮 |
| --- | --- | --- | --- |
| TPS 周期性掉到个位数，几秒后自己恢复 | GC 日志 | 堆偏小 / 新生代偏小，触发 Full GC | ⟦-Xmx⟧ 与 G1 新生代比例 |
| TPS 稳定在 18–19，人一多就继续掉 | spark 火焰图 | 实体 AI、寻路、区块加载 | ⟦simulation-distance⟧、刷怪上限 |
| 玩家跑图时整服卡顿 | 区块生成 | 未预生成 | Chunky 预生成 |
| 内存缓慢上涨，几小时后进程消失 | 堆使用曲线 | 插件内存泄漏 | 二分法摘插件 |
| 启动要五分钟以上 | 启动日志 | ⟦AlwaysPreTouch⟧ + 大堆 | 属正常代价，不是故障 |

两类卡的区分是关键：**周期性的、能自己恢复的卡，几乎都是 GC**；**持续偏低、随人数线性恶化的卡，几乎都是逻辑**（实体、区块、插件）。前者调 JVM 参数有用，后者调 JVM 参数完全没用——这也是为什么"加内存"经常没效果。

## GC 日志与容器内存

Aikar's flags 页面里专门有"Java GC logging"一节，建议开启 GC 日志。原因很实际：不加日志，你只能看到"卡了一下"；加了日志，能看到是 Young GC 频繁（新生代太小）还是 Full GC 偶发（老年代压力或内存泄漏）。前者调 ⟦G1NewSizePercent⟧，后者要查插件。

容器化部署还有一层坑：⟦-Xmx⟧ 只是 Java 堆的上限，**堆外内存不算在里面**——直接内存、Netty 的缓冲、线程栈、元空间都要占容器的内存额度。所以在 Docker 里把 ⟦-Xmx⟧ 设成等于容器 limit 是最常见的死法：进程不是抛 ⟦OutOfMemoryError⟧，而是被内核直接 kill，日志断在最后一行正常输出上。留出 1–1.5GB 余量是保守但不吃亏的做法。

## 对不同角色的实际影响

**服主**：先做三件事——确认 Java 版本、按"物理内存减 1000–1500MB"设堆、把 spark 装好并学会采样。做完再看 fork。一个 20 人的 Paper 服，把 `view-distance` 从 10 降到 8 带来的收益，往往大于换 fork。

**插件开发者**：26.x 之后要同时面对两条版本线（`1.21.11` 与 `26.3`），API 版本声明要写清楚。想支持 Folia 就得从头用区域调度，不能靠兼容层糊过去。

**运维/自动化**：如果你有脚本从 `api.papermc.io/v2` 拉构建号，**它已经在 2026 年失效**——现在返回 `{"ok":false,"error":"sunset"}`。新接口是 `https://fill.papermc.io/v3/projects/paper`，返回结构也变了。这类静默失效最麻烦：脚本不报错，只是永远下到旧包。

## 上线前的五分钟检查清单

1. ⟦java -version⟧ 与上文的 Java 对照表一致；
2. ⟦-Xms⟧ 与 ⟦-Xmx⟧ 相等，且比物理内存少 1000–1500MB；
3. 启动命令里有 ⟦-Dusing.aikars.flags⟧ 标记，方便日后一眼认出用的是哪套参数；
4. ⟦/spark profiler start --timeout 600⟧ 能正常返回链接（说明 spark 可用）；
5. 地图已用 Chunky 预生成到预期半径，且 ⟦server.properties⟧ 的视距与模拟距离是给玩家算过账的值，不是默认值。

这五条都过不了，就别急着换 fork 或调 GC——那些都是在错误层面上做优化。

## 常见坑

1. **`-Xmx` 吃满物理内存**。容器里直接触发 OOMKiller，日志里连堆栈都没有。
2. **Java 版本错配**。26.1+ 用 Java 21 起不来；1.20.x 用 Java 25 也可能出问题，按官方表走。
3. **拿 Timings 报告求助**。1.21 起默认关闭，社区基本不看。
4. **在 Folia 上装普通插件**。能启动，但区块相关功能会随机失效或抛线程异常。
5. **Purpur 配置跨版本复制**。26.2 的 `purpur.yml` 直接丢给 26.3，键名对不上时会被静默忽略。
6. **追最新构建**。164 个构建里必然有回归，生产服固定构建号并保留上一个可回滚的 jar。
7. **忘了预生成**。开服当天 TPS 曲线像心电图，却去调 GC 参数——方向从一开始就错了。

## 来源

- PaperMC Docs — Getting started（Java 版本对照表、"至少 Java 25"）：https://docs.papermc.io/paper/getting-started/ ，访问 2026-11-15
- PaperMC Docs — Aikar's flags（完整参数、内存扣减 1000–1500MB、6–10GB 建议）：https://docs.papermc.io/paper/aikars-flags/ ，访问 2026-11-15
- PaperMC Docs — Profiling（spark 内置、`--timeout 600`、Timings 弃用）：https://docs.papermc.io/paper/profiling/ ，访问 2026-11-15
- PaperMC Docs — Folia / Region logic（regionizer 与不变量）：https://docs.papermc.io/folia/ 、https://docs.papermc.io/folia/reference/region-logic/ ，访问 2026-11-15
- PaperMC Fill API（26.3 build 252、版本族、2026-09-15 首构建）：https://fill.papermc.io/v3/projects/paper ，访问 2026-11-15
- PaperMC Fill API — Folia 版本：https://fill.papermc.io/v3/projects/folia ，访问 2026-11-15
- Purpur 仓库（`ver/26.2` 与 `ver/1.21.11` 分支）：https://github.com/PurpurMC/Purpur ，访问 2026-11-15
- Chunky README（六平台支持）：https://github.com/pop4959/Chunky ，访问 2026-11-15
- Modrinth — Chunky `1.5.66`、spark `1.10.187`：https://api.modrinth.com/v2/project/chunky/version ，访问 2026-11-15

## 不确定项

- Purpur 文档站（purpurmc.org/docs）访问时发生重定向，未取到 `purpur.yml` 的具体配置键，此节只写定位不写键名。
- Folia 的插件兼容清单未逐一核实，"普通插件大概率不可用"是基于其区域线程模型的判断，不是实测结论。
- Paper 的 `view-distance` 等具体配置键未逐条核对文档正文，本文只用它举例说明方向，不给默认值。
- Aikar's flags 当前的官方页面为"解释页"，实际生成脚本指向 Script Generator，页面上未给出各参数在 26.x 上的调整说明。
- 各 fork 的实际 TPS 差异没有实测数据，本文不引用任何性能百分比。
