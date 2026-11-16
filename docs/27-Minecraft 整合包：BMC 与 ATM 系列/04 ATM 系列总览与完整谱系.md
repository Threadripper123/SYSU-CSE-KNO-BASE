# 04 ATM 系列总览与完整谱系

> 主题库：Minecraft 整合包：BMC 与 ATM 系列　｜　源文件：（纯文本条目）　｜　4,192 字符　｜　15 个片段　｜　更新于 2026-11-13
# ATM 系列总览与谱系（All the Mods）

## 1. 是什么

All the Mods（ATM）由 **ATMTeam** 维护，是 Minecraft 最老牌的大型 kitchen-sink 整合包之一。官方自述起点很朴素："最初只是给我几个朋友的私有包，后来别人也想玩。"

它的选模哲学写在每个包的描述里，值得原样理解：

> "Does 'All the Mods' really contain ALL THE MODS? No, of course not."
> 选模三条标准：**不 buggy、不毁性能或进度、已适配目标 MC 版本**。

所以 ATM 不是"模组大杂烩"，而是**有取舍的全能包**：科技（Mekanism、GregTech、Powah、Extreme Reactors）、魔法（Ars Nouveau、Blood Magic、Mahou Tsukai）、存储（AE2、Refined Storage）、农业（Mystical Agriculture、Productive Bees）、冒险（Cataclysm、Blue Skies）都在，但会剔除同类中性能差或破坏平衡的那个。

## 2. 完整谱系（含变体）

| 版本 | MC | 加载器 | CF ID | 下载 | 最新文件 | 备注 |
|---|---|---|---|---|---|---|
| ATM0 | — | — | 372309 | — | — | 最早的私有包 |
| ATM1 | 1.10.2 | Forge | 242462 | 923.7K | 2018-10-26 | 系列起点 |
| ATM2 | **1.11.2** | Forge | 253707 | 253.6K | v1.0a，2017-09-26 | 官方描述"forever evolving... now on 1.11.2" |
| ATM3 | 1.12.2 | Forge | 269708 | 3.1M | 2022-06-08 | 1.12.2 黄金期代表作，含 Thaumcraft、Ender IO、Thermal Expansion |
| ATM3 Remix | 1.12.2 | Forge | 301845 | 516.5K | 2.0.0，2021-01-09 | "更新太大，值得单独出一个包" |
| ATM3 Expert | 1.12.2 | Forge | 325396 | — | — | 专家模式，配方大幅加难 |
| ATM4 | 1.14.4 | Forge | 316059 | 674.8K | 2020-07-02 | |
| ATM5 | 1.15.2 | Forge | 357494 | 693.9K | 2021-12-10 | |
| ATM6 | 1.16.5 | Forge | 381671 | 6.4M | 1.9.2，2024-01-24 | 官方标语"HUGE 1.16.5 Modpack. 你能造出 ATM Star 吗？" |
| ATM7 | 1.18.2 | Forge | 426926 | 3.0M | 1.0.4，2024-03-21 | |
| ATM8 | 1.19.2 | Forge | 520914 | 9.3M | 1.1.0，2023-12-17 | 350+ 模组，带任务书与光影 |
| **ATM9** | 1.20.1 | Forge | 715572 | 13.6M | 1.1.1，2025-10-12 | 400+ 模组，"ATM Star / Gregstar"终局 |
| ATM9 No Frills | 1.20.1 | Forge | 959010 | 1.2M | 0.2，2025-04-24 | 删掉花哨内容，保留同一终局 |
| **ATM10** | 1.21.1 | **NeoForge** | 925200 | 23.0M | 8.5，2026-11-04 | 约 500 模组，首次转 NeoForge |
| ATM10 To the Sky | 1.21.1 | NeoForge | 1298402 | 3.7M | 2.3.1，2026-10-30 | 空岛版："从一棵树开始填满虚空" |

### 派生/并行系列（同一团队，不同主题）

