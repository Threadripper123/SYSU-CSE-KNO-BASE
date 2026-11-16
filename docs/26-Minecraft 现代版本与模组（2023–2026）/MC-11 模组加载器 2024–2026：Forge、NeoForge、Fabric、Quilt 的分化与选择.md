# MC-11 模组加载器 2024–2026：Forge、NeoForge、Fabric、Quilt 的分化与选择

> 主题库：Minecraft 现代版本与模组（2023–2026）　｜　源文件：（纯文本条目）　｜　8,434 字符　｜　27 个片段　｜　更新于 2026-11-15
> 资料截至 2026-11-15；覆盖 2023-11 至 2026-11，重点 2024–2026。

**一句话结论**：2024–2026 这三年，加载器从「Forge 默认、Fabric 轻量替补」变成「NeoForge 接棒 Forge 生态、Fabric 守住快照与轻量、Forge 靠存量续命、Quilt 长期停在 0.x beta」；到 26.3（2026-09-15）为止，NeoForge、Forge、Fabric 都在同一天有可安装的构建，但新模组和新整合包的默认目标已经明显偏向 NeoForge 与 Fabric。

## 关键变化表

| 时间 | 事件 | 对格局的影响 |
|---|---|---|
| 2023-07-13 | NeoForged 团队宣布 NeoForge 从 Forge 分裂并提前发布（首个目标是 1.20.2） | Forge 生态被一分为二，大型科技包开始评估迁移 |
| 2024-06-13 | 1.21 发布，NeoForge 21.0 与 Fabric 同步跟进 | 新版本首日支持成为「加载器是否活着」的公开指标 |
| 2024-10 ~ 2025-03 | 1.21.2/1.21.3、1.21.4、1.21.5 连续三个 drop | NeoForge 采用 `21.x.y` 版本号，Forge 用 `1.21.5-55.x`，两条线开始各自长 |
| 2025-03 ~ 2026-01 | 1.21.5 → 1.21.11 的 drop 节奏压到 2–3 个月 | Forge 在新 drop 上的构建数量明显变薄（1.21.6 仅 10 个构建、1.21.7 仅 4 个），NeoForge 与 Fabric 保持常态构建 |
| 2026-03-24 | 26.1 采用年份版本号，最低 Java SE 25 | 加载器集体抬高 JVM 门槛，旧 JDK 21 工作区必须升级 |
| 2026-06-16 / 09-15 | 26.2、26.3 发布 | NeoForge 26.3.0.103、Forge 26.3-66.1.8、Fabric 26.3 支持同时可用 |

## 四家现状（2026-11-15 实测）

| 加载器 | 最新稳定版本 | 覆盖的最新 MC | Modrinth 标注项目数 | 观察 |
|---|---|---|---|---|
| NeoForge | `26.3.0.103`；LTS 线 `21.1.266` | 26.3 | 34,174 | 生产力最强，LTS 分支到 2026-11 仍在出构建 |
| Fabric | Loader `0.20.1`，Fabric API `0.166.2+26.3` | 26.3 | 62,449 | 项目总数第一，快照/发布日跟进最快 |
| Forge | `26.3-66.1.8` | 26.3 | 39,915 | 仍能出包，但新 drop 的构建数量显著变薄 |
| Quilt | Loader `0.30.2`；最新 `0.31.0-beta.6` | 未见 26.x 支持证据 | 11,411 | 长期停在 0.x，生态基本冻结 |

上面的项目数来自 Modrinth 搜索接口按加载器分类的 `total_hits`，只代表「被打上该加载器标签的项目数量」，不等于活跃度或下载量；但四个数量级足以说明生态重心。另一条独立证据是构建密度：NeoForge 在 maven 上一共有 1,859 个版本，Forge 有 5,106 个，后者总量更大是因为它从 1.1 时代就开始积累，而不是 2026 年更活跃。

## 加载器在技术上到底做了什么

四家做的事可以拆成同一组职责，只是实现方式不同：

