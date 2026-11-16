# MC-12 NeoForge 1.21+/26.x 模组开发：注册、数据组件、事件与 datagen

> 主题库：Minecraft 现代版本与模组（2023–2026）　｜　源文件：（纯文本条目）　｜　11,052 字符　｜　41 个片段　｜　更新于 2026-11-15
> 资料截至 2026-11-15；覆盖 2023-11 至 2026-11，重点 2024–2026。

**一句话结论**：NeoForge 1.21+ 的开发范式已经稳定成四件事——用 `DeferredRegister` 延迟注册、用 `DataComponentType` 承载物品数据、用「游戏总线 + mod 总线」两套事件、用 datagen 生成 JSON；26.x 没有推翻这套模型，只是把 JDK 抬到 25、把版本号换成四段式，并跟着原版新增了一批数据驱动注册表。

## 关键变化表

| 版本 | 对开发者的实际变化 | 影响面 |
|---|---|---|
| 1.20.5/1.20.6 | 物品 NBT 被数据组件取代，`ItemStack` 的 `getOrCreateTag()` 退出主流 | 所有带自定义数据的物品、配方、战利品表 |
| 1.21 | 附魔、画作变体等改为数据驱动注册表；`DeferredRegister` 成为唯一推荐注册方式 | 内容型模组、数据包作者 |
| 1.21.2 | 数据组件、配方、战利品表格式调整（数据包格式 57） | 有自定义组件的模组需要改 Codec |
| 1.21.5 | 组件重构：`tooltip_display` 取代 `hide_tooltip`，装备 NBT 合并为 `equipment`；GameTest 可通过数据包使用 | 客户端渲染、装备类模组 |
| 1.21.9 | `pack.mcmeta` 改用 `min_format` / `max_format`（数据包 88.0 起） | 自带数据包/资源包的模组 |
| 26.1 | 最低 Java SE 25；新增 world clock、`minecraft:dye` 组件等；数据包格式 101.1 | 所有模组（编译与运行门槛） |
| 26.2 / 26.3 | 实体谓词改为组件式、未知字段被拒绝；26.3 数据包格式 121.0 | 写 predicate / 战利品表的模组 |

## 工程骨架：从 MDK 开始

NeoForge 官方不推荐手写构建脚本。流程是：到 Mod Generator 页面填 mod 名、mod id、包名、目标 MC 版本和 Gradle 插件（**ModDevGradle** 或 **NeoGradle**），下载 ZIP 解压成工程。两者都能用，区别在于 ModDevGradle 更贴 NeoGradle 上游、配置项少，NeoGradle 历史更长、可定制的构建流程更多。

工程里真正需要你动的文件只有四个：

| 文件 | 作用 | 常见改动 |
|---|---|---|
| `gradle.properties` | mod id、版本、作者、依赖版本 | 改 `mod_version`、`minecraft_version`、`neo_version` |
| `build.gradle` | 构建与依赖 | 加第三方库、mixin 配置、jarJar 打包 |
| `src/main/resources/META-INF/neoforge.mods.toml` | 模组元数据 | `modLoader`、`loaderVersion`、`[[dependencies]]` |
| `src/main/java/.../ExampleMod.java` | 入口 | `@Mod` 构造器里挂 `DeferredRegister` |

构建命令是 `gradlew build`，产物在 `build/libs/<archivesBaseName>-<version>.jar`；调试直接 `gradlew runClient` 或 `gradlew runServer`，工作目录分别是 `runs/client` 与 `runs/server`。第一次导入会下载并反编译 Minecraft 与 NeoForge 源码，慢的时候要接近一小时，属于正常现象。

`neoforge.mods.toml` 里最容易写错的是依赖段：`modLoader="javafml"`、`loaderVersion="[4,)"`，依赖项的 `versionRange` 用 Maven 版本范围语法（如 `[21.1.0,)`），写成 `21.1.0+` 会被当作字符串而不是范围。

## 注册：DeferredRegister 是唯一推荐路径

