# 01 BMC 系列总览：定位、加载器与配置要求

> 主题库：Minecraft 整合包：BMC 与 ATM 系列　｜　源文件：（纯文本条目）　｜　2,373 字符　｜　14 个片段　｜　更新于 2026-11-13
# BMC 系列总览（Better MC）

## 1. 是什么

Better MC（简称 BMC）是 **LunaPixelStudios** 出品的 Vanilla+ 整合包：不把游戏改造成科技厨房水槽（kitchen-sink），而是保留原版生存循环，在此基础上叠加新维度、Boss、结构与生活质量（QoL）改动。官方宣传口径：

- ✔️ 7 个新维度
- ✔️ 20+ Boss（BMC4 实测统计到 26 个）
- ✔️ 完全重做的世界生成
- ✔️ 100+ 引导任务
- ✔️ 开箱即用的光影预设与优化
- ✔️ 内建免费世界分享（World Sharing）

团队自述为"Minecraft Modding Studio with 1+ Billion Downloads"，CurseForge 主页 22 个项目、83.4M 下载。主开发者 SHXRKIE，维护者 Txni。

> ⚠️ **认包要点**：Modrinth/CurseForge 上有大量同名或近似名的 "Better MC"，只有发布者为 **LunaPixelStudios** 的才是本系列。另一个 "Better Minecraft" 是**完全不同的项目**。

## 2. 三个加载器的实际差异

BMC 不是只有 Forge/Fabric 两条线，而是三条：

| 维度 | Forge（BMC1、BMC4） | Fabric（BMC2、BMC3） | NeoForge（BMC5） |
|---|---|---|---|
| 模组生态 | 最大最成熟，多数模组首发平台 | 较小但增长快 | Forge 的新分支，生态扩张中 |
| 内容深度 | 生物群系/结构/生物整体更多 | 略轻，但有独占内容 | 与 Forge 相当，面向更新的 MC |
| 性能表现 | 偏重，模组多时更明显 | 加载更快、运行更顺 | 与 Forge 接近 |
| 更新节奏 | 稳定但较慢 | 更新更频繁 | 跟随最新 MC 版本 |
| 适合谁 | 要"内容最丰富" | 要"流畅优先" | 要"新版本 + Forge 式生态" |

**重要**：三个加载器的 BMC 是**各自独立开发**的，不是说 Fabric 版是 Forge 的精简复制。某个 Boss、群系或物品在 Forge 版有，不代表 Fabric 版也有。

## 3. 配置要求与优化

| 场景 | 最低 | 推荐 |
|---|---|---|
| 客户端（单人） | 6 GB RAM | 8 GB RAM |
| 服务器 2–5 人 | 4 GB | 6 GB |
| 服务器 6 人以上 | 6 GB | 8 GB+ |

- **Java 17（64 位）** 是 1.20.1 与 1.21.1 版本的共同底线；1.21+ 建议 Java 21。
- 必须 64 位系统。
- **不要把超过一半的物理内存分给客户端**，这会导致不稳定而不是更快。
- **不需要也不支持 OptiFine**：Forge 版自带 **Embeddium + Oculus**；Fabric 版自带 **Sodium + Lithium + Iris**。
- 低配优先降渲染距离、降粒子效果、关掉用不上的视觉模组。

## 4. 安装与开服

### 客户端
1. 安装 CurseForge App（或 Prism Launcher / Modrinth App，后两者通常启动更快）。
2. Minecraft → Browse Modpacks → 搜索 "Better MC" → 选对编号（BMC1–BMC5）。
3. 安装后从 My Modpacks 启动。
4. **安装前先确认版本号**，装错编号意味着之后要重开世界。

### 服务器
1. 在 CurseForge 该整合包的 Files → 展开 Additional Files → 下载 **Server Files**（不是客户端包）。
2. 解压到服务器目录（Windows 下不要解压到 OneDrive 等云同步目录）。
3. Linux 给 `start.sh`/`run.sh` 执行权限，选对应 Java 版本。
4. 首次运行生成文件后，改 `eula.txt` 为 `eula=true`，再启动。

服务端包会剔除仅客户端模组（光影、小地图渲染等），并附带对应加载器的安装器。

## 5. 通用玩法骨架（四版共有）

1. **原版阶段**：正常挖矿、下界、找要塞。BMC 把要塞与下界要塞重做过，结构更大。
2. **维度阶梯**：推荐顺序为 主世界 → 下界 → 以太（Aether）→ 暮色森林（Twilight Forest）→ Blue Skies → 末地。每个维度用上一个维度的产出"卡进度"。
3. **任务书**：100+ 任务用于引导维度顺序与关键合成，不是强制线。
4. **QoL 一揽子**：Waystones（传送石）、XP Tomes/XP Storage（经验存取）、Xaero's Minimap（小地图）、骑龙（Dragon Mounts）。
5. **世界分享**：内建的免费世界共享功能，方便把存档发给朋友。

## 6. 版本选择建议（一句话版）

- 第一次玩 / 想要完整体验：**BMC4**
- 想在新版本 1.21.1 上玩 Forge 生态：**BMC5**
- 电脑较弱 / 更在意加载速度：**BMC2**（1.20.1）或 **BMC3**（1.21.1）
- 1.16.5 情怀或整合包兼容需求：**BMC1**
