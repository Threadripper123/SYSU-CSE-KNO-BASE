# MC-21 整合包制作与分发：Packwiz、KubeJS 7、任务书、CurseForge/Modrinth 发布

> 主题库：Minecraft 现代版本与模组（2023–2026）　｜　源文件：（纯文本条目）　｜　9,926 字符　｜　55 个片段　｜　更新于 2026-11-15
> 资料截至 2026-11-15；覆盖 2023-11 至 2026-11，重点 2024–2026。

# MC-21 整合包制作与分发：Packwiz、KubeJS 7、任务书、CurseForge/Modrinth 发布

## 一句话结论

2026 年的整合包制作分成两条流水线：一条是“文件包”路线（CurseForge zip / Modrinth `.mrpack`），一条是“元数据包”路线（packwiz 的 `pack.toml` + `index.toml`，客户端用 installer 拉取）。KubeJS 7 与 FTB Quests 负责把内容变成可维护的脚本与 SNBT；ATM10 的 67 章任务书就是这套流水线的极限样本。

## 关键变化表

| 环节 | 2024 前的做法 | 2026 的做法 | 关键工具/格式 |
|---|---|---|---|
| 依赖管理 | 手动往 `mods/` 塞 jar | TOML 元数据 + 版本锁定 | packwiz `pack.toml`、`index.toml` |
| 客户端更新 | 重下整个 zip | installer 增量拉取 | `packwiz-installer-bootstrap.jar` |
| 服务端分发 | 单独打包 | 同一份元数据，按 side 过滤 | `side = "server"` / `"client"` |
| 脚本 | CraftTweaker/JSON | JavaScript 事件 | KubeJS 2101.7.2 |
| 任务 | 手写 SNBT 或不用 | GUI 编辑 + SNBT 版本化 | FTB Quests `chapters/*.snbt` |
| 发布 | CurseForge zip | CurseForge zip + Modrinth `.mrpack` | `manifest.json` / `modrinth.index.json` |

## 一、Packwiz：把整合包变成 git 仓库

packwiz 的定位很明确：不管理 jar，只管理“指向 jar 的元数据”。初始化只要一条命令：

```bash
packwiz init
```

它会问包名、作者、版本、Minecraft 版本与加载器，生成 `pack.toml`。之后所有操作都围绕这个文件：

| 命令 | 作用 | 什么时候用 |
|---|---|---|
| `packwiz refresh` | 重建 `index.toml` | 手动改过文件后 |
| `packwiz curseforge add <slug>` | 从 CurseForge 加模组 | 主模组源 |
| `packwiz modrinth add <slug>` | 从 Modrinth 加模组 | 开源模组优先 |
| `packwiz update --all` | 批量升级 | 版本维护 |
| `packwiz pin <file>` | 锁定不升级 | 关键模组/兼容补丁 |
| `packwiz remove <file>` | 移除 | 换模组 |
| `packwiz serve` | 本地 HTTP 测试 | 验证 installer |
| `packwiz curseforge export` | 导出 CF 包 | 发布 CurseForge |
| `packwiz modrinth export` | 导出 `.mrpack` | 发布 Modrinth |

几个实战细节：

- packwiz 没有正式的 GitHub Releases，二进制从 GitHub Actions 或 nightly.link 取；自己编译要 Go 1.24+（README 明确写了）。
- `packwiz refresh` 不是可选的“整理”，它决定 `index.toml` 里的哈希是否与文件一致；忘了跑，installer 会报下载校验失败。
- `side` 字段是服务端分发的核心：同一条元数据可以标记 `client`、`server` 或 `both`，installer 按侧过滤。ATM 这类大包如果不标 side，服务端会把客户端专用模组装进去，直接崩。
- 用 git 管理时，`pack.toml`、`index.toml` 与各模组的 `.pw.toml` 都要提交；jar 本身不进仓库。

客户端接入用 installer：

```bash
java -jar packwiz-installer-bootstrap.jar https://example.com/pack.toml
```

MultiMC/Prism 把它放进“预启动命令”，每次启动先对齐版本；服务器场景则让玩家用同一个 URL，省去“你装了什么版本”的扯皮。

## 二、`.mrpack`：Modrinth 唯一接受的格式

Modrinth 的帮助文档写得很清楚：`.mrpack` 是 ZIP，根目录必须有 `modrinth.index.json`，这是唯一能上传到 Modrinth 的整合包格式。核心字段：

