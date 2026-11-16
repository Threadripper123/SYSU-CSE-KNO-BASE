# MC-15 存储与自动化 2025–2026：Sophisticated、Functional Storage、Refined Storage 2、AE2 自动化

> 主题库：Minecraft 现代版本与模组（2023–2026）　｜　源文件：（纯文本条目）　｜　7,839 字符　｜　27 个片段　｜　更新于 2026-11-15
> 资料截至 2026-11-15；覆盖 2023-11 至 2026-11，重点 2024–2026。

**一句话结论**：2025–2026 的存储模组分成两层并行演进——"物理容器层"（Sophisticated Storage、Functional Storage）靠"MC 前缀 + 分支独立版本号"做到从 1.18.2 到 26.3 全线覆盖，"网络层"（Refined Storage、AE2）则在 26.x 上重新排兵：RS 用 3.x 追到 26.1.2，AE2 用与 MC 对齐的版本号追到 26.3（alpha）。对整合包作者，这意味着"容器随便升、网络要挑"——升级 MC 时先确认你依赖的是哪一层。

## 关键变化表

### 表 1：四个模组的版本落点（2026-11-15）

| 模组 | 1.20.1 线 | 1.21.1 线 | 26.x 线 | 最近更新 |
| --- | --- | --- | --- | --- |
| Sophisticated Storage | 1.5.17.2230 | 1.6.18.2229 | 26.1.2-1.5.131.2228 / 26.2-1.5.131.2232 / 26.3-1.5.132.2231 | 2026-11-12 |
| Functional Storage | 有（未逐一核对） | 有（未逐一核对） | 26.1-1.6.5a | 2026-11-15 |
| Refined Storage | 1.12.4（2023-11-06） | 2.0.9（2026-06-07） | 3.2.1（26.1.2，2026-06-07） | 2026-06-07 |
| AE2 | 15.4.12（2026-10-28） | 19.2.19（2026-11-09） | 26.1.13-beta / 26.2.1-alpha / 26.3.1-alpha | 2026-11-09 |

### 表 2：版本号规则——三条分支三个世界

| 模组 | 规则 | 示例 | 陷阱 |
| --- | --- | --- | --- |
| Sophisticated Storage | `<MC>-<modver>.<build>` | `1.21.1-1.6.18.2229`、`26.3-1.5.132.2231` | 1.21.1 线的 modver（1.6.x）比 26.x 线（1.5.x）还大 |
| Functional Storage | `<MC>-<modver>` | `26.1-1.6.5a` | 前缀写 26.1，实际对应 MC 26.1.2 |
| Refined Storage | 每条 MC 线独立 major | 2.x=1.21.1、3.x=26.x | 3.2.1 与 2.0.9 同日发布，不可比新旧 |
| AE2 | 1.21.1 用 19.x，26.x 与 MC 对齐 | `19.2.19`、`26.3.1-alpha` | 19 与 26 是两条独立线 |

### 表 3：容器与网络怎么分工

| 需求 | 合适方案 | 原因 | 不适合 |
| --- | --- | --- | --- |
| 单物品大宗（圆石、锭、粉） | Functional Storage 抽屉 | 单抽屉只存一种物品、容量大、可用控制器统一访问 | 需要混装多种物品时 |
| 多品类、带升级的通用容器 | Sophisticated Storage | 六级层级、原版木材变体、双色染色、升级槽 | 想要"终端检索"时 |
| 终端检索 + 自动合成 | Refined Storage / AE2 | 统一 Grid、样板自动合成 | 只用它做"大箱子"会浪费 |
| 26.x 上的网络存储 | AE2 26.x | RS 只到 26.1.2 | 需要 RS 的交互习惯时得等 |

## 一、Sophisticated Storage：把"箱子"做成一条产品线

Sophisticated Storage 的核心设计是"层级 + 升级槽"：Barrel、Chest、Shulker Box 都从木头起步，逐级升级到铜、铁、金、钻石、下界合金；Barrel 与 Chest 支持全部原版木材变体，还能用两种颜色染色，Barrel 可以拿其它方块的材料改外观。每升一级，容量与升级槽数量一起增加——这也是它和"单纯加一个大箱子"模组的根本区别：升级槽本身就是玩法。