1.21+ 的注册表分为两类：静态注册表（`BuiltInRegistries`）与数据驱动注册表（datapack registry）。静态注册的写法：

```java
public static final DeferredRegister<Block> BLOCKS =
        DeferredRegister.create(BuiltInRegistries.BLOCK, ExampleMod.MOD_ID);

public static final DeferredHolder<Block, Block> EXAMPLE_BLOCK = BLOCKS.register(
        "example_block", () -> new Block(BlockBehaviour.Properties.of()));
```

要点有三个：`DeferredHolder<R, T>` 是 `Supplier<T>` 的子类，真正取值要等注册事件之后；`register` 的第二个参数可以是 `Supplier`，也可以是 `Function<Identifier, T>`（需要用到自己的注册名时）；最后必须在 mod 构造器里 `BLOCKS.register(modEventBus)`，否则什么都不会发生。

如果必须用老式的 `RegisterEvent`，回调里要按注册表判断：`event.register(BuiltInRegistries.BLOCK.key(), helper -> ...)`。NeoForge 文档明确说这是「不推荐但保留」的路径，因为顺序不可控。

数据驱动注册表（附魔、画作变体、伤害类型、world clock 等）不能用 `DeferredRegister` 静态注册，而是监听 `DataPackRegistryEvent.NewRegistry` 注册 Codec，再让 JSON 进 `data/<namespace>/<registry>/`。这也是模组和整合包能共享内容的地方：整合包作者可以直接改 JSON 而不碰代码。

## 事件：两套总线，别搞混

NeoForge 有两条事件总线：

- `NeoForge.EVENT_BUS`：游戏事件（玩家登录、方块破坏、tick、渲染）。
- **mod bus**：生命周期事件，特征是接口继承 `IModBusEvent`，由 mod 构造器参数传进来。

`@EventBusSubscriber` 会根据事件类型自动挂到正确的总线，这是最省事的写法；手写 `addListener` 时如果挂错总线，表现是「代码不报错但永远不触发」。

启动顺序是固定的：mod 构造器 → 所有 `@EventBusSubscriber` → `FMLConstructModEvent` → 注册事件（`NewRegistryEvent`、`DataPackRegistryEvent.NewRegistry`、各注册表的 `RegisterEvent`）→ `FMLCommonSetupEvent` → 侧别 setup（`FMLClientSetupEvent` / `FMLDedicatedServerSetupEvent`）→ `InterModComms` → `FMLLoadCompleteEvent`。

很多生命周期事件是 `ParallelDispatchEvent` 的子类，会在多个线程上并行触发；在主线程干活必须用 `event.enqueueWork(...)`。把 `FMLCommonSetupEvent` 里直接改全局状态当成单线程写，是新人最常见的崩溃来源。

事件还有三个维度值得先建立概念：**可取消性**（只有标了 `ICancellableEvent` 的事件能设取消，设错类型会抛异常）、**结果类型**（部分事件返回 `TriState` 或 `EventResult`，用来表达「允许 / 拒绝 / 交给下一个监听器」）、**优先级**（高优先级先执行，适合做兼容补丁与覆盖）。这三样决定了模组之间的兼容写法：想「在别人之后改一次结果」和「直接拦掉」是两种完全不同的接入方式。

## 数据组件：物品数据的唯一正确位置

1.20.5 之后，物品不再有「随便塞 NBT」的自由，所有结构化数据都是组件。定义一个自定义组件：

```java
public static final DeferredRegister<DataComponentType<?>> COMPONENTS =
        DeferredRegister.create(Registries.DATA_COMPONENT_TYPE, ExampleMod.MOD_ID);

public static final DeferredHolder<DataComponentType<?>, DataComponentType<ExampleRecord>> EXAMPLE =
        COMPONENTS.register("example", () -> DataComponentType.<ExampleRecord>builder()
                .persistent(ExampleRecord.CODEC)
                .networkSynchronized(ExampleRecord.STREAM_CODEC)
                .build());
```

