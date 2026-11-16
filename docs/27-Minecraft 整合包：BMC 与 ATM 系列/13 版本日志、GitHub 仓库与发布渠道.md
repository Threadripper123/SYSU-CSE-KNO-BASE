# 13 版本日志、GitHub 仓库与发布渠道

> 主题库：Minecraft 整合包：BMC 与 ATM 系列　｜　源文件：（纯文本条目）　｜　2,794 字符　｜　9 个片段　｜　更新于 2026-11-13
# 版本日志、GitHub 仓库与发布渠道

## 1. ATM10 版本时间线（GitHub CHANGELOG.md 实测）

ATM 团队在仓库根目录维护 `CHANGELOG.md`，**这是唯一能拿到完整版本节奏的地方**（GitHub Releases 是空的）。

| 版本 | 日期 |
|---|---|
| **8.5** | **2026-11-04** |
| 8.4 | 2026-10-29 |
| 8.3 | 2026-10-15 |
| 8.2 | 2026-09-22 |
| 8.1 | 2026-08-29 |
| 8.0 | 2026-08-14 |
| 7.3 | 2026-08-02 |
| 7.2 | 2026-07-20 |
| 7.1 | 2026-06-26 |
| 7.0 | 2026-05-15 |
| 6.6 | 2026-04-07 |
| 6.5 | 2026-04-07 |
| 6.4 | 2026-04-04 |
| 6.3 | 2026-04-02 |
| 6.2 | 2026-03-24 |
| 6.1 | 2026-03-07 |
| 6.0 | 2026-02-25 |
| 5.5 | 2026-01-31 |
| 5.3 | 2025-12-02 |
| 5.2 | 2025-11-28 |
| 5.1 | 2025-11-14 |
| 5.0 | 2025-11-13 |
| 4.15 | 2025-11-11 |

**读法**：
- 2025-11-13 进 5.0，2026-02-25 进 6.0，2026-05-15 进 7.0，2026-08-14 进 8.0——**约每 3 个月一个主版本，主版本内 2–4 周一个小版本**。
- 2026-04-02/04/07 连着 6.3/6.4/6.5/6.6 四个版本，是典型「发版后热修」节奏。
- 引用版本必须带日期，否则「8.5」过几周就不再最新。

> CHANGELOG 每节只有标题行被渲染出来，**具体改动仍需看 CurseForge Files 说明或 Discord 公告**。

## 2. LunaPixelStudios 的 GitHub 仓库（BMC 作者）

组织页实测共 **14 个仓库**：

| 仓库 | 说明 |
|---|---|
| **Modpack-Update-Checker** | 整合包更新检查工具 |
| **SteamPunk** | 整合包（9★ / 2 fork / 16 issues） |
| **Fear-Nightfall** | 整合包 |
| **Medieval-MC** | 整合包（CSS 为主） |
| **LunaPixelStudios.github.io** | Pages 仓库，**未发布内容（访问 404）** |
| **Fantasy-Realm** | 整合包，指向 CF `stay-true` |
| **Versatile** | 整合包（CSS） |
| **Performatic** | 整合包，指向 CF `performatic` |
| **mpuc** | Modpack-Update-Checker 的缩写仓库 |
| **EternalAbyss** | 整合包 |
| **The-Abyss-Chapter-II** | 模组，指向 CF `the-abyss-chapter-ii` |
| **Better-Minecraft-Skyblock** | **BMC 空岛版相关仓库** |
| **When-Dungeons-Arise** | 结构模组（1★ / 5 issues） |
| **Eternal-Abyss** | 整合包，指向 CF `the-abyss-official-server` |

**结论（很重要）**：
1. **BMC1–BMC5 没有公开的整合包源码仓库**。本体只走 **CurseForge / Modrinth**，GitHub 上只有周边项目。
2. 查 BMC 版本历史**只能去 CurseForge Files** 或 Modrinth Versions，别指望 GitHub CHANGELOG。
3. 「Better-Minecraft-Skyblock」是**独立仓库**，说明空岛版是一条独立维护的线，不是主包里的一个模式。
4. 组织另有 Medieval-MC、SteamPunk、Fear-Nightfall、Fantasy-Realm、Versatile、Performatic、Eternal Abyss 等**其它整合包**——说明 LunaPixel 不只做 BMC，查资料时别把这些包的内容混进 BMC。

## 3. 发布渠道对照表

| 项目 | 版本历史 | 更新日志 | 反馈 |
|---|---|---|---|
| **BMC1–BMC5** | CurseForge Files / Modrinth Versions | CurseForge 版本说明 | CurseForge Issues、Discord |
| **BMC Skyblock** | Modrinth Versions | 同上 | 同上 |
| **ATM9 / ATM10** | CurseForge Files + GitHub `CHANGELOG.md` | GitHub CHANGELOG 标题行 + CurseForge Files | GitHub Issues（`AllTheMods/ATM-9`、`ATM-10`）+ Discord |
| **ATM10 To the Sky** | CurseForge Files | CurseForge | Discord |

## 4. 抓这类页面的实操要点

- `raw.githubusercontent.com/...` 裸 fetch 会 `fetch failed`，**必须用带 Clash 代理的 browser_open**。
- browser_open 文本**丢字母 t**（见文档 12A），版本号+日期可信，正文措辞不要逐字引用。
- 要精确原文走 `api.github.com/repos/<o>/<r>/contents/<path>`（base64 解码）。
- 组织仓库列表用 `github.com/orgs/<org>/repositories` 且 `scroll_rounds: 3`，否则只出首屏 10 个。
