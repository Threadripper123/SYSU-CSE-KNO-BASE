# 00 总览：BMC 与 ATM 怎么选、怎么区分

> 主题库：Minecraft 整合包：BMC 与 ATM 系列　｜　源文件：（纯文本条目）　｜　4,188 字符　｜　13 个片段　｜　更新于 2026-11-13
# 总览：BMC 与 ATM 怎么选、怎么区分

> 数据截止 2026-11-13。版本号与下载量会持续变化，引用时请以本文档标注的抓取日期为准，或回查 CurseForge / Modrinth 原页。

## 1. 这份知识库是什么

本库覆盖两套**互不隶属**的 Minecraft Java 版整合包（modpack）家族：

| 家族 | 作者/团队 | 定位 | 一句话特征 |
|---|---|---|---|
| Better MC（BMC） | LunaPixelStudios（主开发者 SHXRKIE） | Vanilla+（在原版框架上扩展） | 保留原版节奏，加维度、Boss、任务，装完像"更大的原版" |
| All the Mods（ATM） | ATMTeam | Kitchen-sink（全能科技+魔法） | 几百个模组、任务书驱动、以造出 ATM Star 为终局 |

两者不是同类产品，几乎不存在"哪个更好"，只有"要哪种体验"。

## 2. 一眼选包

想清楚三件事就能定下来：**你要的 MC 版本、你能接受的渲染/加载器生态、以及你想玩"扩展原版"还是"科技全能"**。

- 想要最稳、内容最全的 BMC：**BMC4**（1.20.1 Forge，19.0M 下载，旗舰）
- 想要 1.21.1 又离不开 Forge 系生态：**BMC5**（1.21.1 NeoForge）
- 机器一般、只想流畅：**BMC2 / BMC3**（Fabric，加载更快、更轻）
- 有老存档情结或低配：**BMC1**（1.16.5 Forge）
- 想玩科技/魔法全能、有任务书和长线终局：**ATM10**（1.21.1 NeoForge，约 500 模组）或 **ATM9**（1.20.1 Forge，生态最成熟、攻略最多）
- 只想要 ATM 的框架但硬盘/内存紧张：**ATM9 - No Frills**（去掉大量花哨内容，保留同一终局）

## 3. BMC 版本矩阵

| 版本 | Minecraft | 加载器 | CurseForge 项目 ID | 下载量 | 最新文件 | 模组规模 |
|---|---|---|---|---|---|---|
| BMC1 | 1.16.5 | Forge | 429793 | 5.8M（CF） | v62，2025-06-22 | 210+ |
| BMC2 | 1.20.1 | Fabric | 873389 | 2.1M（CF）/ 3.70M（Modrinth） | v40，2026-01-24 | 280+ |
| BMC3 | 1.21.1 | Fabric | 826731 | 1.5M（CF）/ 390K（Modrinth） | v45，2026-05-07 | 330+（CF 自述）/ 160+（Modrinth 自述） |
| **BMC4** | **1.20.1** | **Forge** | **876781** | **19.0M（CF）** | **v63 HF，2026-11-11** | 270+（Modrinth）/ 433（Minecraft-Guides 统计） |
| BMC5 | 1.21.1 | NeoForge | 462042 | 3.1M（CF） | v54，2026-10-29 | 320+（CF）/ 200+（Modrinth） |
| BMC Skyblock | 1.20.1 | Forge | — | 30K（Modrinth） | — | 空岛版 |

**关键认知：BMC1–BMC5 的编号不是版本递进关系**，每个编号锁定一套「MC 版本 + 加载器」组合，互不覆盖。装错编号不是"降级"，而是换了一个世界生成体系，旧存档无法延续。

## 4. ATM 版本矩阵