1. **接管启动**：在 `net.minecraft.client.main.Main` 之前插入自己的入口，把 classpath 重排，扫描 `mods/` 下的 jar。
2. **转换字节码**：Forge/NeoForge 用 Mixin + AccessTransformer（AT，宽化 Java 可见性），Fabric 用 Mixin + AccessWidener（AW，宽化范围更细），Quilt 两者都兼容一部分。
3. **定义模组元数据**：NeoForge/Forge 读 `META-INF/neoforge.mods.toml` 或 `META-INF/mods.toml`，Fabric/Quilt 读根目录的 `fabric.mod.json` / `quilt.mod.json`。这一步决定依赖解析和加载顺序。
4. **提供注册与事件**：这是 API 层，NeoForge 走 `DeferredRegister` + 双事件总线，Fabric 走 `Registry.register` + 回调式 API（`ServerLifecycleEvents`、`ClientTickEvents` 等）。

差异带来的直接后果是：**同一个模组的 1.21 版源码，在 Forge 与 NeoForge 之间不能只改 import**，注册表 API、事件注解、物品组件构造方式都不同；而 Fabric 与 NeoForge 之间的移植量更大，因为连「注册发生在什么时候」这件事都不一样（Fabric 是静态注册，NeoForge 是延迟到注册事件里）。

### NeoForge：从「分支」变成「主线」

NeoForge 的崛起不是靠兼容层，而是靠把 Forge 的历史包袱换掉：事件总线拆成 `NeoForge.EVENT_BUS`（游戏事件）与 mod bus（生命周期事件），注册系统推 `DeferredRegister`，数据组件、数据地图（data map）、`neoforge.mods.toml` 都重做了一遍。对模组作者最直接的好处是 API 边界更清楚，坏处是 1.20.1 时代那套 `ForgeRegistries` + `@Mod.EventBusSubscriber` 的写法不能照抄。

版本号规则值得单独记：NeoForge 的 major 是 Minecraft 的 minor，minor 是 Minecraft 的 patch，patch 才是 NeoForge 自己的构建号；26.x 之后因为年份版本号本身有三位，变成四段式，例如 `26.1.0.5` 表示「MC 26.1 的第 6 个构建」。所以 `21.1.266` 不是 MC 21.1，而是 MC 1.21.1 的第 267 个构建，这条到现在还有人在 issue 里搞错。

LTS 是 NeoForge 2024–2026 最成功的产品决策：`21.1.x` 这条线从 2024 年一直维护到 2026 年 11 月（`21.1.266` 就在 maven 最新列表里），整合包作者可以在一个 MC 小版本上待两年不动，而不用跟着 drop 重做。这也是大型科技包长期钉在 1.21.1 的技术前提。

### Forge：还在，但节奏变了

Forge 没有停更。maven 元数据里 5,106 个版本，最新 `26.3-66.1.8`，1.21.1 分支（`52.1.21`）也仍在维护。但从构建密度能看出资源分配的变化：1.21.1 有 70 个构建、1.21.4 有 53 个、1.21.5 有 41 个，到 1.21.6 只剩 10 个、1.21.7 只剩 4 个、1.21.9 只剩 6 个；26.1 只有 8 个，26.1.2 又回到 24 个。这不是「死了」，而是「只保证能跑，不再追每个 drop 的细节」。

对整合包的现实含义：1.20.1 和 1.21.1 的 Forge 包依然能长期维护，但如果你想用 26.x 的新内容，Forge 的模组覆盖会比 NeoForge 薄，尤其是新 API（数据组件、数据地图、客户端物品模型）相关的模组。反过来说，Forge 在 1.20.1 的存量优势仍然巨大：`47.4.29` 这条线是过去三年最多模组兼容的版本，很多老包根本不会离开。

### Fabric：项目数第一，路线最「贴版本」

Fabric 的打法一直是「快照第一天就能跑」。`meta.fabricmc.net` 的 game 版本列表里 26.1、26.1.1、26.1.2、26.2、26.3 全部在列，Fabric API 在 2026-11-11 还在发 `0.166.2+26.3`，说明 26.3 的适配是持续维护而不是一次性发布。Loader 稳定版到 `0.20.1`，跨大版本升级时 Loader 本身的 breaking change 很少，这是它受服务端和轻量客户端整合包欢迎的原因。

代价是 Fabric 不做「全家桶」：物品栏、渲染管线、网络包、维度这些都要靠 Fabric API 和第三方库拼，模组作者要自己决定依赖哪些库，整合包里因此经常出现三个不同作者的小 lib。另一个副作用是版本分裂快：Fabric API 的每个 MC 小版本都有独立构建号（如 `0.166.2+26.3`），手动维护整合包时容易把 API 版本和 MC 版本配错。

### Quilt：兼容 Fabric，但没形成自己的生态

