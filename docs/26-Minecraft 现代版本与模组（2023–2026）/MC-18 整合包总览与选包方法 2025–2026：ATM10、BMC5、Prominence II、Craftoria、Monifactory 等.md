# MC-18 整合包总览与选包方法 2025–2026：ATM10、BMC5、Prominence II、Craftoria、Monifactory 等

> 主题库：Minecraft 现代版本与模组（2023–2026）　｜　源文件：（纯文本条目）　｜　7,930 字符　｜　36 个片段　｜　更新于 2026-11-15
> 资料截至 2026-11-15；覆盖 2023-11 至 2026-11，重点 2024–2026。

# MC-18 整合包总览与选包方法 2025–2026：ATM10、BMC5、Prominence II、Craftoria、Monifactory 等

一句话结论：截至 2026-11-15，最有代表性的五个大包全部停在 1.20.1 或 1.21.1，没有一个迁到 26.x——所以"选包"在 2026 年实际上是"选版本 + 选加载器 + 选节奏"，而不是选内容多少。

## 五个包的基本盘

| 整合包 | MC 版本 | 加载器 | 最新版本 | 发布日期 | 下载量 | 定位 |
| --- | --- | --- | --- | --- | --- | --- |
| All the Mods 10（ATM10） | 1.21.1 | NeoForge | 8.6 | 2026-11-14 | CurseForge 2308 万 | 全能：科技 + 魔法 + 任务，Extra Large |
| Craftoria | 1.21.1 | NeoForge | 1.37.0 | 2026-09-13 | CurseForge 315 万 | 500+ 模组 / 1500+ 任务，团队 TeamAOE |
| Better MC BMC5 | 1.21.1 | NeoForge | v31 | 2025-06-10 | Modrinth 33.3 万 | 冒险探索 + Boss + 优化，200+ 模组 |
| Prominence II: Hasturian Era | 1.20.1 | Fabric | v4.1.8 | 2026-11-13 | Modrinth 220 万 | RPG 剧情 + 天赋树 + 语音对话 |
| Monifactory | 1.20.1 | Forge | Beta 0.14.0 | 2026-11-12 | CurseForge 46.1 万 | GregTech CEu Modern 专家科技 |

## 更新节奏：把"最后更新"换算成天数

| 整合包 | 最后更新 | 距 2026-11-15 | 节奏判断 |
| --- | --- | --- | --- |
| ATM10 | 2026-11-14 | 1 天 | 活跃，接近每周到每两周一次 |
| Prominence II | 2026-11-13 | 2 天 | 活跃，剧情版本推进中 |
| Monifactory | 2026-11-12 | 3 天 | 活跃，但仍以 Beta 号发布 |
| Craftoria | 2026-09-13 | 63 天 | 放缓，已转向续作 |
| BMC5 | 2025-06-10 | 523 天 | 实质停更，等同 LTS |

这张表比下载量更能决定"现在入坑值不值"：ATM10 与 Prominence II 是一天一更新的类型，适合追新；BMC5 已经 523 天没有新文件，适合"我知道它是什么、我就要这个体验"的玩家；Craftoria 的 63 天则要结合它的续作一起看。

## 逐个说清楚

### ATM10：全能包的基准线

ATM10 是 2026 年事实上的"全能包基准"：CurseForge 2308 万下载、Project ID 925200、1.21.1 + NeoForge、分类里直接标 Extra Large。官方仓库（GitHub 的 AllTheMods/ATM-10）写明"All The Mods 10 1.21.1"，README 也解释了它的定位——从几个朋友的私包长成"大牌包该有的都有，但混进一些较新或较小众的模组"。

它的版本号推进很有规律：2025 年上半年还在 2.x（2.26 到 2.47 密集发了十几版），2025-05-20 跳到 3.0，2025-06-17 到 4.0，2025-11-13 到 5.0，2026-02-25 到 6.0，2026-05-15 到 7.0，2026-08-14 到 8.0，最新 8.6 在 2026-11-14。也就是说它大约每 3 个月跨一个大版本号，中间用小版本修 bug。

对玩家最重要的实际信息是：它每个版本都同步发服务端包（ServerFiles-8.6.zip 与 8.6 同日），所以"开服"这件事在 ATM10 上基本是开箱即用。