| 字段 | 含义 | 注意 |
|---|---|---|
| `formatVersion` | 格式版本 | 当前为 `1` |
| `game` | 游戏类型 | 只能是 `minecraft` |
| `versionId` | 版本标识 | 每次发布唯一 |
| `name` | 包名 | 人类可读 |
| `files[]` | 文件清单 | 每项含 `path`、`hashes`、`downloads`、`fileSize` |
| `hashes` | 校验 | 必须同时有 `sha1` 与 `sha512` |
| `env` | 客户端/服务端 | `client`/`server` 各取 required/optional/unsupported |
| `dependencies` | 游戏与加载器 | 如 `minecraft`、`neoforge` |
| `overrides/` | 覆盖文件 | 配置、脚本、任务书放这里 |

两个容易踩的点：

1. `path` 必须相对实例目录，且不能包含 `..` 或以盘符/斜杠开头；Modrinth 上传时会自动校验，越界直接拒。
2. `hashes` 缺 `sha512` 会被判不合规；很多第三方导出工具只写 sha1，上传前要检查。

## 三、KubeJS 7：脚本层的“可维护性”

KubeJS 7 对应 1.21.x，当前 NeoForge 1.21.1 的版本号是 **2101.7.2-build.381**（2026-11-01 发布）。它的目录结构决定了“什么能热重载”：

| 目录 | 内容 | 重载方式 |
|---|---|---|
| `kubejs/startup_scripts/` | 注册物品/方块、改注册表 | `/kubejs reload startup_scripts`，部分要重启 |
| `kubejs/server_scripts/` | 配方、标签、事件 | `/reload` 或 `/kubejs reload server_scripts` |
| `kubejs/client_scripts/` | 客户端 UI/提示 | `F3 + T` 或 `/kubejs reload client_scripts` |
| `kubejs/assets/` | 材质、模型、语言 | 随资源重载 |

配方事件的标准写法：

```js
ServerEvents.recipes(event => {
  event.shaped('minecraft:stick', [
    'A',
    'A'
  ], { A: '#minecraft:planks' })
})
```

注册自定义物品要放在 startup：

```js
StartupEvents.registry('item', event => {
  event.create('test_item')
  event.create('custom_sword', 'sword').tier('diamond').attackDamageBaseline(10)
})
```

材质路径是 `kubejs/assets/kubejs/textures/item/test_item.png`；要换命名空间就用 `.texture('mobbo:item/lava')`，对应 `kubejs/assets/mobbo/textures/item/lava.png`。调试用 `/kjs hand` 看手上物品的 ID 与组件，比猜 ID 快。

KubeJS 7 的实战价值不在“能写脚本”，而在“脚本能进 git、能 review、能按目录热重载”。ATM10 这种 500 模组的包，配方改动如果全靠 CraftTweaker 的 zen 文件，维护成本会高一个量级。

## 四、FTB Quests：任务书是数据，不是代码

FTB Quests 把任务存成 SNBT。ATM10 的仓库就是最好的教材：

- `config/ftbquests/quests/data.snbt`：全局设置；
- `config/ftbquests/quests/chapter_groups.snbt`：章节分组（ATM10 有 10 组）；
- `config/ftbquests/quests/chapters/*.snbt`：每章一个文件（ATM10 67 个，Sky 55 个）；
- `config/ftbquests/quests/lang/*`：语言文件（ATM10 860 个，Sky 200 个）。

`data.snbt` 里值得抄的键：

```snbt
{
	default_autoclaim_rewards: "disabled"
	detection_delay: 60
	emergency_items_cooldown: 300
	progression_mode: "flexible"
	version: 13
	grid_scale: 0.5d
}
```

单章文件的结构（真实样本，已删减）：

```snbt
{
	autofocus_id: "5151CDD8FCDE7A07"
	default_quest_shape: "diamond"
	filename: "basic_tools"
	group: "15E5B587D291A4AA"
	icon: { id: "minecraft:diamond_pickaxe" }
	id: "4B9D26E0087CD163"
	images: [ { id: "2E5A81BDE262AE73", image: "atm:textures/questpics/basictools/4.png", x: -3.5d, y: 2.0d, width: 0.5d, height: 0.7d } ]
}
```

ID 是 16 位十六进制大写，任务、章节、分组、图片各有一套；手写时最容易错的是“引用了不存在的 ID”。实践建议：

1. 先在游戏里用编辑模式搭结构，再导出 SNBT 进 git；
2. 章节文件名与 `filename` 字段保持一致；
3. 汉化只改 `lang/`，不要把中文写进 `chapters/`；
4. 改完重进世界或重载任务书，验证依赖线没断。

## 五、发布：CurseForge 与 Modrinth 的差异