Limited Barrel 是这条产品线里最实用的分支：只开 1–4 个槽位，但基础堆叠上限提高，且**升级提升的是堆叠上限而不是槽位数**。于是它天生适合"圆石/沙子/玻璃"这类单物品大宗存储——一个 Limited Barrel 顶得上一排普通箱子，还不会因为物品杂而混乱。交互上右键/左键点正面显示的物品即可存取，按住 sneak 取整组。

版本覆盖是它最强的竞争力：Modrinth 上从 1.18.2 一路列到 26.3，共 15 个 MC 版本，官方只发 Forge/NeoForge。2026-11-12 的 featured 版本同时更新了 1.20.1、1.21.1、26.1.2、26.2、26.3 五条线，这种"一次发五条"的节奏在 26.x 时代非常少见。26.3 的首个版本是 2026-09-30 的 `26.3-1.5.115.2145`，更新日志只有一句"Ported to 26.3"，之后到 `26.3-1.5.132.2231` 都是移植与修复。

需要留意的反直觉点：1.21.1 线的 modver 已经到 1.6.x，而 26.x 线还在 1.5.x。这不是"26.x 落后"，而是两条分支各自计版本：1.21.1 是长期主力线，功能更新继续堆；26.x 线是新移植，从 1.5.x 重新起算。跨线比大小必然误判。

一个容易被忽略的实用点：升级槽是"容器可编程"的前提。同样大小的箱子，升级槽多的那一级能塞进更多功能升级（拾取、磁吸、过滤、销毁、压缩、合成），而功能升级往往比容量更影响体验——一个带拾取升级的木桶，实际效率可能高于一个没升级的钻石箱。所以选层级时先问"我要装几个升级"，再问"我要多少格"。

## 二、Functional Storage：抽屉、控制器与优先级

Functional Storage 的模型是"抽屉 + 控制器"：Drawer 只存一种物品（Compacting Drawer 存"块/锭/粒"的压缩形态，Fluid Drawer 存流体），Controller 统一访问范围内所有抽屉，Controller Extension 扩大范围；再配 Puller/Pusher 升级做输入输出、Void 升级做销毁、Storage 升级扩容量。它和 Sophisticated Storage 的分工很清楚：前者解决"单物品海量"，后者解决"多品类 + 升级"。

1.5.8 是一次结构性更新，值得逐条看：新增 Armory Cabinet 的 GUI；给连接抽屉系统加了并发安全检查；优化区块加载与卸载；破坏抽屉时检查锁定状态；修复 Ender Drawer 只接受一组物品的问题；修复 Controller 插入时不检查 selector 的问题；修复"右键空抽屉会偷走你第一个物品"的老 bug；新增 `functionalstorage:armory_cabinet_insertable` 标签；补上比较器支持；加入 Puller 与 Pusher 升级互相转换的配方；给连接到 Controller 的抽屉加优先级系统；流体抽屉在物品形态下表现得像储罐。

1.6.0 修了 Compacting Drawer 槽位错位与 GUI 库存插件缺失，并新增 Pale Oak Drawer。1.6.3–1.6.5 主要是收尾：修复不该被抽出的升级、Armory Cabinet 的能力检测、抽屉清空锁定槽、流体抽屉在少量流体时的渲染；1.6.5（`26.1-1.6.5a`，2026-11-15）把 creative vending 改得更可靠、去掉 AWT 引用、在 fluid handler 里调用 `setChanged`、新增 `functionalstorage:creative_vending_denylist` 标签、修复抽屉 GUI 里升级方向改不了的问题，并补了 Pale Oak 抽屉与流体提示的简体中文翻译。

对实际产线，优先级系统是最容易被忽略的一项：当你把几十个抽屉挂在同一个 Controller 上，插入顺序会直接影响"哪个抽屉先被填满"。有了优先级，才能让"缓冲抽屉先吃、溢出抽屉后吃"，而不是让系统随机分配。

