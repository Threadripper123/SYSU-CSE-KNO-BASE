# MC-26 启动器与实例管理：Prism、Modrinth App、CurseForge、Packwiz

> 主题库：Minecraft 现代版本与模组（2023–2026）　｜　源文件：（纯文本条目）　｜　8,981 字符　｜　39 个片段　｜　更新于 2026-11-15
> 资料截至 2026-11-15；覆盖 2023-11 至 2026-11，重点 2024–2026。

**一句话结论**：2026 年装 Minecraft 已经分成两条互不替代的路——图形启动器把每个整合包做成一个"自带 Java、自带 mods、自带存档"的实例（Prism Launcher、Modrinth App、CurseForge App），解决"能不能跑起来"；packwiz 这类元数据工具把实例变成可从 Git 复现的声明式配置，解决"换台机器还能不能一模一样"。真正省时间的做法不是二选一，而是用启动器管运行、用 packwiz 管版本。

## 一、关键变化表（2024–2026）

| 时间 | 对象 | 变化 | 对使用者的实际含义 |
| --- | --- | --- | --- |
| 2026-04-12 | Prism Launcher 11.0.2 | 11.0 系列稳定版进入维护 | 从 10.x 迁移的窗口期结束，wiki 与 issue 都以 11.x 为准 |
| 2026-07-11 | Prism Launcher 11.0.3 | 修复 7 月 16 日之后的 CurseForge 支持；在 CurseForge CDN 链接里带 API key；认证服务器宕机时离线模式更易用 | 旧版 Prism 在 7 月中旬后拉 CurseForge 文件会失败，必须升级 |
| 2026-09-03 | Prism Launcher 11.1.0 | 手动导入 Modrinth / CurseForge 整合包时，对"包内嵌 jar"给出警告；修大量磁盘占用问题 | "导入后 mods 目录里多出一堆 jar"从静默变成显式提示 |
| 2026-09-28 | Prism Launcher 11.1.1 | 针对 11.1.0 中一个安全漏洞的热修 | 11.1.0 不建议继续使用 |
| 2026-11-14 | Prism Launcher 12.0.0-pre1 | 官方称"自 10.0 以来最大的一次"，以 QoL、修 bug、现代化代码为主 | 预发布，只适合愿意提 issue 的人 |
| 2026-10-07 起 | Modrinth App 0.21.9 → 0.23.3 | 登录改用 deeplink 并修共享 token 导致的随机掉登录；共享实例进入 Library；实例设置加"恢复默认"与保存/放弃提示条 | 五周内连发十余个版本，属于高频迭代期 |
| 2026 年内 | packwiz | 仍是命令行工具，`pack.toml` + `index.toml` + 每模组一个 `.pw.toml` 的结构未变 | 学习成本集中在 TOML 与 Git，而不是 GUI |

## 二、四个工具在做不同的事

把四者放进同一张表比较，容易得出"哪个更好"的错误结论。它们的分工其实是这样：

| 方案 | 分发单位 | 更新方式 | 离线可用 | 适合谁 |
| --- | --- | --- | --- | --- |
| Prism Launcher | 实例目录（可导出 zip） | 启动器内检查更新 / 手动换 jar | 是，装好即离线 | 多版本并存、想要完全控制权的玩家 |
| Modrinth App | 实例 + 账号同步 | 应用内一键更新全部 mod | 是 | 只玩 Modrinth 生态、不想碰文件系统的人 |
| CurseForge App | 整合包（CF 项目） | 应用内更新 | 是 | 追 CurseForge 独占包、要与朋友用同一套包的人 |
| packwiz | `pack.toml`（元数据） | `packwiz update` + Git 提交 | 需先联网同步一次 | 整合包作者、服主、要复现的环境 |

关键差别在"实例"这个词的含义。启动器的实例是一个真实目录，里面有独立的 `.minecraft`、独立的 `mods`、独立的配置文件；packwiz 的"实例"只是一份清单，jar 在需要时才按清单下载。所以启动器能保证"这台机器现在能玩"，packwiz 能保证"三个人下周一装出来是同一个东西"。


## 三、Prism Launcher：实例就是目录

### 3.1 数据放在哪

Prism 把数据放在操作系统的标准应用数据目录；便携版则放在程序自己的目录里（菜单 `Folders > Launcher Root` 可直接打开）：