### Craftoria：任务量最大的那一档

Craftoria 由 TeamAOE 维护（Project ID 1039252），1.21.1 + NeoForge，CurseForge 315 万下载，页面自称 500+ 模组、1500+ 任务，描述正文里又写"2000+ 结构化任务"——同一页面两个数字不一致，本文按较低的 1500+ 采信，并把 2000+ 视为宣传口径。它的定位是"科技 + 魔法 + 冒险 + 建筑"四合一的巨型包，分类同样是 Extra Large。

2026 年最值得注意的不是 1.37.0（2026-09-13），而是续作 Craftoria 2 - Worlds Beyond 在 2026-08-25 上线（当时 7.33 万下载）。这意味着 Craftoria 1.x 进入了"维护模式"：63 天没更新，新内容往二代走。想玩长线存档的人要提前决定跟 1.x 还是转 2。

### BMC5：停在 1.21.1 的冒险向 LTS

Better MC BMC5 是 Luna Pixel Studios 的冒险向包，1.21.1 + NeoForge，Modrinth 33.3 万下载，v31 发布于 2025-06-10。它的特色写得很具体：完整重做的世界生成（Oh The Biomes We've Gone + Geophilic）、重做要塞与堡垒、数以百计的新地牢与结构（Moog's Voyager Structures、YUNG's 系列、Formations 系列）、新 Boss 与生物（Raided、Friends & Foes）、Waystones 与 XP Tome 这类 QoL、Xaero 小地图、可骑乘的龙、内置免费的世界分享，以及 200+ 模组。

它的价值在于"稳定"：523 天没动，意味着整合包作者早已把配方与结构调平，玩家不会因为一次更新炸存档。缺点同样明显——1.21.1 之后的原版内容（1.21.2 之后的收纳袋改动、1.21.4 苍白花园、1.21.5 的动物变种、1.21.6 的恶魂坐骑、1.21.9 的铜器时代）它一个都没有。

### Prominence II：RPG 剧情包的另一条路

Prominence II: Hasturian Era 是 Fabric 阵营的代表：1.20.1 + Fabric，Modrinth 220 万下载、1528 关注，v4.1.8 发布于 2026-11-13。它卖的不是模组数量而是自制内容——两条随进度解锁的剧情线、随时可回的首都 Vaaz（NPC、秘密、会随任务推进演化）、自制天赋树（10 种命运、16+ 被动、5+ 主动、100+ 属性天赋）、自制等级与怪物缩放、带配音的对话系统、幻化（Transmogrify）、Primordial Forge 终局护甲、Mythic Challenges 终局 Boss。

v4.0 的名字是"Kingdom of Vaaz"，核心就是那座首都。这类包的特点是"版本号含义完全不同"：它的 v4.x 是内容版本，不是"第 4 次重写"。

### Monifactory：GregTech 专家的现代重制

Monifactory 是 Nomifactory CEu 的重制与移植，底层换成 GregTech CEu Modern，1.20.1 + Forge，CurseForge 46.1 万下载，最新是 Beta 0.14.0（2026-11-12），Project ID 1004602。

它的改动点很明确：重做 UV 之后的进度、用 Sculk 相关机制替换 Draconic Evolution 与 Avaritia、为老 Nomi CEu Hard Mode 玩家准备 Hard/Expert 模式、全套贴图现代化、可选兼容 Create 与 XNet。它还有一个自研 coremod（MoniLabs）。

需要注意"Beta"这个词在这里的语义：它已经连续发布了两年 Beta 号（0.x），社区照常玩，但版本号不承诺兼容。对整合包/服务器管理员来说，这意味着升级前必须读 changelog，而不是看"0.13 → 0.14 只是小版本"。

## 选包方法：六维打分 + 五个检查动作

| 维度 | 权重建议 | 怎么快速判断 |
| --- | --- | --- |
| 版本与加载器 | 高 | 先定 1.20.1 / 1.21.1 / 26.x，再定 Forge / NeoForge / Fabric |
| 主题匹配 | 高 | 科技、魔法、冒险、RPG 剧情、专家科技，只能选一个主 theme |
| 难度与引导 | 中高 | 有没有任务书、是否 Expert、是否要求看 wiki |
| 更新节奏 | 中 | 看"最后更新"换算成天数，再决定是追新还是求稳 |
| 服务器与多人 | 中 | 有没有同步服务端包、是否支持内置世界分享 |
| 硬件门槛 | 中 | Extra Large 类包按 8–12GB 客户端内存起算 |