## 三、Refined Storage：二代代码库与 26.x 的追赶

Refined Storage 在 1.20.1 上停在了 1.12.4（2023-11-06）——那是老代码库的最后一版。第二代（仓库名 `refinedstorage2`）先做 1.21.1：2.0.0-beta.1（2025-03-28）→2.0.0（2025-09-27）→2.0.9（2026-06-07）；再做 26.1.2：3.0.0-beta.1（2026-04-13）→3.0.0（2026-05-14）→3.2.1（2026-06-07）。也就是说，社区口中的"RS2"是 1.21.1 线，"RS3"是 26.x 线，二者是同一套代码在不同 MC 上的两条发布分支。

有意思的是发布说明极其简短：3.0.0 只有"Ported to Minecraft 26.1.2"加一个探测器火把模型修复；2.0.0 只有"改进自动合成提示的贴图"。真正的变化藏在未发布区（Unreleased）与 3.2.x 的修复里：Grid 不活跃时仍显示置顶项、其它模组从 Interface 抽零量导致 Fabric 崩溃、Detector 在区块加载瞬间发出错误红石信号（现在要求变化稳定 1 秒才改信号）、数量界面按钮在部分语言下越界、资源种类多时 Grid 搜索变慢。

机制上 RS 仍是"无频道"路线：Grid 统一检索，Crafting Grid 合成，Controller 管理网络，Disk Drive 装 Storage Disk，Importer/Exporter 做进出，Constructor/Destructor 在世界里放置与破坏，External Storage 把外部容器接进网络，Pattern Grid 编样板、Crafter 执行自动合成，网络消耗 FE。它和 AE2 的差别不在"能不能自动合成"，而在"要不要为频道做设计"：RS 让你把精力放在产线，AE2 让你把精力放在网络结构。

## 四、AE2 自动化：样板供应器、子网与频道预算

AE2 的自动化骨架在 1.21.1 与 26.x 上没有变：样板供应器（Pattern Provider）把编码好的样板推给相邻的分子装配室或机器；接口（Interface）把网络库存暴露给外部设备，也让外部设备把物品送回网络；存储总线（Storage Bus）把箱子、抽屉、机器接进网络；等级发射器（Level Emitter）按库存量发红石信号；P2P 隧道搬运物品、流体、能量、红石甚至频道；子网把一段自动化从主网隔离，避免它吃掉主网频道。

频道是 AE2 与 RS 最大的分野：普通线缆承载 8 条频道、致密线缆 32 条，没有控制器的 ad-hoc 网络最多 8 条。一个样板供应器、一个接口、一个存储总线各占 1 条频道，所以"一个产物一套供应器 + 装配室"的工厂很容易把一条致密线吃满。成熟的基地通常这样分层：主网只放存储与终端，每个生产区做成独立子网，用接口或存储总线与主网交换成品；需要跨区搬运时用 P2P 把频道"打包"过去。

自动合成的容量由合成 CPU 多方块决定：合成单元提供任务容量，协同处理单元提供并行度，合成监控器显示进度。常见误区是只堆合成单元不看并行度——结果任务能排进去，但一次只能做一个，速度上不去。

另一个常见设计是"自动补货"（autostocking）：用等级发射器监测某种物品低于阈值就发信号，让出口总线或接口把物品送进缓冲区，再由生产区补上。它比"一直开着出口总线"省电也更可控，但阈值设得太紧会让合成任务与补货任务抢同一批原料，表现为"合成卡住不动"——这时要么给补货单独开子网，要么把阈值拉开。

26.x 上 AE2 的版本号与 MC 对齐（26.1.13-beta、26.2.1-alpha、26.3.1-alpha），1.21.1 仍是 19.2.19。对整合包作者，这意味着"26.x 上想要网络存储，目前只有 AE2 一条成熟路线"——RS 只到 26.1.2，26.2/26.3 没有对应版本。

## 五、四者怎么组合：一个可落地的分层