| 平台 | 默认路径 |
| --- | --- |
| Windows | `%APPDATA%\PrismLauncher` |
| Windows（Scoop） | `%HOMEPATH%\scoop\persist\prismlauncher` |
| macOS | `~/Library/Application Support/PrismLauncher` |
| Linux | `~/.local/share/PrismLauncher` |
| Flatpak | `~/.var/app/org.prismlauncher.PrismLauncher/data/PrismLauncher` |
| 便携版 | PrismLauncher 程序目录内 |

根目录下的子目录各有明确职责，理解了就不会乱删：`assets` 存游戏资源，`libraries` 存 Minecraft 与加载器依赖，`meta` 存版本元数据缓存，`java` 存启动器自己下载的 JDK，`instances` 放用户实例，`logs` 放日志，其余 `icons`、`themes`、`catpacks`、`skins`、`translations` 属于外观与皮肤，`cache` 是下载缓存。

### 3.2 实例 ID 与命令行

Prism 的实例 ID 就是实例文件夹名。右键实例 → `Instance Folder`，弹出的目录名就是 ID。这个 ID 是命令行接口的唯一入口：

- `prismlauncher -l <实例ID>` 直接启动某个实例
- `prismlauncher -l <实例ID> -s play.example.com` 启动并直连服务器（必须与 `-l` 同用）
- `prismlauncher -a <账户名> -l <实例ID>` 指定账户启动
- `prismlauncher -I <zip路径或URL>` 从 zip 导入实例
- `prismlauncher -d <目录>` 用自定义根目录（`.` 表示当前目录），这是做"多套隔离环境"的关键
- `prismlauncher --alive` 启动后写一个 `live.check` 文件，便于脚本判断启动器是否已就绪

这意味着"改名实例"不是纯外观操作：一旦文件夹改名，所有写死 ID 的快捷方式与启动脚本都会失效。要么一开始就用稳定的英文 ID，要么改名后同步改脚本。

### 3.3 Java 与内存：三个开关

Prism 全局 Java 设置里有三个开关值得记住：`Auto-detect`（扫描本机所有 Java，最优的排最前）、`Autodetect Java version`（启动时按版本自动选 Java，且只在自己管理的 Java 里找）、`Auto-download Mojang Java`（自动下载所需 Mojang 运行时，依赖上一项开启）。另有 `Skip java compatibility checks`，用于"我确定这个组合能跑，别拦我"的场景。设置页里的 `Test` 可以在不启动游戏的情况下验证 Java、内存与 JVM 参数组合。

官方文档对内存有一句很直接的话：**分配更多内存不等于更好**，除大型整合包外 4GB 通常够用。这与很多教程"能开多大开多大"的说法相反，原因是过量堆会让 GC 停顿变长，反而更卡。

### 3.4 11.x 的三个真实故障点

11.0.3 修的是 CurseForge 在 7 月 16 日之后的下载问题——CF 的 CDN 开始要求 API key，旧版本构造的直链会被拒。11.1.0 增加"导入包内含 jar"的警告，因为手动导入 `.mrpack` 或 CF zip 时，包里可能夹带作者塞进去的 jar，绕过正常下载校验。11.1.1 是安全热修。

另一个长期存在的坑在 Linux：Forge 安装器报 `Processor failed, invalid outputs`。CachyOS、Fedora 这类用 zlib-ng 替代 zlib 的发行版会触发，解决办法是在 `Edit > Settings > Java > Java arguments` 里加 `-Dforgewrapper.skipHashCheck=true`，或换回经典 zlib。


## 四、Modrinth App：账号体系带来的便利与绑定

### 4.1 目录与迁移