四个 builder 选项的含义：`persistent` 是落盘用的 `Codec`；`networkSynchronized` 是网络同步用的 `StreamCodec`；`cacheEncoding` 缓存编码结果（只适合几乎不变的值）；`ignoreSwapAnimation` 关掉物品切换动画。前两个至少要给一个，否则构造时直接 `NullPointerException`。

组件值必须是不可变、实现了 `hashCode`/`equals` 的类型，`record` 是最省事的选择。读取侧用 `stack.get(EXAMPLE)` 拿值，写入侧用 `stack.set(...)` 或 `DataComponentPatch` 做补丁；`PatchedDataComponentMap` 是内部实现，不要在模组里直接依赖它。

### Data map：把硬编码字段搬到 JSON

Data map 是 NeoForge 独有的系统，可以理解为「注册表对象 → 任意对象」的可重载映射。文件路径是 `<mapNamespace>/data_maps/<registryNamespace>/<registryPath>/<mapPath>.json`，例如给 `minecraft:item` 加一张 `mymod:drop_healing` 表，文件就放在 `data/mymod/data_maps/item/drop_healing.json`。它和 tag 一样是「叠加」而不是覆盖，`/reload` 就能生效，适合把「哪些物品能烧、能堆肥、给什么燃料值」这类表交给整合包作者改。

## datagen：别再手写 JSON

datagen 的入口是 `GatherDataEvent`，分 `GatherDataEvent.Client` 与 `GatherDataEvent.Server` 两个子类型：前者可以包含全部 provider，后者只能生成数据包条目。MDK 默认把 provider 都注册到 `Client`，用 `gradlew runClientData` 生成；想分开就跑 `runClientData` + `runServerData`。

生成物直接进 `src/generated/resources`（MDK 默认配置），再被 `processResources` 打进 jar。`PackMetadataGenerator` 负责生成 `pack.mcmeta`，方块状态、模型、 loot table、tag、配方、附魔、伤害类型都有对应的 provider。**26.x 里手写 `pack.mcmeta` 已经不合时宜**：1.21.9 起字段从 `pack_format` 换成 `min_format`/`max_format`，用生成器可以避免跟着版本改数字。

## 一个自定义物品的完整链路

把上面几块拼起来，1.21+ 里加一个「会显示自定义提示、带自定义组件、能在创造模式物品栏出现」的物品，实际要做六件事：

1. **注册物品**：`DeferredRegister.Items`（NeoForge 提供的专用子类）里 `registerItem("example_item", Item::new)`，第二个参数是 `Item.Properties`。
2. **给默认组件**：在 `Item.Properties` 上链式调用 `.component(COMPONENTS.EXAMPLE.get(), new ExampleRecord(1, true))`，或用 `DataComponents` 里的原版组件（`MAX_STACK_SIZE`、`RARITY`、`FOOD` 等）。
3. **加入创造标签**：监听 `BuildCreativeModeTabContentsEvent`，把物品塞进某个 `CreativeModeTabs`；这一步经常被忘，表现为「游戏里有这个物品但创造模式搜不到」。
4. **写资源**：`assets/<modid>/models/item/example_item.json`、`textures/item/example_item.png`，以及 1.21.4 起必需的 `assets/<modid>/items/example_item.json`（物品模型定义）。
5. **本地化**：`assets/<modid>/lang/zh_cn.json` 与 `en_us.json`，键是 `item.<modid>.example_item`。
6. **datagen 化**：把 4、5 两步交给 `ItemModelProvider`、`LanguageProvider` 与 `ModelProvider`，否则每次改名字都要手工同步两份 JSON。

第 4 步是 1.21 之后最容易被旧教程坑到的地方：1.21.4 之前物品的模型直接写在 `models/item/*.json`，之后多了一层「物品模型定义」，由 `minecraft:item_model` 组件指向 `assets/<ns>/items/<id>.json`。如果只写了模型没写定义，游戏里物品会显示成紫黑方块，日志里通常只有一行「Unable to load model」的警告。

