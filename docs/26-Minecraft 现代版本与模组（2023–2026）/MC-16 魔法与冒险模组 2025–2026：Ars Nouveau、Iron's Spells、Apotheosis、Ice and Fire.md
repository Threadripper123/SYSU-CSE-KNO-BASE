# MC-16 魔法与冒险模组 2025–2026：Ars Nouveau、Iron's Spells、Apotheosis、Ice and Fire

> 主题库：Minecraft 现代版本与模组（2023–2026）　｜　源文件：（纯文本条目）　｜　9,084 字符　｜　37 个片段　｜　更新于 2026-11-15
> 资料截至 2026-11-15；覆盖 2023-11 至 2026-11，重点 2024–2026。

# MC-16 魔法与冒险模组 2025–2026：Ars Nouveau、Iron's Spells、Apotheosis、Ice and Fire

一句话结论：2026 年的魔法/冒险模组已经分成三层——仍守在 1.21.1 的活跃双雄（Ars Nouveau、Iron's Spells）、率先跨进 26.x 的数据驱动派（Apotheosis）、以及停在 1.20.1 不再前进的遗产巨兽（Ice and Fire）。选模组时先定游戏版本，再看加载器，最后才比较内容量；顺序反了就会做出装不上的整合包。

## 关键变化表：四个模组的版本现状

| 模组 | 最新版本 | 发布日期 | 声明支持的游戏版本 | 加载器 | 体量参考 |
| --- | --- | --- | --- | --- | --- |
| Ars Nouveau | 5.13.5 | 2026-11-09 | 1.21 / 1.21.1（1.20.1 停在 4.6.0） | NeoForge | Modrinth 405 万下载 |
| Iron's Spells 'n Spellbooks | 1.21.1-3.16.3 | 2026-08-18 | 1.21.1；同时维护 1.20.1-3.16.3 | NeoForge（1.21.1）/ Forge（1.20.1） | Modrinth 609 万、CurseForge 7246 万下载 |
| Apotheosis | 26.1.2-9.1.0 | 2026-10-04 | 26.1.2；1.21.1-8.9.0；1.20.1-7.4.8 | NeoForge（1.21+）/ Forge（1.20.1） | Modrinth 39.7 万（2026-06 才建页） |
| Ice and Fire: Dragons | 2.1.13-1.20.1-beta-5 | 2024-08-15 | 最高 1.20.1 | Forge / NeoForge | CurseForge 1.0996 亿下载 |

## 第二条时间线：同一条版本轴上谁在哪

| 版本轴 | Ars Nouveau | Iron's Spells | Apotheosis | Ice and Fire |
| --- | --- | --- | --- | --- |
| 1.20.1 | 4.6.0（2023-10） | 3.16.3（2026-08-18） | 7.4.8（2026-06-19） | 2.1.13 beta-5（2024-08-15） |
| 1.21.1 | 5.13.5（2026-11-09） | 3.16.3（2026-08-18） | 8.9.0（2026-09-27） | 无 |
| 26.1.2 | 无 | 无 | 9.1.0（2026-10-04） | 无 |

这张表比任何宣传语都直接：想在 26.1.2 上开一个魔法包，目前只有 Apotheosis 一条路；想在 1.21.1 上组"法术 + 词缀装备"，三件套齐全；想留在 1.20.1 且必须要有龙，只有 Ice and Fire 这一条老路，而且它已经两年没有新文件。

## Ars Nouveau：把法术拆成"字形序列"

Ars Nouveau 的核心不是给你一堆固定法术，而是给一套拼装语法。玩家先做一本 Novice's Spell Book（书 + 1 个青金石），之后每个法术都是一串字形的组合，施法消耗 Mana；左下角法力条会随时间回复，每学会一个新字形都会永久提升法力上限与回复速度，升级法术书、穿魔法装备、喝法力药水也能叠加这些加成。