Modrinth App 默认把文件放在 Windows 的 `%AppData%\ModrinthApp\`。这个目录名在 **0.8.0** 从 `com.modrinth.theseus` 改成了 `ModrinthApp`，升级时自动迁移；如果实例"凭空消失"，除了检查当前 app 目录，还可以翻一下旧的 `%appdata%/com.modrinth.theseus`。

官方明确警告：不要把 app 目录改到需要管理员权限的位置（例如 `Program Files`），否则会坏。原因很朴素——Java 进程与更新器需要在该目录里反复读写，权限继承一旦出问题，表现就是"启动即崩"或"更新装不进去"，而日志通常不会告诉你真正原因。

### 4.2 0.21 → 0.23 的三个方向

第一是稳定性：0.21.8 修 `missing field synced_options` 报错，0.21.9 把登录改成 deeplink、修共享 token 造成的随机登出。第二是共享：0.22.0 起共享实例可以包含实例内任意文件（不再限于配置文件），并在推送更新时显示存储额度条；0.23.0 把"已加入但尚未安装"的共享实例显示在 Library，并加了安装状态与所有者筛选。第三是设置体验：0.23.2 给 Java/内存、窗口、启动钩子三个标签页的每个设置加了"恢复默认"，并加了保存/放弃提示条，减少"改了没生效"的困惑。

一个容易被忽略的观察：0.21.10 在设置的 troubleshooting 标签里加了 `Report ad` 入口。这说明应用内已经存在广告位，对把 Modrinth App 当"纯净启动器"的人来说，是需要重新评估的变化。

### 4.3 实例丢了怎么找回

官方帮助中心给的顺序是：先在 app 目录里确认实例文件夹是否还在（路径见设置里的 resource management），如果之前改过自定义目录就去旧目录找，从很老版本（例如 0.7.1）升级上来的再去 `com.modrinth.theseus` 找；找到后把实例文件夹整体移到别处，再逐个重新导入。反向的结论更重要：卸载时勾了 `Delete Application Data`，实例不可恢复。

## 五、CurseForge App：生态最大，但边界最硬

CurseForge 的优势是包的数量与独占项目，代价是闭环。两个具体约束值得写进文档：

第一，**包的完整性依赖 CF 项目**。CF 清单只记 `projectID` 加 `fileID`，启动器按 ID 去查；任何一个模组在 CF 上没有对应项目，导入时就会缺 mod。这也是"从别处搬来的包"在 CF 启动器里装不全的根本原因。

第二，**部分组件必须走浏览器**。Prism 的 CurseForge 集成页明确写着：某些整合包需要通过浏览器下载特定组件。也就是说，即使你用 Prism，遇到 CF 独占分发的包仍可能被踢回网页。

2026 年 7 月中旬 CF CDN 开始要求 API key，这件事同时影响 Prism（11.0.3 专门修）和其他第三方启动器。它说明一个结构性事实：CF 的"开放"是带条件的，第三方客户端要跟着它的鉴权策略走。


## 六、packwiz：把实例变成可 diff 的配置

### 6.1 文件结构

packwiz 不存 jar，只存元数据。三个文件构成全部：

- `pack.toml`：主文件，第一个被读取。必需字段 `index`（指向索引文件的对象）、`name`、`versions`（Minecraft 与加载器版本）
- `index.toml`：文件索引，记录每个被管理文件的路径与哈希
- `<mod>.pw.toml`：每个模组一个，记录来源（Modrinth / CurseForge / URL）、版本、side（client / server / both）

因为全是文本，它天生适合 Git：改一个模组版本就是一次 diff，回滚就是 `git revert`。

### 6.2 常用命令

| 命令 | 作用 | 什么时候用 |
| --- | --- | --- |
| `packwiz init` | 初始化，写 `pack.toml` | 新包第一步 |
| `packwiz refresh` | 重算 `index.toml` 哈希 | 手改过文件之后，忘了这步会"改了不生效" |
| `packwiz modrinth add` / `curseforge add` | 按来源加模组 | 日常加 mod |
| `packwiz update` | 按版本约束升级 | 每周维护 |
| `packwiz pin` / `unpin` | 锁住 / 解锁某个模组版本 | 某个 mod 升级会崩时 |
| `packwiz serve` | 本地起 HTTP 服务，默认 `http://localhost:8080/pack.toml` | 自测分发，每次请求自动刷新索引 |
| `packwiz modrinth export` / `curseforge export` | 导出 `.mrpack` / CF zip | 要公开发布时 |
| `packwiz migrate` | 迁移 MC 版本或加载器 | 整包升 1.21 → 26.x |

### 6.3 与启动器对接

packwiz 官方给的接法是"空实例 + 预启动命令"：建一个只有加载器和 MC 版本的空实例，把 `packwiz-installer-bootstrap.jar` 放进实例的 Minecraft 目录（和 `options.txt` 同级，可能是 `.minecraft` 也可能是 `minecraft`），然后在 `Edit Instance > Settings > Custom commands` 里勾选自定义命令，把下面这行填进 pre-launch：

`"$INST_JAVA" -jar packwiz-installer-bootstrap.jar https://你的服务器/pack.toml`

这样每次启动前都会对齐一次版本，玩家不需要手动更新。服务器端则把同一行放进启动脚本。这条链路的价值在于：客户端与服务端读同一个 `pack.toml`，从机制上消灭"你装了什么版本"的扯皮。