| 包 | 定位 | CF ID |
|---|---|---|
| ATM6 - To the Sky | 空岛（筛子系） | 442246 |
| ATM7 - To the Sky | 空岛 | 655739 |
| ATM9 - To the Sky | 空岛 | — |
| ATM10 - To the Sky | 空岛，1.21.1 NeoForge | 1298402 |
| All The Magic: Spellbound | 纯魔法包，无科技，"重写 ATM Star 配方，各模组互相咬合" | 500199 |
| All The Magic: Arcana | 另一条纯魔法线 | — |
| All The Mods - Volcano Block | 火山块/筛子系，多条资源路径（Ex Machinis、神秘农业、资源鸡、虚空矿机） | — |
| All The Mods - Gravitas / Gravitas² | 重力学主题（GregTech 深度向） | 807446 / 949996 |
| All the Mods - Slice of Pi / Slice of Pi 2 | 轻量趣味向 | 432480 |
| Maul The Odds (MTO) | 独立实验包 | 987792 |

> 结论：**"ATM"已经是一个品牌，不是单一整合包**。查资料时一定要先确认是哪个编号、哪个变体，否则配方对不上。

## 3. 硬件与分配

官方攻略站给出的标准（适用于大多数 ATM 包）：

- **系统内存至少 16GB**，才能有"可玩"的体验
- 客户端分配 **8–12GB**（8192–12288 MB）是推荐区间
- 小型 Lite 包最低 4GB
- **不要**分配全部内存或超出需要的内存
- 需要正版 Minecraft Java 账号
- 需要第三方启动器：CurseForge、FTB App、Prism、ATLauncher、GDLauncher、PolyMC

Java 版本对应关系见《10 性能与服务端运维》文档。

## 4. 官方攻略站的使用方式

https://allthemods.github.io/alltheguides/ 是官方维护的 All The Guides，结构分三块：

1. **Pack Guides（按包）**：atm10（FAQ、Apothic Enchanting、矿分布）、atm9（FAQ、维度表、AllTheModium、Apotheosis、Extreme Reactors、Generator Galore、GregTech、Mahou Tsukai、Mekanism、Mob Grinding Utils、Mystical Agriculture、Powah、Productive Bees）、atm8（FAQ、Apotheosis、Mekanism、Mystical Agriculture）、atm7/lite/sky、atm6（含 sky）、legacy（atm1–atm5、atm3 expert/remix、slop）、atm10tts、atm9tts、atma、atms、atmvb、gravitas、gravitas2。
2. **Mod Guides（通用模组）**：AE2 全家桶（介绍、通道、P2P、存储、自动合成、子网、搭建、压印器、赛特斯农场）、Bulk Cells、Bigger Reactors、Integrated Dynamics、Kaimonick's Guides、AllTheModium 自定义。
3. **Help（通用排障）**：Java、Lag/Profiling、Server、Commands、Configs、Backups、FAQ、AMP 服务器。

**关键提醒**：官方明说"All The Guides 只适用于我们的包"，因为 **ATM 对配方做了大量自定义修改**。在 ATM 里查 JEI 看到的配方才是真配方，网上通用攻略（尤其别的整合包的）经常对不上。

## 5. 进度主线（ATM9/ATM10 共有的骨架）

1. **开局**：任务书（FTB Quests）会给出最省事的起步路线；优先解决食物、床、基础矿物。
2. **电力**：Powah / Mekanism 起步，中期上 Extreme Reactors 或 反应堆，后期 GregTech。
3. **存储**：AE2（或 Refined Storage）是分水岭，做出 ME 系统之后游戏节奏完全变了。
4. **资源自动化**：Mystical Agriculture（精华作物）、Productive Bees（资源蜜蜂）、AllTheModium 矿（挖掘维度）。
5. **附魔与装备**：Apotheosis（ATM9）/ Apothic Enchanting（ATM10）提供远超原版的附魔上限。
6. **终局**：**ATM Star**。ATM9 里由 GregTech 的 **Star Forge** 合成，并进一步通向 **Gregstar**（更硬的终极目标）。

## 6. 下载量说明

ATM10 目前是全系列下载量最高的单包（23.0M，截至 2026-11-13），且是 2026 年最热门的整合包之一；ATM9 是"1.20.1 时代"的事实标准，攻略与视频资料最多。**要查资料，ATM9 的生态是最厚的；要玩新版本，ATM10 是主线。**
