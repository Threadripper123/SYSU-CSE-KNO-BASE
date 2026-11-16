# 14 中文资源与国内通道实测（CSDN/B站/MC百科）

> 主题库：Minecraft 整合包：BMC 与 ATM 系列　｜　源文件：（纯文本条目）　｜　4,649 字符　｜　25 个片段　｜　更新于 2026-11-13
# 中文资源与国内通道实测（CSDN / B站 / MC百科）

> 用户指出「CSDN 我这边能搜」，实测确认：**站点没问题，是自动化浏览器走不通**。本文记录最终可用的国内通道，以及从中拿到的中文一手经验。

## 1. 国内通道实测矩阵（2026-11-13）

| 通道 | www.csdn.net | so.csdn.net 搜索页 | blog.csdn.net 文章页 |
|---|---|---|---|
| **web_fetch**（Node 直连） | ✅ 200（100k 字符） | ✅ 200（6k，SPA 空壳） | ✅ 200 |
| **read_page**（云端渲染） | — | — | ✅ 17.9k / 17.0k 字符，正文完整 |
| **browser_open**（本机自动化 Chrome） | ❌ 超时 | ❌ ERR_TIMED_OUT | ❌ ERR_TIMED_OUT |
| **browser_search** | — | ❌ ERR_TIMED_OUT | — |
| 用户自己的 Chrome | ✅ 正常 | ✅ 正常 | ✅ 正常 |

**结论**：browser_open 对 **csdn.net 全域名**都超时。试过 proxy 传空串、传 none、传 direct 三种写法，全部同样超时——说明**自动化浏览器在启动时就固定走 Clash，单次调用的 proxy 参数改不了它**，国内站走代理必挂。

> 这条推翻了我先前的判断「CSDN 站点侧慢」。真实原因是通道选错：**CSDN 不能用 browser_open，要用 web_fetch / read_page**。

## 2. CSDN 检索的正确姿势：直接打搜索接口

搜索页是 SPA，HTML 只有空壳；结果由接口返回：

'''
https://so.csdn.net/api/v2/search?q=<URL编码关键词>&t=all&p=1
'''

返回 JSON，关键字段：
- total：命中总数
- result_vos[]：结果数组，含 title、url、author、view、digg、create_time
- 标题里的关键词带 em 高亮标签，需要 strip

**注意**：响应体较大时会被 web_fetch 截断在 100000 字符，JSON 不完整 → 不要直接解析 JSON。做法是用正则在已返回的前段里抽 title 与 url 对，取前 8 条即可（结果按相关度排序，前几条就是最相关的）。

实测两条查询：
- All the Mods 10 整合包 → total **5276**
- Better MC BMC4 整合包 教程 → total **29**（中文圈对这个包的覆盖确实很少）

v3 接口也能用，但结构更杂（含 A/B 实验字段），v2 更干净。

## 3. 中文一手经验：ATM10 开服（CSDN，2026-05-28，V7.0）

来源：https://blog.csdn.net/havesten/article/details/158740432 （作者 Havesten；视频版 B 站 BV1cUZcBQEeX）

**包体认知**：1.21+、**500+ 模组**、NeoForge、千万级下载、内置任务系统、支持着色器、有终局目标，且**已更新 V7.0 并附带汉化**。

**服务器选型（中文社区经验，与官方 8–12GB 口径互补）**：

| 项目 | 建议 |
|---|---|
| CPU | 要高**主频**：14700K / 14900K / 7950X / 9950X |
| 核心数 | 4–6 核 |
| 内存 | **10GB+**（比官方 8GB 下限更保守） |
| 带宽 | **10Mbps**；经验值 **1Mbps ≈ 2 人** |
| 系统 | **不推荐 Windows 服务器**（系统自身吃内存/CPU），用 Linux / MCSM 面板 |
| Java | 面板环境选「纯环境 / Java 21」 |

**部署要点（MCSM 面板）**：安装时环境选纯环境 + Java 21；注意端口映射（示例 48652，内外端口都要放行）；不要在面板里重复装 Java 运行时。

## 4. 中文一手经验：BMC4 开服（CSDN）

来源：https://blog.csdn.net/kejibian/article/details/149189698 （2026-11-10 更新）

**包体信息（与英文源交叉一致）**：Better MC [FORGE] BMC4、LunaPixelStudios、1.20.1/1.20、Forge、**270+ 模组**、**100+ 任务**、冒险与 RPG / 原版增强 / 多人。

**核心模组清单（中文译名对照，便于和游戏内中文名对齐）**：