## 调试与排错

NeoForge 工程的调试体验比 Forge 时代好，因为 `runClient` / `runServer` 是真正的 Gradle 任务，可以直接在 IDE 里断点。几个实用习惯：

| 现象 | 大概率原因 | 定位方法 |
|---|---|---|
| 启动即崩，日志顶部 `Mixin apply failed` | mixin 目标类/方法签名与当前 MC 版本不符 | 看 `logs/latest.log` 里第一个 `Caused by` |
| 模组列表里没有自己的模组 | `neoforge.mods.toml` 的 `modId` 与 `@Mod` 不一致 | 搜 `Mod file ... missing` |
| 物品/方块存在但无贴图 | 缺 `items/*.json` 或纹理路径不对 | F3+T 重载资源包后看日志 |
| 联机时自定义组件为空 | 没给 `networkSynchronized` | 在客户端打印 `stack.get(...)` |
| datagen 不产出文件 | provider 没注册进 `GatherDataEvent`，或跑错任务 | 检查 `runClientData` 输出目录 |

另外两个开关值得记住：`-Dneoforge.enabledGameTestNamespaces=<modid>` 可以只跑自己的 GameTest；`--username Dev` 可以在 `runClient` 里固定用户名，避免每次启动换名字导致存档丢失。

## 26.x 的额外注意点

- **JDK 25**：NeoForge 官方文档要求 Java 25 Development Kit，且必须 64 位 JVM；Gradle 与 IDE 也要跟着换，否则 `UnsupportedClassVersionError`。
- **版本号**：`neo_version` 在 26.x 是四段（如 `26.3.0.103`），在 1.21.x 是三段（如 `21.1.266`），写错会拉到不存在的构件。
- **新注册表**：26.1 引入 world clock、26.2 引入 sulfur cube 相关内容，模组若要兼容就要跟着加 datapack registry，否则 `Registry` 找不到键会在加载期直接崩。
- **格式收紧**：26.2 起实体谓词「未知字段直接报错」，过去靠多写字段碰运气的数据包会加载失败。

## 1.20.1 → 1.21+ API 对照

大部分「照抄旧教程」的失败都集中在这几处，逐条对照能省半天：

| 1.20.1 / Forge 写法 | 1.21+ / NeoForge 写法 | 备注 |
|---|---|---|
| `ForgeRegistries.BLOCKS` | `BuiltInRegistries.BLOCK` | 注册表字段名改为单数，类型也换了 |
| `DeferredRegister.create(ForgeRegistries.BLOCKS, MODID)` | `DeferredRegister.create(BuiltInRegistries.BLOCK, MODID)` | 参数顺序不变 |
| `RegistryObject<Block>` | `DeferredHolder<Block, Block>` | `get()` 语义相同 |
| `stack.getOrCreateTag().putInt(...)` | `stack.set(MY_COMPONENT, ...)` | 自定义数据必须走组件 |
| `@Mod.EventBusSubscriber(modid, bus = Bus.MOD)` | `@EventBusSubscriber(modid = MODID, bus = EventBusSubscriber.Bus.MOD)` | 注解与枚举都换了包，`Bus` 的取值名不变 |
| `MinecraftForge.EVENT_BUS` | `NeoForge.EVENT_BUS` | 事件类整体迁移到 `net.neoforged.neoforge.event` |
| `mods.toml` | `neoforge.mods.toml` | 文件名与 `modLoader` 都变了 |

还有一个隐性差异：Forge 时代很多模组习惯在 `FMLCommonSetupEvent` 里注册网络通道，NeoForge 1.21+ 改用 `RegisterPayloadHandlersEvent` 与 `PayloadRegistrar`，旧的 `SimpleChannel` 已不在主线文档里。移植网络包时这块基本要重写。

## 常见坑