一个不容易翻车的组合是：**生产区用 Functional Storage 抽屉做单物品缓冲**（圆石、沙子、矿石），**车间用 Sophisticated Storage 做多品类周转箱**（带升级、可染色、便于识别），**主存储用 RS 或 AE2 做终端检索与自动合成**。三层之间用漏斗、传送带或 AE2 的存储总线连接，而不是让所有物品都涌进网络——网络的价值是"检索与合成"，不是"当仓库"。

如果目标版本是 26.x，组合要改成：Sophisticated Storage（26.3 有版本）+ Functional Storage（26.1.2 有版本）+ AE2 26.x。三者都能在 26.x 上跑，但注意 FS 的 26.x 版本只到 26.1.2，26.2/26.3 暂无。

## 六、常见坑与局限

1. **跨分支比版本号**：Sophisticated Storage 的 `1.6.18` 是 1.21.1 线，`1.5.132` 是 26.3 线，后者数字小但 MC 更新；Functional Storage 的 `26.1-1.6.5a` 前缀是 26.1，实际装 26.1.2。
2. **Refined Storage 的 1.20.1 是死路**：1.12.4 之后没有 2.x/3.x，想升级只能换 MC。
3. **RS 的 3.x 不等于"RS2 的升级版"**：它是 26.x 分支，功能集与 2.x 基本同步，缺的是 MC 覆盖而不是功能。
4. **AE2 频道算错**：把供应器、接口、存储总线都挂在一条普通线缆上，超过 8 条就会随机掉设备，表现为"机器时好时坏"。
5. **抽屉优先级**：不设优先级时插入顺序不可控，缓冲抽屉可能永远吃不饱；1.5.8 之后才有的优先级系统要主动配置。
6. **流体抽屉的物品态**：1.5.8 起流体抽屉在物品形态下像储罐，管道/漏斗的交互行为与旧版不同，老产线迁移要重测。

## 来源

- https://api.modrinth.com/v2/project/sophisticated-storage 与 https://api.modrinth.com/v2/project/sophisticated-storage/version?featured=true （SS 版本、MC 覆盖与 featured 列表，访问 2026-11-15）
- https://api.modrinth.com/v2/project/functional-storage 与 https://raw.githubusercontent.com/Buuz135/FunctionalStorage/26.1.2/CHANGELOG.md （FS 版本与 1.5.8–1.6.5 更新日志，访问 2026-11-15）
- https://api.modrinth.com/v2/project/refined-storage （RS 项目元数据与 MC 覆盖，访问 2026-11-15）
- https://raw.githubusercontent.com/refinedmods/refinedstorage2/develop/CHANGELOG.md （RS 2.0.0/3.0.0/3.2.1 与 Unreleased 条目，访问 2026-11-15）
- https://api.github.com/repos/refinedmods/refinedstorage2/tags?per_page=8 （RS 3.x tag 序列，访问 2026-11-15）
- https://api.modrinth.com/v2/project/ae2 与 https://api.github.com/repos/AppliedEnergistics/Applied-Energistics-2/releases?per_page=6 （AE2 19.2.19 与 26.x，访问 2026-11-15）
- https://guide.appliedenergistics.org/1.21.1/ （AE2 指南目录与自动化条目，访问 2026-11-15）
- https://api.modrinth.com/v2/project/create （对照 Create 6 的 1.21.1 限制，访问 2026-11-15）

## 不确定项

- Sophisticated Storage 与 Functional Storage 的 1.20.1/1.21.1 具体版本号只核对了 featured 列表与 26.x 列表，未逐条核对全部历史版本。
- Functional Storage 的升级种类（Puller/Pusher、Void、Storage 等）来自更新日志提到的名称，完整清单未取得官方文档。
- AE2 频道数值（普通 8、致密 32、无控制器 8）为长期稳定机制，本次未能打开指南正文（站点前端渲染 + read_page 429），未逐字核对。
- RS 的"二代代码库"定位依据仓库名 `refinedstorage2` 与项目起始时间，官方没有单独发布"重写说明"。
- 26.2/26.3 上 RS 是否有计划版本、FS 是否会跟进，均无公开时间表。
- Sophisticated Storage 是否存在官方 Fabric 版本未核实；Modrinth 标注的加载器只有 Forge/NeoForge。