| 维度 | CurseForge | Modrinth |
|---|---|---|
| 包格式 | zip + `manifest.json` | `.mrpack` |
| 清单 | `manifestType`、`manifestVersion`、`minecraft.version`、`modLoaders[]`、`files[]` | `modrinth.index.json` |
| 覆盖目录 | `overrides/` | `overrides/` |
| 审核 | 人工/规则审核 | 自动校验 + 规则 |
| 更新 | 上传新 file | 上传新 version |

CurseForge 的清单不写下载 URL，只写 `projectID` + `fileID`，由启动器去查；这带来一个硬约束：**你用的每个模组都必须在 CurseForge 有对应项目与文件**，否则导入时缺 mod。Modrinth 的 `files[].downloads` 可以直接写 CDN URL，但哈希必须对。

发布前的检查清单：

- 客户端 `mods/` 与服务端 `mods/` 的差集是否只有 side 标记的模组；
- `config/` 里是否有本机路径、绝对路径、测试用 OP 名单；
- 任务书是否引用了仅客户端模组的物品；
- 版本号是否与 `pack.toml`/`manifest.json`/`modrinth.index.json` 三处一致。

## 六、ATM10 作为工程样本

ATM10 的任务书规模（67 章、4,818 任务、860 语言文件）说明大包的任务书已经是“半个软件项目”：

- 章节按模组切分，单章最大 251 个任务（Productive Bees），最小只有几十个；
- 10 个 chapter group 把 67 章折叠成可浏览的层级；
- `progression_mode: "flexible"` 允许玩家跳章，避免“卡一章弃坑”；
- `detection_delay: 60` 与 `emergency_items_cooldown: 300` 是防刷/防误触的保护值。

如果你要做自己的包，建议先定“章节数 ≤ 20、单章 ≤ 60 任务”，再逐步加；ATM10 的规模是三年迭代出来的，不是一次写成的。

## 七、从零做一个 20 章小包的顺序

1. 定版本：先选 Minecraft 与加载器（2026 年新包建议 1.21.1 + NeoForge，生态最全）。
2. `packwiz init`，提交 `pack.toml` 与 `.gitignore`（忽略 `mods/`、`.minecraft/`）。
3. 按“科技/魔法/探索/QoL”四类各挑 5–8 个模组，`packwiz modrinth add` 优先。
4. 进游戏跑一次，确认没有重复物品/配方冲突，再开始写 KubeJS。
5. 任务书先搭 5 章骨架（开局、资源、电力、维度、终局），每章 ≤ 30 任务。
6. 用 `packwiz serve` 让测试客户端走 installer，验证增量更新。
7. 最后才导出 `.mrpack` 与 CurseForge zip，发布前跑一遍“纯净客户端 + 服务端”双端测试。

这个顺序的核心是“先锁版本与依赖，再写内容”。反过来做，通常会在第 4 步发现模组冲突，然后被迫重选整条科技线。

## 对作者 / 服主 / 发布者的实际影响

- **作者**：packwiz + git 是当前最省心的组合，尤其适合“私包/小范围分发”；要上 Modrinth 再加一步 `packwiz modrinth export`。
- **服主**：用 packwiz 的 side 过滤能保证客户端与服务端模组表一致，减少“进服被踢”的工单。
- **发布者**：CurseForge 与 Modrinth 的清单格式不同，别指望一份 zip 两边通吃；`.mrpack` 也不能直接传 CurseForge。

## 常见坑（逐条对照）

1. **忘了 `packwiz refresh`**：`index.toml` 哈希过期，installer 校验失败。
2. **`.mrpack` 的 `path` 越界**：带 `..` 或盘符会被 Modrinth 拒绝，这是安全校验不是 bug。
3. **只写 sha1**：`.mrpack` 要求 sha1 + sha512，缺一不可。
4. **把客户端模组标成 both**：服务端会加载渲染/UI 模组，轻则报错重则崩服。
5. **手改 SNBT 的 ID**：16 位十六进制 ID 一旦重复或引用错，任务线会整段消失。
6. **任务书用中文直写**：语言文件与章节分离，直写会导致切换语言时混乱。
7. **CurseForge 清单引用站外模组**：`projectID` 不存在时导入缺 mod，且不会给你明确报错。


## 八、团队协作：把 packwiz 当代码库

packwiz 的真正优势在多人协作时才会显现：

- 一人一分支，一个模组一个 `.pw.toml`，diff 很小，review 时能看清“加了哪个文件、锁了哪个版本”；
- CI 里跑一次 `packwiz refresh`，再执行 `git diff --exit-code index.toml`，就能拦住“忘记刷新”的提交；
- 发布用 tag：`v1.0.0` 对应 `pack.toml` 的 `version`，installer 的 URL 指向发布分支的 `pack.toml`，玩家端自然对齐；
- CurseForge 的 API key 不要写进仓库，放 CI secret 或本地环境变量；私包泄露 key 比泄露包更麻烦。

