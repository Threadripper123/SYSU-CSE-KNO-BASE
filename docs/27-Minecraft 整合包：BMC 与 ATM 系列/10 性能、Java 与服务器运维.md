# 10 性能、Java 与服务器运维

> 主题库：Minecraft 整合包：BMC 与 ATM 系列　｜　源文件：（纯文本条目）　｜　8,114 字符　｜　41 个片段　｜　更新于 2026-11-13
# 性能、Java 与服务器运维

> 内容来自 ATM 官方 All The Guides 的 Help 分区（Java / Lag / Server / Backups / Commands / Configs），对任何大型 Forge/NeoForge 整合包都适用。

## 1. Java 版本对照（选错必定崩）

| Minecraft | 所需 Java |
|---|---|
| ≤ 1.15 | Java 8 |
| 1.16 | Java 8 或 Java 11 |
| 1.17 | Java 16 |
| 1.18+ | Java 17 |
| **1.21+** | **Java 21** |

Linux 安装示例：`sudo apt install openjdk-21-jdk`；下载渠道可选 Amazon Corretto、Azul Zulu、Oracle JDK，注意选 **x64** 架构。

**多版本共存的处理**：装多个 Java 时，默认「java」可能不是对的那个。
- 只装一个就够用，但**显式写全路径**更稳（如 `C:\Program Files\Eclipse Adoptium\jdk-21.0.4.7-hotspot\bin\java.exe`）。
- 或者建系统环境变量（服务端脚本里会用到，如 `ATM10_Java`）。
- Linux 用 `sudo update-alternatives --config java`。

## 2. JVM 参数

参数的作用：减少内存占用、减少卡顿尖峰（GC 更积极但单次停顿更小）。

### 客户端参数（启动器里设置）

- CurseForge：Settings → Minecraft → 向下滚到 Java 设置
- ATLauncher：包设置 → Java/Minecraft → Java Parameters 大框

**Java 8–11 完整参数（Aikar 经典版）**

```
-XX:+UseG1GC -XX:+ParallelRefProcEnabled -XX:MaxGCPauseMillis=200 -XX:+UnlockExperimentalVMOptions
-XX:+DisableExplicitGC -XX:+AlwaysPreTouch -XX:G1NewSizePercent=30 -XX:G1MaxNewSizePercent=40
-XX:G1HeapRegionSize=32M -XX:G1ReservePercent=20 -XX:G1HeapWastePercent=5 -XX:G1MixedGCCountTarget=4
-XX:InitiatingHeapOccupancyPercent=15 -XX:G1MixedGCLiveThresholdPercent=90
-XX:G1RSetUpdatingPauseTimePercent=5 -XX:SurvivorRatio=32 -XX:+PerfDisableSharedMem
-XX:MaxTenuringThreshold=1 -Dusing.aikars.flags=https://mcflags.emc.gs -Daikars.new.flags=true
```

**Java 17+ 客户端（精简版）**

```
-XX:+UnlockExperimentalVMOptions -XX:+UseG1GC -XX:G1NewSizePercent=20 -XX:G1ReservePercent=20
-XX:MaxGCPauseMillis=50 -XX:G1HeapRegionSize=32M
```

**Java 21 客户端**：同上（17+ 的精简版即适用）。

**内存 ≥12GB 的 Java 21 客户端可以试 ZGC**

```
-XX:+UseZGC -XX:+ZGenerational -XX:+DisableExplicitGC -XX:+AlwaysPreTouch
-XX:+PerfDisableSharedMem -XX:+UseDynamicNumberOfGCThreads
```

### 服务端参数（写在 server 文件夹的 `user_jvm_args.txt`）

**Java 17 服务端**

```
-Xms4G -Xmx6G -XX:+UseG1GC -XX:+ParallelRefProcEnabled -XX:MaxGCPauseMillis=200
-XX:+UnlockExperimentalVMOptions -XX:+DisableExplicitGC -XX:+AlwaysPreTouch
-XX:G1NewSizePercent=30 -XX:G1MaxNewSizePercent=40 -XX:G1HeapRegionSize=8M
-XX:G1ReservePercent=20 -XX:G1HeapWastePercent=5 -XX:G1MixedGCCountTarget=4
-XX:InitiatingHeapOccupancyPercent=15 -XX:G1MixedGCLiveThresholdPercent=90
-XX:G1RSetUpdatingPauseTimePercent=5 -XX:SurvivorRatio=32 -XX:+PerfDisableSharedMem
-XX:MaxTenuringThreshold=1
```

**Java 21 服务端（通用）**

```
-Xms4G -Xmx6G -XX:+UnlockExperimentalVMOptions -XX:+UseG1GC -XX:G1NewSizePercent=20
-XX:G1ReservePercent=20 -XX:MaxGCPauseMillis=50 -XX:G1HeapRegionSize=32M
```