五个检查动作，按顺序做：

1. 打开 CurseForge/Modrinth 项目页，先看 Game Versions 与 Mod Loaders 两栏。这两栏不匹配，后面全白搭。
2. 看"最后更新"和文件列表日期是否一致。有的包页面显示"几天前更新"，但主文件还是几个月前的。
3. 打开 changelog 或 GitHub 仓库，看最近三个版本是"修 bug"还是"加内容"。只修 bug 的包适合长期存档。
4. 搜一下它的 issue 区或 GitHub issues，看有没有"启动崩溃/存档损坏"的高赞问题还没关。
5. 确认服务端包是否同步发布。ATM10 与 Craftoria 都有同日服务端文件；只发客户端的包要自己拼服务端。

## 硬件与性能预期

按 CurseForge 的分类，ATM10 与 Craftoria 都属于 Extra Large，实际体验的门槛大致是：

- 客户端：8GB 只是"能进"，12GB 才是"能玩"，16GB 才有余量开光影与高渲染距离。
- 服务端：ATM10 这类包建议 12–16GB 堆内存，并且一定要预生成世界；不预生成的话，玩家跑图时的区块生成会把 tick 拖到个位数。
- BMC5 与 Prominence II 相对轻：BMC5 自带优化与光影支持，Prominence II 是 Fabric 阵营、模组数量少，6–8GB 客户端通常够用。
- Monifactory 的内存需求不高，但 GregTech 的机器逻辑密集，CPU 单核性能比内存更重要。

一个反直觉的经验：这五个包里"最吃配置"的往往不是模组最多的 ATM10，而是玩家自己在 ATM10 里搭的自动化基地。选包时估算硬件，要按"你打算玩到什么程度"而不是"包有多大"。

## 服务器与多人

- ATM10：每个版本同步发 ServerFiles zip，NeoForge 1.21.1，社区文档最全，适合第一次开服。
- Craftoria：有独立 Server 包（1.37.0 同日），适合小团队；任务量大，进度同步要提前约定。
- BMC5：宣传"内置免费世界分享"，对不想自己开服的玩家最友好；但 1.21.1 的版本意味着部分新原版内容缺失。
- Prominence II：Fabric，自带多人支持与怪物缩放，适合 2–4 人剧情通关；剧情包不适合大服。
- Monifactory：GregTech 包天然适合小团队分工（采矿/电力/化工），但学习曲线陡，服务器要配好备份。

## 常见坑

- 把 1.21.1 的包塞进 1.20.1 客户端：报错通常是"mod 需要更高版本"，而不是包不兼容，容易误判。
- 只看下载量选包：BMC5 的 523 天停更不会体现在下载量上，但会体现在"你玩到后期发现没有新内容"。
- 把 Craftoria 与 Craftoria 2 混为一谈：二代是独立项目（2026-08-25），存档与任务线不通用。
- Monifactory 的 Beta 号当成"不稳定"或"稳定"都不对：它稳定到可以长期玩，但不承诺跨版本兼容。
- 追 ATM10 的每个小版本：它的 8.x 内部小版本主要修 bug，跟着升没问题；跨大版本（7→8）要备份存档。
- 用 CurseForge 页面上的"Created 2 years ago"判断新旧：那是项目创建时间，不是版本时间。

## 谁适合哪个包：受众匹配表

| 整合包 | 最适合 | 不太适合 |
| --- | --- | --- |
| ATM10 | 想一次装齐科技+魔法+任务、并且打算开服的人 | 想要强剧情驱动、或机器性能吃紧的人 |
| Craftoria | 喜欢"任务书带着走"、想慢慢解锁几百个模组的人 | 讨厌任务墙、只想自由发挥的人 |
| BMC5 | 想玩冒险探索、要稳定不折腾、喜欢结构与 Boss 的人 | 追新版本、需要 1.21.2 之后原版内容的人 |
| Prominence II | 想单人/小队通关一条 RPG 剧情线的人 | 想要大服、或不喜欢被剧情与等级限制的人 |
| Monifactory | 玩过 GT 或 Nomi、想要现代重制与专家难度的人 | 第一次接触 GregTech、或只想轻松建造的人 |