Quilt Loader 最新是 `0.31.0-beta.6`，稳定线 `0.30.2`，仍在 0.x。它能加载相当一部分 Fabric 模组，但 QSL（Quilt Standard Libraries）没有形成规模，模组作者没有动力同时维护两套 API。对普通玩家，结论很简单：没有明确理由就用 Fabric 或 NeoForge，Quilt 只在「某个包明确要求」时使用。

## 怎么快速判断一个包用的是哪个加载器

不用打开游戏，看文件就够了：

| 位置 | 看到什么 | 结论 |
|---|---|---|
| 实例根目录 | `fabric-server-launch.jar`、`fabric-loader-*.jar` | Fabric |
| 实例根目录 | `libraries/net/neoforged/`、`run.sh` 里出现 `neoforge` | NeoForge |
| 实例根目录 | `libraries/net/minecraftforge/forge/` | Forge |
| `mods/` 里任意 jar | `META-INF/neoforge.mods.toml` | NeoForge 模组 |
| `mods/` 里任意 jar | `META-INF/mods.toml` | Forge 模组 |
| `mods/` 里任意 jar | 根目录 `fabric.mod.json` | Fabric/Quilt 模组 |

服务端还有一层容易混淆的地方：Paper、Purpur、Folia 属于「插件平台」，它们跑的是 Bukkit/Paper 插件；只有 Forge、NeoForge、Fabric 才有服务端 mod。一个「Fabric 服务端」可以加 Fabric 模组，但不能装 Paper 插件，反之亦然——两类东西偶尔会通过兼容层共存（例如 Cardboard、Youer 一类项目），但那是额外项目，不是加载器本身的能力。

## 选择矩阵

| 场景 | 首选 | 理由 |
|---|---|---|
| 大型科技/魔法整合包（1.21.1 长周期） | NeoForge `21.1.x` LTS | 模组覆盖最全，LTS 不用追 drop |
| 想玩 26.3 的最新内容 | NeoForge 或 Fabric | Forge 的新 drop 构建与模组都更薄 |
| 轻量客户端包、服务端插件式玩法 | Fabric | Loader 稳定、启动快、库依赖少 |
| 老 1.20.1 包维护 | Forge `47.4.29` | 历史模组兼容性最好，迁移成本高 |
| 二次开发/自己写 API 扩展 | NeoForge | 文档与 datagen 体系最完整 |
| 跟着别人发布的包走 | 看包作者的 loader | 混装没有意义，兼容性不打折 |

## 对玩家与整合包作者的实际影响

- **选包先看加载器，再看 MC 版本**：同一个包名在不同版本可能换加载器，装错 loader 的报错通常是「找不到 mod 的入口点」或直接崩在 mixin 阶段。
- **1.21.1 是事实上的「长周期版本」**：NeoForge LTS 与 Forge 52.x 都还在维护，大型整合包优先钉这里，升级到 26.x 的收益主要是新内容而不是稳定性。
- **26.x 的统一门槛是 Java 25**：26.1 起游戏最低 Java SE 25，NeoForge 文档也要求 JDK 25；老实例里配的 JVM 21 必须换，32 位 JVM 一律不行。
- **别在一个 `mods` 目录里混装 Forge 与 NeoForge 的同名模组**：两者 jar 里的 `META-INF` 元数据互不识别，崩溃日志经常只报一个空指针，很难看出是混装导致。

### 从 1.20.1 Forge 包迁到 1.21.1 NeoForge 的最小路线

1. **先换 Java**：1.21.1 要求 Java 21，26.x 要求 Java 25；把启动器实例的 JVM 参数指向新 JDK，再动模组。
2. **清空 `mods/` 重新按 NeoForge 版本收模组**：不要试图复用 1.20.1 的 jar，NeoForge 的 `neoforge.mods.toml` 与 Forge 的 `mods.toml` 不通用。
3. **配置文件迁移要人看**：`config/` 下大部分 TOML 能复用，但涉及注册名变更（如 `forge:` → `neoforge:` 命名空间）的键会被重置为默认值。
4. **存档先备份**：1.20.1 → 1.21.1 跨了两个数据版本（data version），方块与实体的 NBT 结构在 1.20.5 有一次性大改（物品组件化），旧存档直接开会丢自定义物品数据。

## 常见坑