三个制造台构成中期的骨架：

- Imbuement Chamber：把紫水晶碎片或青金石转化为 Source Gem；在周围三个 Arcane Pedestal 上放指定物品，可以把 Source Gem 进一步注魔成各种 Essence，而 Essence 是大量字形的合成材料。
- Enchanting Apparatus：放在 Arcane Core 上方，用来做 Magebloom 种子、魔法装备、召唤护符，也可以直接给装备指定附魔。它是"魔法版附魔台 + 工作台"，也是整个模组最容易堵管道的一环。
- Alteration Table：把不同 Threads 装进魔法护甲槽位，换取被动属性。

Source 是第二套资源，和 Mana 完全独立。Sourcelink 负责从世界里"抽取"Source 注入 Source Jar，早期最实用的是 Agronomic Sourcelink（靠作物）与 Mycelial Sourcelink（靠菌类/蘑菇）；后期有 Alchemical、Volcanic、Vitalic、Fluid 等变体。Source Relay 有 Collector / Depositor / Splitter / Warper 四种，负责把 Source 在罐子之间远距离搬运。Source 用来加速 Imbuement Chamber、驱动设备、供养魔法仆从、开启传送门，也用于高阶 Enchanting Apparatus 配方。

自动化由三类东西完成：Spell Turret（把法术变成炮塔，用于刷怪、耕作、采掘）、Rituals（范围仪式）、Familiars（Starbuncle 搬物品、Whirlisprig 催熟作物、Drygmy 产生生物掉落、Wixie 处理流体与酿造，另有 Bookwyrm、Amethyst Golem 等）。2026 年这条线的一个现实约束是：5.x 只发 1.21.1 与 NeoForge，1.20.1 的最后一个是 4.6.0（2023-10-06）。所以"Ars Nouveau 5.13 装在 1.20.1 上"不是配置问题，是根本不存在这个文件。

从发布节奏看，5.12.0（2026-06-17）到 5.13.5（2026-11-09）约五个月走了六个小版本，属于"每月有更新、但不保证每周"的健康维护状态，适合长期存档。

## Iron's Spells：RPG 法术书 + 结构驱动的世界

Iron's Spells 走的是另一条路：它不让你拼法术，而是让你"收集法术书页"。模组按学派组织内容，每个学派有一个代表性的 Focus 物品作为合成材料：

| 学派 | Focus | 主题 |
| --- | --- | --- |
| Fire | Blaze Rod | 直接伤害与持续伤害 |
| Ice | Frozen Bone | 伤害与群体控制 |
| Lightning | Bottle o' Lightning | 集中爆发伤害 |
| Holy | Divine Pearl | 治疗、增益、辅助 |
| Ender | Ender Pearl | 伤害、功能、位移 |
| Blood | Blood Vial | 亡灵/凋零/血魔法，伤害同时强化自身 |
| Evocation | Emerald | 万金油：攻防与功能兼备 |
| Nature | Poisonous Potato | 上减益，再用伤害收尾 |
| Eldritch | Echo Shard（学会后才可用） | 远古魔法，不能直接合成 |

进程主干很短但很清楚：早期靠结构战利品拿卷轴，把卷轴带到 Inscription Table 抄进法术书；法术书槽位有限、也没有属性。进入下界资源阶段后解锁 Scroll Forge——墨水决定卷轴稀有度（也就是法术等级），Focus 决定学派。中期用下界资源做法术书（例如 Apprentice's Spell Book 用金），用 Arcane Cloth 与 Arcane Essence 做更强的法师护甲。Eldritch 学派单独一条线：Ancient Knowledge Fragment 加 Echo Shard 拼成 Eldritch Manuscript，读一次学一个法术。

规模上，官方口径是 100+ 可升级法术、2 个施法 Boss、9 种施法者敌人/盟友/商人、12 套法师护甲、11 种随机结构，另有武器注魔与护甲升级系统，以及细到单个法术的配置项。原生兼容 JEI/JEED、Apotheosis、Tetra、Better Combat、Patchouli。