**Java 21 服务端（4 核以上 + 8–12GB 内存，ZGC）**

```
-Xms8G -Xmx12G -XX:+UseZGC -XX:+ZGenerational -XX:SoftMaxHeapSize=10g
-XX:+DisableExplicitGC -XX:+AlwaysPreTouch -XX:+PerfDisableSharedMem
-XX:+UseDynamicNumberOfGCThreads
```

## 3. 卡顿定位（Spark / Observable）

大多数 ATM 包默认带 **Spark** 与 **Observable**。

### Spark

- 普通玩家可用：`/spark tps`、`/forge tps`——先看整体 TPS。
- 单人开作弊或服务器 OP：`/spark profiler --timeout 180` → 3 分钟采样，返回一个网页链接，可交互查看所有线程。
- 抓间歇性卡顿：`/spark profiler --only-ticks-over 100 --timeout 60`。
- **客户端 FPS 问题用 `sparkc`**（同样参数）。
- 常用补充：`/spark health`（可加 `--memory`/`--network`）、`/spark tickmonitor`、`/spark gc`、`/spark heapsummary`、`/spark heapdump`。
- `--thread *` 可强制记录 Spark 默认没抓到的线程。

### Observable

- 同样是 OP 权限；先在按键设置里绑一个键打开 UI。
- 点「profile TPS」→ 完成后给出链接或游戏内表格，列出最耗 TPS 的方块/实体。
- 会用**绿→红色谱**在世界上高亮相关方块，红色最差。
- 1.20+ 默认在 ATM9 中启用，**必须自己绑键**。网页上的「Visit」按钮需要 OP。

## 4. 实体泄漏与 Item Clear

**Crash Utilities** 默认随包提供，但 **Item Clear 功能是关闭的**。开了它能防止农场/自动化造成的实体爆炸（这是服务器卡顿和崩溃的常见原因）。

开启方法：打开 `world/serverconfigs/crashutilities-server.toml`，最顶部把 `enabled` 设为 `true`。计时器是检查间隔（分钟），超过阈值会在聊天栏公告并清理。

**改配置前必须停服/退出世界**（客户端在非世界中时改 world/serverconfigs 是安全的）。

### 清理命令

- 清掉落物：`/kill @e[type=minecraft:item]`（可能需要执行多次）
- 清所有非玩家实体：`/kill @e[type=!minecraft:player]`
- 查看实体列表：`/cu entities list`
- 手动触发清理（需先在配置启用）：`/cu callItemClear`
- 把玩家（含离线）传回出生点：`/cu unstuck <name>`
- 打开玩家背包（含离线）：`/cu inventory <name>`

## 5. 服务器搭建

### 前置

- **至少 6GB 内存**（多数 ATM 包）
- 与包 MC 版本匹配的 Java

### 步骤

1. CurseForge 该包页面 → Files → 选版本 → 展开 **Additional Files** → 下载 **Server Files**。
2. 解压到一个文件夹。**装了第三方杀软的话，把该文件夹加白名单**，否则解压后文件可能被删/损坏。
3. **Windows 不要解压到 OneDrive 等云同步目录**；Linux 确保目录读写权限、脚本可执行，建议放 `/home/<user>/` 下。
4. 首次运行 `startserver.bat`（Linux `./startserver.sh`，先 `chmod +x`）。**首次运行会生成大量文件，中途不要关**。
5. 生成 `eula.txt` 后改成 `eula=true`。
6. 按需改 `server.properties`、configs、`world/serverconfigs`，再启动。
7. 看到 `Dedicated server took xx seconds to load` 就成功了。

**警告**：网上很多教程让你改 `server-ip` 或 `server-port`。**大多数情况下都不需要改，尤其 server-ip**，改错会导致服务器起不来。

### 没有 startserver 脚本？

- NeoForge 先装服务端：`java -jar neoforge-installer*.jar --installServer`，之后用 `run.bat/sh`。
- 老版本 Forge 需自己写脚本：`java -Xms4G -Xmx8G -jar forge-*universal.jar`。
- 加参数到 `user_jvm_args.txt`；老 Forge 没有该文件就写进脚本。
- 改内存：改 `user_jvm_args.txt` 里的 `-Xms` / `-Xmx`。

### 导入世界

把 world 文件夹上传到服务器目录，**名字必须是 `world`**。

### 托管商（无 NeoForge 安装器时）

- 上传解压后的 Server Files（FTP/SFTP 或在线文件管理器）。
- 需要 `.jar` 时：新版 Forge 不能直接跑 universal jar，用 **ServerStarterJar**（NeoForge 与 Forge 1.17 及更早用 neoforged/ServerStarterJar）。
- 或写 `server-setup-config.yaml`（ATM9 示例关键字段）：