## 七、对玩家、作者、服主的影响

**玩家**：只玩一个包，Modrinth App 或 CurseForge App 足够；同时玩三四个不同版本，Prism 的实例隔离更省心。判断标准不是"哪个启动器强"，而是"你会不会同时开两个版本"。

**整合包作者**：用 packwiz 管版本，用 Prism 做测试实例，用 `packwiz modrinth export` 出 `.mrpack` 发布。反过来的顺序（先做包再想分发）通常会在发布当天发现清单对不上。

**服主**：客户端用 packwiz 预启动对齐，服务端用同一个 `pack.toml`；CF 独占包则要接受"部分文件得手动补"的现实，并在文档里写清楚。

## 八、常见坑

1. **把实例放进 OneDrive 同步目录**：Java 启动时文件被同步进程占用，表现为随机"文件被另一个程序使用"。
2. **便携版和安装版混用**：两者数据目录不同，看起来"实例全没了"，其实只是开了另一个根目录。用 `-d` 显式指定根目录可以避免。
3. **实例改名**：ID 即文件夹名，改名会断掉 CLI 脚本与快捷方式。
4. **多个启动器共用同一个 `.minecraft`**：配置、日志、崩溃报告互相覆盖，排错时看到的是别人的现场。
5. **无脑拉大 `-Xmx`**：官方文档明确说不是越大越好，4GB 起步更稳。
6. **packwiz 忘了 `refresh`**：手改文件后索引哈希不更新，安装端会认为文件"没变"或校验失败。
7. **CF 包缺文件**：清单里有的项目在 CF 上没有对应文件，导入即缺 mod，先核对 `modlist.html` 与 `manifest.json`。
8. **在 `Program Files` 里建 Modrinth app 目录**：权限问题导致的崩溃很难从日志看出来。

## 九、局限

本文只覆盖"启动与实例管理"这一层，不涉及服务端性能（见 MC-22、MC-24）与客户端渲染优化（见 MC-23）。CurseForge App 的具体版本号与内部目录结构未能从一手来源核实，故本文只写其机制性约束而不给版本数字。Prism 12.0 尚在预发布，正式版行为可能变化。

## 来源

- Prism Launcher 数据位置：https://prismlauncher.org/wiki/getting-started/data-location/ （访问 2026-11-15）
- Prism Launcher 命令行：https://prismlauncher.org/wiki/getting-started/command-line-interface/ （访问 2026-11-15）
- Prism Launcher Java 设置：https://prismlauncher.org/wiki/help-pages/java-settings/ （访问 2026-11-15）
- Prism Launcher 故障排查：https://prismlauncher.org/wiki/overview/troubleshooting/ （访问 2026-11-15）
- Prism Launcher 版本发布：https://github.com/PrismLauncher/PrismLauncher/releases （访问 2026-11-15）
- Modrinth App 发布记录：https://github.com/modrinth/code/releases （访问 2026-11-15）
- Modrinth App 目录位置：https://support.modrinth.com/en/articles/8797641-modrinth-app-storage-folder-location （访问 2026-11-15）
- Modrinth 实例找回：https://support.modrinth.com/en/articles/9977693-where-are-my-instances （访问 2026-11-15）
- packwiz 文档：https://packwiz.infra.link/ 与 https://packwiz.infra.link/tutorials/installing/packwiz-installer/ （访问 2026-11-15）
- Prism CurseForge 集成：https://prismlauncher.org/wiki/help-pages/flame-platform/ （访问 2026-11-15）

## 不确定项

- CurseForge App 在 2026 年的具体版本号、默认实例路径与 changelog 未能从一手来源取得（support.curseforge.com 的 sitemap 返回 404，下载页依赖 JS 渲染）。文中只写机制约束，未给版本数字。
- Modrinth App 的"共享实例"完整语义（是否需要账号、文件存放何处、额度上限具体数值）只从 release notes 条目推断，未读官方帮助文档的对应篇目。
- Prism 12.0.0 正式版发布日期未知；12.0.0-pre1 的完整 changelog 只读了开头部分。
- ATLauncher、GDLauncher、HMCL、PCL2 等启动器在 2026 年的状态未核实，本文未涉及。
- Prism 内部目录职责来自 wiki 表格，但 `cache` 与 `meta` 能否安全删除未在文档中说明。