作者对版本策略说得非常直白：不做多版本移植，"开发一个版本要花几个月"。所以 1.20.1 与 1.21.1 是两条并行线，3.16.x 在同一天（2026-08-18）给两边都发了 3.16.3——这是"双线同版"的典型维护方式，整合包作者可以据此判断它短期不会跳到 26.x。

## Apotheosis：模块化 + 世界层级，第一个吃到 26.x 的冒险大模组

Apotheosis 在 1.20.1 之后被拆成四个模块，各自有独立的 CurseForge 页面：

- Apothic Attributes：属性与药水效果底座，其他模块都依赖它。
- Apothic Spawners：刷怪笼的拾取与改造。
- Apothic Enchanting：重做附魔系统，并加入大量新附魔，是第二大模块。
- Apotheosis（adventure 模块）：词缀、宝石、世界层级这些"RPG 味道"的部分。

冒险模块的循环是：打怪/开箱拿到带词缀（Affix）的物品 → 在 Salvaging Table 拆成 Rarity Materials → 在 Reforging Table 用材料 + 宝石重铸出自己要的词缀物品。词缀物品可能带 Sockets，可以镶 Gem；拔宝石要 Sigil of Withdrawal，给没孔的物品开孔要 Sigil of Socketing；最后用 Augmenting Table 花大量经验 + Sigil of Enhancement 单词缀重 roll，Sigil of Malice 则是随机改写整件物品的"赌博"道具。

世界层级（World Tier）是它把难度和掉落绑在一起的机制，按 CTRL+T 打开选择界面，共五级：Haven → Frontier → Ascent → Summit → Pinnacle。Summit 需要先击杀凋灵才开放，Pinnacle 是神话物品的主要来源。层级越高，掉落越好、自身有额外加成，但所有敌人同步变强；进入 Frontier 后会开始刷 Apothic Invader（自带词缀、必掉词缀物品）与 Apothic Elite，另外还有 Augmentations 给任意生物随机加 buff。

版本上最值得注意的是：Apotheosis 9.x 已经发到 26.1.2。Modrinth 页面显示 26.1.2-9.0.3 在 2026-07-24、26.1.2-9.1.0 在 2026-10-04，1.21.1 线同时维护到 8.9.0（2026-09-27），1.20.1 线停在 7.4.8（2026-06-19）。换句话说，它是本组四个模组里唯一明确跨进"年份版本号"时代的，这也解释了为什么 2026 年下半年新开的 26.x 冒险包几乎都会带它。

对开发者来说，Apotheosis 本质是一台数据包引擎：词缀、宝石、孔位、层级增益全部 JSON 化，schema 在 GitHub 的 1.21 分支；CurseForge 的 Additional Files 里能下到每个版本对应的 Stock Datapack，改数值不用写 Java。硬依赖包括 Placebo、Patchouli、Apothic Attributes、Apothic Spawners、Apothic Enchanting——少装 Patchouli 不会崩，但游戏内的 Chronicle of Shadows 手册会消失。

## Ice and Fire：内容仍在，但时间停在 2024

Ice and Fire 的数据很矛盾：CurseForge 累计 1.0996 亿次下载，属于历史级模组；但页面显示最后更新 2024-08-15，最新文件是 1.20.1 的 beta-5，而且标记是 Beta 而不是 Release，游戏版本列表最高只到 1.20.1。

它的内容量并不小：火龙/冰龙/雷龙三种龙，各有 roost/den 结构；可以找龙蛋、驯服、骑乘；有龙钢装备；还有一整套希腊/中世纪神话生物——九头蛇、独眼巨人、死亡蠕虫、石化鸡蛇（gorgon 能把僵尸石化）、斯廷法利斯鸟、小精灵、恐惧巫妖（dread lich）与它的陵墓。Lectern 方块用来在游戏内查资料。

