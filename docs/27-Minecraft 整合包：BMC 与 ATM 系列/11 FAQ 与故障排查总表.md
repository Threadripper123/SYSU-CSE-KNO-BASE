# 11 FAQ 与故障排查总表

> 主题库：Minecraft 整合包：BMC 与 ATM 系列　｜　源文件：（纯文本条目）　｜　3,467 字符　｜　32 个片段　｜　更新于 2026-11-13
# FAQ 与故障排查总表（BMC + ATM）

## A. 启动与崩溃

### A1. 启动就崩，日志里什么都没有
先查**内存分配**。ATM 官方：多数包需要 8–12GB，比例上不要超过物理内存的一半。分配过少会在加载阶段直接 OOM。

### A2. "Exit code 1" 是什么意思
**它什么都不是。** 这是 Minecraft 的通用「加载失败且不再重试」退出码，本身不携带原因。要拿到真实报错必须看启动器生成的日志（ATM Discord 里用 `?logs` 上传）。

### A3. 认证服务器连不上 / authentication servers are down
通常不是你被封。客户端在**试连会话服务器**；如果启动器里有「离线模式/演示模式」开关就先打开进游戏，等 Mojang 恢复。

### A4. 启动时 inter font 未初始化 / 文字显示异常
字体资源加载顺序问题，多出现在旧版包或改过资源包的情况下。换回默认资源包、清 `resourcepacks` 里的自定义字体包通常能解决。

### A5. 一进世界就崩，且每次都在同一位置
「ticking entity / ticking block entity」类崩溃：某个方块实体或实体在持续报错。用日志里的坐标找到它，用 `/cu entities list` 或直接编辑区块文件移除。**处理前先备份世界。**

### A6. 整合包装完没有变化 / 版本不对
BMC 的编号不是递进关系（BMC3 是 1.21.1，BMC4 反而是 1.20.1）。装之前确认「MC 版本 + 加载器」组合，而不是只看数字大小。

## B. 玩法与进度

### B1. 任务完成了却提交不了
任务界面右下角开「编辑任务」（需 OP）→ 右键该任务 → **Complete Instantly**；或右键奖励 → **Reset Progress**（还能拿奖励）。

### B2. 挖 Allthemodium/Vibranium/Unobtainium 什么都不掉
**Miner's Fervor** 附魔导致。去掉该附魔再挖。

### B3. 这些矿用机械/假玩家挖不到
设计如此。只有真实玩家能挖。替代来源：击杀监守者给 ATM Ingot 奖励。

### B4. 有了锭却做不了装备（ATM9）
缺**模板（Template）**。三件套 = 模板 + 护甲 + 锭。模板来源：Allthemodium 在主世界远古城市可疑黏土；Vibranium 在下界堡垒可疑灵魂沙；Unobtainium 在 The Other 地下图书馆箱子。

### B5. 飞行突然被禁
ATM9 的 **No Fly Zone** 在 The Other、Blue Skies、Twilight Forest 生效。改 `<世界名>/serverconfig/noflyzone.snbt` 关闭。

### B6. 找不到某个生物群系
做 **Nature's Compass**（自然指南针），右键可搜索群系并指向最近的坐标。ATM10 同样可用。

## C. 自动化与机器

### C1. RS 里的东西看不到/取不出
控制器功耗打满 **64k FE/t** 上限。改 `saves/<世界>/serverconfig/refinedstorage-server.toml` 第 36 行提高容量。

### C2. 洒水壶/收割机不工作
神秘农业默认禁止假玩家浇水。`config/mysticalagriculture-common.toml` → `fakePlayerWatering = true`。

### C3. 模块化路由器无效
在已认领区块里要给路由器插 **Security Upgrade**（自己的区块也一样）。

### C4. Chunk Destroyer 要先加载区块吗
不用。只需强加载它自己所在的那一个区块。

### C5. AE2 通道不够
- 无控制器网络最多 8 设备/8 通道 → 加 ME 控制器。
- 控制器每个面 32 通道。
- 用 **Dense 线缆**（32）、**P2P 隧道**搬通道、或拆子网。
- 跨维度用 **Quantum Ring**（32 通道、无限距离）。

### C6. AE2 存储元件塞不进东西 / Types 满了
**别移除 Types 上限**。NBT 物品（刷怪笼、附魔书、自定义工具）会迅速吃满 Type 并可能锁死存档。用 **MEGA Bulk Cell**（1 Type、无限容量）或外部存储 + Storage Bus。

### C7. 自动合成不推进 / 很慢
优先加 **Co-Processor** 而不是 Crafting Storage。一个 CPU 同时只处理 1 个请求，要并行就多造 CPU。

### C8. 样板做错了导致无限造物品
Processing 样板里**不要放可复用工具**（压印模板等），系统会每合成都造一个新的。

## D. 视觉与"灵异现象"

### D1. 准星旁的小 3D 方块
Quark 功能，**K** 键切换。

### D2. 背包格子上的黄色数字
Super Factory Manager，**Ctrl + I** 切换。

### D3. 每隔几分钟出现的幽灵
Corail Tombstone。`/tbgui` → effect → ghost 开关。**该开关只在 GUI 缩放为 4 时显示**。要永久关掉节日事件装 **BadCorailNoHolidays**（或改 `badcorailnoholidays-common.toml`）。

### D4. 屏幕像被持续伤害一样抖动，无法操作
玩家 NBT 异常。**先备份世界**，再用 NBT Explorer 改：
- 单人 `saves/<世界>/level.dat` → Data → Player → `AbsorptionAmount`=1、`Health`=1
- 多人 `world/playerdata/<UUID>.dat` 同样处理

## E. 服务器

### E1. 服务器起不来 / 秒退
检查四件事：Java 版本是否匹配 MC 版本、内存是否够、是否接受了 EULA、是否误改了 `server-ip`。**「server-ip」绝大多数情况不该填。**

### E2. 玩家下线后区块还在加载吗
FTB Chunks 有三种模式：**Never / When online / Always**。默认是「有人在时」。要挂机就改 `force_load_mode` 为 **Always**（在 `ftbchunks-world.snbt` 里）。

### E3. 服务器越跑越卡
1. `/spark tps` 看整体
2. `/spark profiler --timeout 180` 找热点
3. 检查实体数量（农场、掉落物、蜜蜂、刷怪笼）
4. 开 Crash Utilities 的 Item Clear
5. 查认领/强加载区块是否有孤儿机器在空转

### E4. 更新后配置全丢
更新时**不要整份复制 configs 文件夹**，只复制你实际改过的单个文件。跨版本整份复制会损坏安装。

## F. 选包类问题

### F1. BMC 和 Better Minecraft 是同一个吗
不是。只有发布者为 **LunaPixelStudios** 的才是 BMC 系列。

### F2. All the Mods 真的包含"所有模组"吗
不。官方明说不是，选模标准是「不 buggy、不毁性能/进度、已适配目标版本」。

### F3. BMC4 和 BMC5 该选哪个
要最稳最全 → BMC4（1.20.1 Forge）；要 1.21.1 的 NeoForge 生态 → BMC5。存档不互通（世界生成不同）。

### F4. ATM9 和 ATM10 该选哪个
查资料方便、生态成熟 → ATM9（1.20.1）。要新版本、接受部分模组尚未跟进 → ATM10（1.21.1 NeoForge）。