```
_specver: 2
modpack:
  name: ATM9
  install:
    mcVersion: 1.20.1
    loaderVersion: 47.3.7
    installerUrl: "https://files.minecraftforge.net/maven/net/minecraftforge/forge/{{@mcversion@}}-{{@loaderversion@}}/forge-{{@mcversion@}}-{{@loaderversion@}}-installer.jar"
    installerArguments: ["--installServer"]
    modpackFormat: zip
  launch:
    maxRam: 8
    minRam: 4
    startFile: "forge-{{@mcversion@}}-{{@loaderversion@}}.jar"
    supportedJavaVersions: [17]
```

字段含义：`mcVersion` = MC 版本，`loaderVersion` = Forge 版本，`supportedJavaVersions` = 支持的 Java。

## 6. 更新服务器（先备份世界）

1. **自动**：有整合包安装器的托管商通常支持一键更新。
2. **手动 A（原地替换）**：备份改过的 configs / kubejs → 删除 `kubejs`、`defaultconfigs`、`mods`、`config` → 若 Forge 版本变了，删 `libraries` 并换新的启动脚本/安装器 → 从 Server Files 解压回这些目录。
3. **手动 B（新目录迁移）**：解压新 Server Files 到新文件夹 → 拷贝 `world`、`local`、`server.properties`、`eula.txt`（有 journeymap 也拷）→ 拷回改过的配置/脚本。

## 7. 备份

### 该备份什么

- `options.txt`：菜单、音量、视频设置
- `keys.txt`：全部按键绑定
- 你自己改过的 `packInstall/configs` 里的文件（**不要整份复制 configs 文件夹**，跨版本复制会损坏安装）
- 世界存档
- （可选）Journeymap 路径点：`packinstall/journeymap/data/sp`

### SimpleBackups

- 客户端与服务端都带，**但服务端默认未启用**。
- 启用：到 `mods` 文件夹把 `SimpleBackups.jar.DISABLED` 去掉 `.DISABLED` 后缀。
- 默认保存到 `packInstall/simplebackups`，zip 名为世界名 + 时间戳。
- 默认间隔 **每 2 小时（120 分钟）**，可在 `configs/simplebackups-common.toml` 改。

### 恢复（CurseForge）

1. 完全关闭整合包。
2. 整合包图标右键 → Open Folder → 打开 `simplebackups`。
3. 打开要恢复的 zip（按日期命名，选最新），把里面的文件夹复制出来。
4. 到 `saves` 里把旧世界**先备份再删除**，然后粘贴回去。
5. 如果恢复失败，试更早的一份备份。

ATLauncher 流程相同，只是文件夹入口不同。

## 8. 常用命令速查

| 需求 | 命令 |
|---|---|
| 回出生点 | `/spawn` |
| 看 Forge TPS | `/forge tps` |
| 清掉落物 | `/kill @e[type=minecraft:item]` |
| 清非玩家实体 | `/kill @e[type=!minecraft:player]` |
| 清空背包 | `/clear` |
| 服务端采样 | `/spark profiler --timeout 30` |
| 服务端健康 | `/spark health` / `--memory` / `--network` |
| TPS 采样 | `/spark tps` |
| 网络延迟 | `/spark ping` |
| 改认领区块 | `/ftbchunks admin extra_claim_chunks <player> <set/add> <amount>` |
| 改强加载区块 | `/ftbchunks admin extra_force_load_chunks <player> <add/set> <amount>` |
| 绕过区块认领 | `/ftbchunks admin bypass_claims` |
| 卸载所有强加载 | `/ftbchunks admin unload_everything` |
| 清空所有认领 | `/ftbchunks admin unclaim_everything` |
| 任务编辑模式 | `/ftbquests editing_mode true` |
| Mahou 点数 | `/showmahou` |
| 设置 Morgan 伤害 | `/mahouset morgan @p 5000000` |
| 墓碑 GUI | `/tbgui` |
| 绑定传送点 | `/tbbind set 1-5` / `/tbbind teleport 1-5` |
| 传送到死亡点 | `/tbteleport death` |
| 找回墓碑 | `/tbrestoreinventory` / `/tbshowlastgraves` |

## 9. 配置文件位置

| 文件 | 作用 |
|---|---|
| `config/` | 包级配置 |
| `<世界名>/serverconfigs/` | 世界级配置（服务端规则） |
| `world/serverconfig/ftbessentials.snbt` | /home、/tpa、/back、/rtp |
| `world/serverconfig/ftbranks/ranks.snbt` | 权限组 |
| `world/serverconfig/noflyzone.snbt` | 禁飞维度 |
| `world/serverconfig/ftbchunks-world.snbt` | 区块加载模式（`force_load_mode`） |
| `config/mysticalagriculture-common.toml` | 假玩家浇水等 |
| `configs/simplebackups-common.toml` | 备份间隔 |
| `world/serverconfigs/crashutilities-server.toml` | Item Clear |