问题在于"停在 beta"这件事对整合包是硬伤：beta 版本通常意味着作者自己没打算把它当稳定版，而 1.21/26.x 玩家只能等社区移植或换替代品。因此 2026 年的正确用法是：把它当 1.20.1 老包的怀旧内容，而不是新包的默认选项。

## 对玩家、整合包与开发者的实际影响

给玩家的版本决策树其实只有三条：

1. 存档在 1.20.1（Forge）：Iron's 3.16.3 + Apotheosis 7.4.8 + Ice and Fire 2.1.13 beta-5 是唯一齐全的组合，Ars Nouveau 只能用 4.6.0。
2. 存档在 1.21.1（NeoForge）：Ars Nouveau 5.13.5 + Iron's 3.16.3 + Apotheosis 8.9.0，这是目前内容最完整、更新最勤的一条线。
3. 存档在 26.1.2：目前只有 Apotheosis 9.1.0 可选；其余三个要么等，要么用替代品。

给整合包作者的三条经验：第一，法术类模组互相叠加时，最容易被忽略的是"法术获取途径重复"——Ars 靠字形研究、Iron's 靠卷轴与结构，两条线同时给玩家会导致前期强度曲线塌陷，通常要把其中一条的战利品表下调。第二，Apotheosis 的词缀是乘算式增强，和其它 RPG mod 的百分比加成叠在一起很容易出现"一件装备打穿整个包"，进包前先决定 World Tier 上限。第三，Ice and Fire 的 beta 标签意味着要固定客户端与服务端同一个 jar，任何一边自动更新都会握手失败。

给开发者的提示：Apotheosis 的 JSON schema 是最值得抄的工程设计——把"词缀池/权重/稀有度/孔位规则"全部外置，服务器管理员改一个 datapack 就能重做经济；Iron's 的 per-spell 配置适合做"禁用某个法术"的服务器策略；Ars 的字形系统则适合做教学型任务书（按字形解锁排任务）。

## 常见坑

- Optifine 会让 Iron's Spells 的模型渲染错乱，作者在 FAQ 里直接点名"就是它"，换 Oculus 之类的替代品。
- Ars Nouveau 5.x 没有 1.20.1 版本，把 1.21.1 的 jar 丢进 1.20.1 实例只会在启动时崩，不会给你友好提示。
- Apotheosis 26.1.2 分支属于 26.x 生态，而 26.1 起最低要求 Java SE 25；用 Java 21 启动会直接失败，这一点在升级整合包时最常被忽略。
- Ice and Fire 的 1.20.1 最新文件是 beta-5，别把 beta-4 与 beta-5 的客户端/服务端混用。
- Apotheosis 的 Chronicle of Shadows 需要 Patchouli；很多"轻量包"为了省 mod 把它删了，结果玩家完全看不到词缀说明。
- Iron's 的结构战利品与 Terralith/其它世界生成 mod 共存时，结构密度会异常，需要在结构配置里调 spacing/separation。

## 成本画像：四个模组进包前该看什么

| 模组 | 常驻 tick 压力的主要来源 | 启动与内存 | 外置配置程度 | 进包前必查 |
| --- | --- | --- | --- | --- |
| Ars Nouveau | Spell Turret、Familiar 的寻路与掉落判定、Sourcelink 的周期性扫描 | 中等；字形与法术 NBT 会随存档增长 | 高：字形、仪式、饰品多为数据驱动 | 炮塔密度、Source 产出是否被任务书卡住 |
| Iron's Spells | 结构生成、施法者 AI、卷轴/法术冷却 | 中偏高；结构与战利品表条目多 | 中高：可按单个法术配置 | 结构 spacing、与其它世界生成 mod 的冲突 |
| Apotheosis | Invader/Elite 的生成判定、词缀属性重算、刷怪笼改造 | 中；数据包条目多但常驻逻辑轻 | 极高：词缀/宝石/层级全 JSON | World Tier 上限、与其它 RPG 模组的百分比叠加 |
| Ice and Fire | 龙 AI 与飞行寻路、巢穴结构、神话生物生成 | 偏高；龙实体与巢穴方块状态多 | 低：以配置文件为主 | 客户端/服务端 jar 必须同版本 |