一个常见误区是“先把 jar 都塞进仓库再慢慢整理”。packwiz 的模型是反过来的：仓库里只有元数据，jar 由 installer 按需下载；这样仓库体积小、历史清晰，回滚也只回滚元数据。

## 九、KubeJS 7 的三个高频改造

1. 删配方：`ServerEvents.recipes(event => { event.remove({ id: 'minecraft:stick' }) })`。用 `id` 比用 `output` 稳，`output` 会把同产物的所有配方一起删掉。
2. 改标签：`ServerEvents.tags('item', event => { event.add('c:ingots/steel', 'kubejs:steel_ingot') })`。标签改错会连带影响所有引用它的配方，改完要用 JEI 抽查。
3. 加熔炉：`event.smelting('minecraft:iron_ingot', 'minecraft:iron_ore').xp(0.7).cookingTime(200)`。`cookingTime` 的单位是 tick，200 就是原版熔炉的 10 秒。

这三类都放在 `server_scripts/`，改完 `/reload` 即可生效；但如果你同时改了 `startup_scripts/` 的注册内容，必须重启，不能只靠 `/reload`。

## 十、任务书汉化的分工

- 章节文件只放结构与 ID，所有可读文本进 `lang/`；
- 一个章节对应一个翻译文件，文件名与 `chapters` 里的 `filename` 对齐；
- 用 `data.snbt` 的 `version` 判断结构代际（ATM10 是 `13`），跨代直接套文本会错位；
- 翻译后先在单人存档里开编辑模式走一遍依赖线，确认没有“任务存在但前置丢失”。

汉化最容易出的不是错译，而是“章节名翻了、任务里的物品名没翻”这种半截状态；用 `lang/` 统一管理可以避免二次维护。


## 十一、给非技术作者的极简方案

如果你只想做“给朋友的 30 个模组小包”，可以跳过 git：

1. 用 Prism Launcher 建一个实例，把模组装好、跑通；
2. 在实例目录执行 `packwiz init`（Minecraft 版本与加载器照实填），再 `packwiz refresh`；
3. 把 `pack.toml`、`index.toml` 与 `.pw.toml` 传到任意静态 HTTP 目录（GitHub Pages、对象存储都行）；
4. 朋友在 Prism 的预启动命令里填 `java -jar packwiz-installer-bootstrap.jar <你的 pack.toml URL>`。

这套流程不需要 CurseForge 账号，也不受审核影响；代价是没有公开页面、没有下载统计，版本回滚也只能靠自己留 tag。要公开发布，再走 CurseForge/Modrinth 的正式清单。公开发布前，至少找一位没参与制作的人按文档装一遍，能拦住大部分“我这能跑”的问题。

## 来源

- https://packwiz.infra.link/ （2026-11-15）
- https://packwiz.infra.link/reference/commands/packwiz/ （2026-11-15）
- https://packwiz.infra.link/tutorials/creating/getting-started/ （2026-11-15）
- https://github.com/packwiz/packwiz （2026-11-15）
- https://support.modrinth.com/en/articles/8802351-modrinth-modpack-format-mrpack （2026-11-15）
- https://kubejs.com/wiki/tutorials/recipes （2026-11-15）
- https://kubejs.com/wiki/tutorials/item-registry （2026-11-15）
- https://api.modrinth.com/v2/project/kubejs/version?game_versions=%5B%221.21.1%22%5D&loaders=%5B%22neoforge%22%5D （2026-11-15）
- https://raw.githubusercontent.com/AllTheMods/ATM-10/main/config/ftbquests/quests/data.snbt （2026-11-15）
- https://raw.githubusercontent.com/AllTheMods/ATM-10/main/config/ftbquests/quests/chapters/basic_tools.snbt （2026-11-15）

## 不确定项

- CurseForge `manifest.json` 的字段名来自行业通用格式，本轮没有抓到 CurseForge 官方逐字段文档；发布前应以 CurseForge 上传页的实际校验为准。
- FTB Quests 的重载命令未在本轮实测；不同版本可能只支持重进世界。
- packwiz 没有正式 Releases，文中不写具体版本号；不同构建的 `packwiz init` 交互提示可能略有差异。
- KubeJS 的 `2101.7.2-build.381` 是 2026-11-01 的 NeoForge 1.21.1 构建，Fabric 线版本号不同。
- Modrinth 与 CurseForge 的文件大小上限未在本轮核实。