1. 把 `21.1.266` 当成 MC 21.1 的加载器版本，下载了错误分支。
2. 用 Forge 的 `mods.toml` 模板去写 NeoForge 模组：NeoForge 要的是 `META-INF/neoforge.mods.toml`，`modLoader` 写 `javafml`、`loaderVersion` 与 `[[dependencies]]` 的版本范围语法也不同。
3. 客户端装 Fabric、服务端装 NeoForge，然后疑惑为什么连不上：加载器不同的服务端不会在握手阶段告诉你原因，只会显示协议不兼容。
4. 以为 Quilt 能「无痛」跑 Fabric 包：Loader 兼容不等于 API 兼容，涉及 Fabric API 事件和 mixin 注入点的模组经常在 Quilt 上出问题。
5. 在 26.x 上继续用 JDK 21 启动，日志第一行就是 `UnsupportedClassVersionError`。
6. 只换了加载器却沿用旧 `options.txt` 与光影包：渲染管线在 1.21.5/26.x 有改动，旧 shaderpack 会黑屏而不是报错。

## 26.4 与后续节奏（加载器视角）

26.4 尚未发布（截至 2026-11-15），但从两条侧证能看出加载器会怎么跟：一是 minecraft.wiki 的资源包格式表里已经出现「26.4 Snapshot 1 → 98.0」这一行，说明快照周期已经开始；二是 NeoForge 的 maven 里保留着 `26.1.0.0-alpha.*` 这类快照构建，命名规则是「目标版本 + alpha + 快照编号」，也就是加载器会在正式版之前先出 alpha，让模组作者提前适配。

这意味着 2026 年冬到 2027 年初的现实节奏是：正式版发布当天能用的通常只有 NeoForge 与 Fabric 的构建，Forge 会晚几天到几周，Quilt 则要看有没有人接手。对整合包作者的操作建议是：不要在快照期把主包迁到新版本，等第一个 `26.4.x` 稳定构建和 Fabric API 的对应版本都出来再动。

## 来源

- NeoForge maven 元数据（版本清单、最新构建）：https://maven.neoforged.net/releases/net/neoforged/neoforge/maven-metadata.xml （访问 2026-11-15）
- Forge maven 元数据（5,106 个版本、按 MC 版本统计构建数）：https://maven.minecraftforge.net/net/minecraftforge/forge/maven-metadata.xml （访问 2026-11-15）
- Fabric Loader 版本元数据：https://meta.fabricmc.net/v2/versions/loader （访问 2026-11-15）
- Fabric 游戏版本支持列表：https://meta.fabricmc.net/v2/versions/game （访问 2026-11-15）
- Quilt Loader 版本元数据：https://meta.quiltmc.org/v3/versions/loader （访问 2026-11-15）
- Modrinth 搜索接口按加载器统计（fabric 62,449 / forge 39,915 / neoforge 34,174 / quilt 11,411）：https://api.modrinth.com/v2/search?facets=[["categories:fabric"]] （访问 2026-11-15）
- Fabric API 26.3 版本记录（`0.166.2+26.3`，2026-11-11）：https://api.modrinth.com/v2/project/fabric-api/version?game_versions=["26.3"] （访问 2026-11-15）
- NeoForge 官方文档（版本号规则、JDK 25 要求）：https://docs.neoforged.net/docs/gettingstarted/ 与 https://docs.neoforged.net/docs/gettingstarted/versioning/ （访问 2026-11-15）
- 二手来源（NeoForge 分裂日期 2023-07-13，仅作交叉参考）：https://www.mcmod.cn/class/11433.html （访问 2026-11-15）

## 不确定项

- NeoForge 分裂公告的官方原文（neoforged.net 博客）本次未直接打开成功，2023-07-13 这个日期来自 MC 百科二手条目，建议后续用官方博客复核。
- Quilt 是否已经支持 26.x 未能确认：`meta.quiltmc.org` 只提供 Loader 版本，没有给出对应的游戏版本矩阵；文中「未见 26.x 支持证据」按此理解。
- 「NeoForge LTS 会维护多久」没有官方承诺页，只能从 `21.1.266` 仍在大版本列表尾部推断仍在维护。
- Sinytra Connector（在 NeoForge 上跑 Fabric 模组）在 2026 年是否仍活跃未核实，本文未把它计入选择建议。
- 迁移路线中关于配置键与存档 NBT 的结论来自机制推断，未做实际跨版本开档验证。