一个常被低估的点是"法术模组的成本不在法术，而在实体与方块实体"。Ars 的炮塔、Iron's 的施法者、Apotheosis 的 Invader 都是需要每 tick 决策的实体；把它们密集放在出生点周围，比多装十个装饰模组更伤 TPS。做包时的经验做法是：把 Ars 的自动化集中到一个"魔法工业区"并限制区块加载，把 Iron's 的结构密度调到默认值的 60%–80%，Apotheosis 则通过层级解锁把 Invader 出现时间推后到玩家有像样装备之后。

## 2024→2026 的三次转折（只讲对这四个模组的影响）

第一次是 1.20.5/1.20.6 的物品组件化。它没有直接改变这四个模组的内容，但改变了它们"怎么做兼容"：以前靠 NBT 拼装备属性的写法在 1.21 之后必须改成组件，于是 1.21.1 分支普遍比 1.20.1 分支晚半年才稳定，这也是 Ars 与 Iron's 都出现"双线并行"的结构性原因。

第二次是 NeoForge 从 Forge 分家后形成的加载器割裂。1.21.1 上 Ars 只发 NeoForge、Iron's 只发 NeoForge、Apotheosis 只发 NeoForge；到了 1.20.1 又全部退回 Forge。对整合包作者而言，这意味着"1.21.1 + Forge"这条组合基本没有魔法线可选。

第三次是 26.x 的年份版本号与 Java 25 门槛。它把"要不要跟进"变成一次真实的成本决策：跟进意味着玩家群体、服务端镜像、启动器参数都要重做；不跟进则意味着留在 1.21.1。Apotheosis 选择跟进，另外三个选择观望，这就是 2026 年魔法模组生态最真实的分歧。

## 来源

- https://api.modrinth.com/v2/project/ars-nouveau 与 /version（访问 2026-11-15）
- https://api.modrinth.com/v2/project/irons-spells-n-spellbooks 与 /version（访问 2026-11-15）
- https://api.modrinth.com/v2/project/apotheosis 与 /version（访问 2026-11-15）
- https://www.curseforge.com/minecraft/mc-mods/ice-and-fire-dragons（访问 2026-11-15）
- https://www.curseforge.com/minecraft/mc-mods/irons-spells-n-spellbooks（访问 2026-11-15）
- https://ars.guide/book/getting_started/spell_casting/ 、/spell_mana/ 、/source/ 、/magical_crafting/ 、/magical_automation/（访问 2026-11-15）
- https://github.com/iron431/Irons-Spellbooks-Docs 的 _tabs/schools.md 与 _tabs/progression.md（访问 2026-11-15）
- https://api.modrinth.com/v2/project/apotheosis 描述正文（含模块拆分、World Tier、Reforging/Augmenting 细节，访问 2026-11-15）

## 不确定项

- Apotheosis 26.1.2-9.1.0 相对 8.9.0 的具体改动没有逐条读 changelog，只确认了版本号、日期与支持版本。
- Ars Nouveau 5.12→5.13 的 changelog 未读，文中"每月有更新"是按发布日期推断的节奏，不是官方承诺。
- Iron's Spells 3.16.x 的具体新增法术/结构未核实，规模数字取自官方 README 的固定表述。
- Ice and Fire 是否存在可用的 1.21+ 社区移植分支未核实，本文不推荐任何未验证的 fork。
- 26.x 上除 Apotheosis 外是否还有同级别魔法模组已迁移，未做全量扫描。