## 分发渠道：同一份包，三个入口

这五个包大多同时存在于 CurseForge 与 Modrinth，但"存在"不等于"同步"：

- CurseForge 是 ATM10、Craftoria、Monifactory 的主发布地（Project ID 分别是 925200、1039252、1004602），文件最全、服务端包也在这里。
- Modrinth 是 BMC5 与 Prominence II 的主发布地（BMC5 有 "Full parity with Curseforge Version" 的说明，Prominence 的页面直接引用 forgecdn 的图床）。
- 第三方启动器（Prism、Modrinth App、CurseForge App）本质上只是这三个入口的客户端；同一个包在不同启动器里可能显示不同版本号，原因是索引延迟，而不是包不同。

选包时的实用判断是：如果 CurseForge 与 Modrinth 上同名的包版本号差得远，以文件更新更频繁的那一侧为准，并在下载前确认它的服务端包是否同日发布。

## ATM10 的版本密度：一个可量化的例子

把 ATM10 的 changelog 条目按时间排开，从 2025-01-22 的 2.26 到 2026-11-14 的 8.6，一共 58 个版本，跨度约 22 个月，平均每月 2.6 个版本。其中 2025 年上半年最密（2.26 到 2.47 之间塞了十几个），2026 年稳定在每月 1–3 个。

这个密度对玩家是好事（bug 修得快），对整合包作者是负担（要跟版本重做 KubeJS 脚本）。它也是判断"这个包是否还活着"的最直接指标：月更两次以上是活跃，一季度一次是维护，一年不动就是停更。

## 关于 26.x：现在该不该等

这是 2026 年下半年最常被问的问题。把事实摆齐：五个主流包里最新的是 ATM10 8.6（2026-11-14），它仍然在 1.21.1；26.1（2026-03-24）已经发布了八个月，26.2（2026-06-16）、26.3（2026-09-15）也先后上线，但没有任何一个同量级的大包迁过去。

原因不神秘：26.1 起最低要求 Java SE 25，服务端镜像、启动器参数、托管面板都要跟着换；而大包的模组数量意味着"等八成模组都发了 26.x 版本"才敢动。按 1.21 的历史节奏（2024-06 发布，主流包在 2024 年底到 2025 年初才稳定），26.x 的大包大概率要等到 2027 年上半年。

所以 2026 年底的合理策略是：想现在玩，选 1.20.1 或 1.21.1；想等 26.x，先用手上的 1.21.1 存档过渡，别为了"版本新"去装半成品包。

## 来源

- https://www.curseforge.com/minecraft/modpacks/all-the-mods-10（访问 2026-11-15）
- https://github.com/AllTheMods/ATM-10 的 README.md 与 CHANGELOG.md（访问 2026-11-15）
- https://www.curseforge.com/minecraft/modpacks/craftoria（访问 2026-11-15）
- https://api.modrinth.com/v2/project/better-mc-neoforge-bmc5 与 /version?featured=true（访问 2026-11-15）
- https://api.modrinth.com/v2/project/prominence-2-fabric 与 /version?featured=true（访问 2026-11-15）
- https://www.curseforge.com/minecraft/modpacks/monifactory（访问 2026-11-15）
- https://github.com/Omicron-Industries/Monifactory 的 README.md 与 CHANGELOG.md（访问 2026-11-15）

## 不确定项

- ATM10 与 Craftoria 的模组总数没有在一手页面拿到确切数字（页面只给 Extra Large 分类与"500+ 模组"），文中不给具体条数。
- Craftoria 页面同时出现"1500+ 任务"与"2000+ 任务"，未核实哪个是当前值。
- Monifactory 的 MC 版本写作 1.20.1（CurseForge Game Versions 栏），但其 README 未明确写版本号，以页面为准。
- 26.x 上是否已有同量级的大型整合包，未做全量扫描；本文只断言"这五个都没上 26.x"。
- 各包的推荐内存是基于分类与模组规模的估计，不是官方最低配置。
