# 12A 抓取通道修正与 GitHub 实测（更正文档 12）

> 主题库：Minecraft 整合包：BMC 与 ATM 系列　｜　源文件：（纯文本条目）　｜　2,240 字符　｜　9 个片段　｜　更新于 2026-11-13
# 12A 抓取通道修正与 GitHub 实测（对文档 12 的更正）

> 文档 12 里有一条**错误结论**，本文更正。另外补充 GitHub 页面用真实浏览器抓取的结果。

## 1. 更正：Fandom 乱码不是「字体子集」，是 browser_open 丢字母 t

文档 12 原写「Fandom 该站字体映射异常导致提取文本缺字母」。**实测推翻了这个判断。**

用同样的 browser_open 抓 GitHub 的纯文本页面，得到完全一致的缺陷：

| 原文 | 返回 |
|---|---|
| `https://discord.gg/LunaPixel` | `h ps://discord.gg/LunaPixel` |
| `In-depth changelog of the new versions of the pack` | `In-dep h changelog of he new versions of he pack` |
| `Note: use some browser extension` | `No e: use some browser ex ension` |
| `Better Minecraft Studios` | `Be er Minecraf S udios` |
| `14 repositories` | `14 reposi ories` |

**规律**：只有小写字母 **t** 被吞掉，其它字符完好；数字、日期、版本号、标点全部无损。

**影响评估**：
- 可信：结构、链接（除 t）、数量、版本号、日期、表格关系。
- 不可信：任何含 t 的**人名、URL、命令、模组名、配方名**。
- 对策：逐字串一律用 `api.github.com`（JSON，精确）或 read_page（云渲染，无此缺陷）复核；browser_open 只用来判断「页面存在 / 结构长什么样 / 有多少条目」。

## 2. GitHub 页面的实测结果（Clash 代理 + 可见窗口）

| URL | 结果 | 备注 |
|---|---|---|
| `raw.githubusercontent.com/AllTheMods/ATM-10/main/CHANGELOG.md` | ✅ 200 | 裸 fetch 会 fetch failed；带代理的 browser_open 可读 |
| `github.com/AllTheMods/ATM-10/blob/main/CHANGELOG.md` | ✅ 200 | 渲染后的 Markdown 视图，比 raw 更干净（去掉了链接语法） |
| `github.com/orgs/LunaPixelStudios/repositories` | ✅ 200 | **必须开 scroll_rounds≥3**，否则只出首屏 10 个（实际 14 个） |
| `github.com/LunaPixelStudios` | ✅ 200 | 组织主页，含 67 followers、Australia、Discord 链接 |
| `lunapixelstudios.github.io` | ❌ 404 | 组织有 Pages 仓库但没发布内容，不是文档站 |
| `github.com/LunaPixelStudios/Better-MC-Forge-BMC4` | ❌ 404 | 该仓库不存在（BMC 无公开整合包仓库） |
| `api.github.com/repos/AllTheMods/ATM-10/releases` | ✅ 200 但 `[]` | 团队不用 Releases 发版 |
| `so.csdn.net/so/search?...` | ❌ `ERR_TIMED_OUT` ×2 | browser_search 一次、browser_open + 15s settle 一次，均超时；站点侧慢，与代理无关 |

## 3. 结论：GitHub 抓取的正确姿势（按优先级）

1. **要精确内容** → `api.github.com/repos/<owner>/<repo>/contents/<path>`，返回 base64，解码后逐字精确。
2. **要页面结构/条目数/仓库清单** → browser_open（代理 + 可见 + 滚动）。
3. **要渲染后的 Markdown** → `github.com/<owner>/<repo>/blob/<branch>/<file>`。
4. **不要**用裸 web_fetch 打 raw.githubusercontent.com（fetch failed）；**不要**依赖 browser_open 的文本做逐字引用（丢 t）。

## 4. 本次因改用浏览器才拿到的增量信息

- **ATM10 完整的版本时间线**（8.5 → 4.15，含每版日期）→ 见文档 13。
- **LunaPixelStudios 的 14 个仓库全表**，以及「BMC 没有公开整合包源码仓」这个关键事实 → 见文档 13。
- 「Better-Minecraft-Skyblock」是**独立仓库**，印证空岛版是独立维护的一条线。