| 版本 | Minecraft | 加载器 | CurseForge 项目 ID | 下载量 | 最新文件 |
|---|---|---|---|---|---|
| ATM0 | — | — | 372309 | — | 早期私有包 |
| ATM1 | 1.10.2 | Forge | 242462 | 923.7K | 2018-10-26 |
| ATM2 | **1.11.2** | Forge | 253707 | 253.6K | v1.0a，2017-09-26 |
| ATM3 | 1.12.2 | Forge | 269708 | 3.1M | 2022-06-08 |
| ATM3 Remix | 1.12.2 | Forge | 301845 | 516.5K | 2.0.0，2021-01-09 |
| ATM3 Expert | 1.12.2 | Forge | 325396 | — | — |
| ATM4 | 1.14.4 | Forge | 316059 | 674.8K | 2020-07-02 |
| ATM5 | 1.15.2 | Forge | 357494 | 693.9K | 2021-12-10 |
| ATM6 | 1.16.5 | Forge | 381671 | 6.4M | 1.9.2，2024-01-24 |
| ATM7 | 1.18.2 | Forge | 426926 | 3.0M | 1.0.4，2024-03-21 |
| ATM8 | 1.19.2 | Forge | 520914 | 9.3M | 1.1.0，2023-12-17（350+ 模组） |
| **ATM9** | **1.20.1** | **Forge** | **715572** | **13.6M** | **1.1.1，2025-10-12**（400+ 模组） |
| ATM9 No Frills | 1.20.1 | Forge | 959010 | 1.2M | 0.2，2025-04-24 |
| **ATM10** | **1.21.1** | **NeoForge** | **925200** | **23.0M** | **8.5，2026-11-04**（约 500 模组） |
| ATM10 To the Sky | 1.21.1 | NeoForge | 1298402 | 3.7M | 2.3.1，2026-10-30 |

**注意 ATM2 是 1.11.2，不是 1.12.2**——这是最容易记错的一条，实测原页为 1.11.2。ATM3 才是 1.12.2 主力。

## 5. 三个必须避开的名字陷阱

1. **"Better MC" ≠ "Better Minecraft"**。Modrinth 上存在多个同名/近名项目（例如 CubingXT 的 "Better MC"、"Better MC [PACK]"、"Better MC LITE"、"Better MC: Hunter's Edition" 等，下载量从几千到一万多不等）。**认准 CurseForge/Modrinth 发布者为 LunaPixelStudios（Modrinth 显示为 SHXRKIE 上传）。**
2. **BMC 编号不等于版本高低**。BMC3 是 1.21.1 Fabric，BMC4 反而是 1.20.1 Forge；BMC5 才又回到 1.21.1。
3. **"All the Mods" 并没有包含所有模组**。官方自己在每个包描述里都写"Does All the Mods really contain ALL THE MODS? No, of course not."，选模标准是：不 buggy、不毁性能/进度、已适配目标 MC 版本。

## 6. 本库文档索引

| 文档 | 内容 |
|---|---|
| 01 BMC 系列总览 | 定位、加载器差异、配置要求、通用玩法骨架 |
| 02 BMC 各版本档案 | BMC1–BMC5 + Skyblock 逐版细节与特色模组 |
| 03 BMC 维度与 Boss | 维度进入方式、Boss 链、26 个 Boss 名单 |
| 04 ATM 系列总览与谱系 | ATM1–ATM10、Sky/魔法/Gravitas/Volcano 等变体 |
| 05 ATM10 详解 | 1.21.1 NeoForge 专属内容与 FAQ |
| 06 ATM9 详解 | 1.20.1 专属维度表、AllTheModium、FAQ |
| 07 AE2 应用能源 | 通道、存储、自动合成 |
| 08 电力与资源模组 | Mekanism、Powah、Extreme Reactors、GregTech 等 |
| 09 农业与刷怪模组 | 神秘农业、产能蜜蜂、Apotheosis、Mahou |
| 10 性能与服务端运维 | Java 版本/参数、Spark、服务器、备份 |
| 11 FAQ 与故障排查 | 崩溃、进度卡住、常见"灵异"现象 |
| 12 来源与抓取限制 | 每个站点用什么通道抓的、哪里抓不到 |

## 7. 官方入口

- BMC（LunaPixelStudios）：CurseForge 搜索 "Better MC"；Modrinth 项目 slug：`better-mc-forge-bmc4`、`better-mc-fabric-bmc2`、`better-mc-fabric-bmc3`、`better-mc-neoforge-bmc5`、`better-mc-skyblock`
- ATM 官方攻略站：https://allthemods.github.io/alltheguides/ （只覆盖自家整合包，配方有自定义）
- ATM GitHub 组织：https://github.com/AllTheMods （ATM-9、ATM-10 等仓库用于 issue）