| 英文 | 中文 |
|---|---|
| Biomes o' Plenty | 丰富生物群系 |
| Geophilic | 地理学 |
| Mowzie's Mobs | 莫西的生物群 |
| Cataclysm | 大灾变 |
| When Dungeons Arise | 当地下城崛起 |
| AdoraBuild: Structures | 阿多拉建筑：结构 |
| The Aether | 以太 |
| Deep Aether | 深以太 |
| Aether Redux | 以太复刻 |
| Aether Villages | 以太村庄 |
| Waystones | 路石 |
| XP Tomes | 经验之书 |
| Xaero's Minimap | Xaero 的小地图 |
| Rideable Dragons | 可骑乘的龙 |
| Free World Sharing | 免费世界分享 |

**从零开服的完整链路（Debian 12 + MCSM + Docker）**：

1. 系统选 **Debian 12**；配置至少 4 核 8G，CPU 建议 **CPU-Z 单核 600+**。
2. APT 换源：运行 `curl https://chsrc.run/posix | bash`，再执行 `chsrc set debian`。
3. 装 MCSM 面板：先 `apt update`，装 wget 与 git，然后执行官方一键脚本 `wget -qO- https://script.mcsmanager.com/setup_cn.sh | bash`。装完访问 `http://<服务器IP>:23333/` 初始化账号。
4. 装 Docker：`apt -y install docker.io`；`systemctl enable docker` 后 `systemctl start docker`；用 `docker -v` 验证；换源 `chsrc set docker`；`systemctl daemon-reload` 后重启 docker 服务。
5. Java 镜像：面板「节点管理 → 镜像管理」创建 **OpenJDK 17**。
6. 上传服务端包，启动命令 `bash run.sh`；实例设置里启用 Docker 容器，镜像选 `openjdk:17`，**网络模式选 host**。
7. 客户端「多人游戏 → 添加服务器」，填公网 IP + 端口（默认 25565）。

> 注意版本差异：**BMC4 是 Forge 1.20.1 → Java 17**；**ATM10 是 NeoForge 1.21.1 → Java 21**。两篇教程用的 Java 版本不同，别抄错。

## 5. 中文资料索引

| 资源 | 地址 | 说明 |
|---|---|---|
| **MC百科 · Better MC 项目页** | https://www.mcmod.cn/modpack/205.html | 中文模组百科，查模组中文名/配方最方便 |
| CSDN · ATM10 开服教程 V7.0 | https://blog.csdn.net/havesten/article/details/158740432 | 含汉化、服务器选型、MCSM 部署 |
| CSDN · Better MC 开服联机教程 | https://blog.csdn.net/kejibian/article/details/149189698 | 含核心模组中文译名、Debian+MCSM 全流程 |
| CSDN · All The Mods 9 模组集合 | https://blog.csdn.net/gitblog_00087/article/details/139556301 | ATM9 中文介绍 |
| CSDN · packwiz 使用教程 | https://blog.csdn.net/gitblog_00317/article/details/154665321 | 用 TOML 做可自动更新的整合包分发 |
| CSDN · docker-minecraft-server ATM10 排障 | https://blog.csdn.net/gitblog_00353/article/details/151500924 | ATM10 服务器连接问题 |
| B 站 · ATM10 开服视频 | https://www.bilibili.com/video/BV1cUZcBQEeX | 上述 ATM10 教程的视频版 |
| B 站 · ATM10 空岛汉化安装 | https://www.bilibili.com/video/BV1qvFyzTE1z/ | ATM10 Sky 下载 + 汉化包 |

## 6. 中文资料的覆盖度判断

- **ATM 系列**：中文覆盖尚可（开服、汉化、面板部署都有），但**深度攻略（终局、配方链）几乎没有**，仍要以官方英文攻略站为准。
- **BMC 系列**：中文覆盖**极薄**。CSDN 全站搜「Better MC BMC4」只有 29 条命中，且多数是泛整合包开服教程套壳。**BMC 的玩法内容基本只能看英文源**。
- 中文源的价值集中在三块：**开服运维（MCSM / Docker / 换源）、汉化包、中文模组名对照**——这些英文源反而没有。

## 7. 给未来的自己：国内站抓取决策树

1. 先试 **web_fetch**（Node 直连，不走代理），最快，能判断站点是否可达。
2. 是 SPA / 需要渲染 → **read_page**（云端渲染，不走本机代理）。
3. 需要登录态（知乎、微信公众号、小红书）→ 才用 browser_open；**但 CSDN 是例外，它连不上自动化浏览器**。
4. 站内搜索优先用接口（CSDN 是 `api/v2/search`），比渲染搜索页稳。