1. `DeferredRegister` 建了但忘记 `register(modEventBus)`，注册表里什么都没有，报错却是「方块不存在」。
2. 在 `FMLCommonSetupEvent` 里直接操作世界或注册表：该事件并行触发，必须 `enqueueWork`。
3. 组件只给了 `persistent` 没给 `networkSynchronized`：单机正常，联机客户端拿不到值。
4. `neoforge.mods.toml` 的 `versionRange` 写成 `1.21.1` 而不是 `[21.1.0,)`，加载器判定依赖不满足，模组被静默跳过。
5. 用 1.20.1 的教程照抄 `ForgeRegistries`、`@Mod.EventBusSubscriber` 与 `getOrCreateTag()`，在 1.21+ 全部失效。
6. datagen 生成的 `pack.mcmeta` 还是旧格式：1.21.9+ 加载时提示不兼容，但游戏仍能启动，很容易漏掉。

## 学习路径建议

NeoForge 文档已经按主题拆得很细，按依赖顺序读比乱翻快得多：先 `concepts/registries` 与 `concepts/events`（这两篇决定你后面所有代码的骨架），再 `items/datacomponents` 与 `blocks/index`，然后 `resources/`（资源与 datagen）与 `datablockentities`。网络包放到最后，因为 1.21+ 的 payload 注册方式与旧教程差异最大。

如果只想先跑起来，最短路径是：Mod Generator 出工程 → `gradlew runClient` 确认能进游戏 → 加一个 `DeferredRegister.Items` 的物品 → 用 `ItemModelProvider` 生成模型 → 再回头补事件与组件。先写一大套架构再启动，是新手最容易卡住的地方，因为 NeoForge 的注册时机与资源加载顺序都需要实际跑一遍才能建立直觉。

## 来源

- NeoForge 官方文档 Getting Started（JDK 25、Mod Generator、构建与运行）：https://docs.neoforged.net/docs/gettingstarted/ （访问 2026-11-15）
- NeoForge 文档 Versioning（四段版本号规则、26.x 版本语义）：https://docs.neoforged.net/docs/gettingstarted/versioning/ （访问 2026-11-15）
- NeoForge 文档 Registries（`DeferredRegister`、`RegisterEvent`、datapack registry）：https://docs.neoforged.net/docs/concepts/registries/ （访问 2026-11-15）
- NeoForge 文档 Events（双总线、生命周期顺序、`ParallelDispatchEvent`）：https://docs.neoforged.net/docs/concepts/events/ （访问 2026-11-15）
- NeoForge 文档 Data Components（builder 四个选项、不可变值）：https://docs.neoforged.net/docs/items/datacomponents/ （访问 2026-11-15）
- NeoForge 文档 Data Maps（路径规则、叠加语义）：https://docs.neoforged.net/docs/resources/server/datamaps/ （访问 2026-11-15）
- NeoForge 文档 Resources（`pack.mcmeta`、`GatherDataEvent`、`runClientData`）：https://docs.neoforged.net/docs/resources/ （访问 2026-11-15）
- NeoForge maven 元数据（`26.3.0.103`、`21.1.266` 等真实版本号）：https://maven.neoforged.net/releases/net/neoforged/neoforge/maven-metadata.xml （访问 2026-11-15）
- minecraft.wiki Pack format（26.1 数据包格式 101.1、26.3 为 121.0）：https://minecraft.wiki/w/Pack_format （访问 2026-11-15）

## 不确定项

- 文档站 `docs.neoforged.net` 的「最新版」对应哪个 NeoForge 分支没有明示；本文引用的 API（`DataComponentType.Builder`、`GatherDataEvent` 双子类型）按 1.21+/26.x 当前主线理解，若在 26.3 有重命名未逐一核对。
- 26.2「实体谓词未知字段被拒绝」来自 minecraft.wiki 的格式变更摘要，未在 NeoForge 侧验证其错误提示形态。
- ModDevGradle 与 NeoGradle 的具体版本号未核实，文中只比较定位不比较版本。
- `gradle.properties` 中的属性名（`neo_version`、`minecraft_version`）按 MDK 惯例书写，未直接读取 26.3 的 MDK 模板逐一确认。
