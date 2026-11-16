# MC-17 农业与生活模组 2025–2026：Mystical Agriculture、Productive Bees、Farmer's Delight 生态

> 主题库：Minecraft 现代版本与模组（2023–2026）　｜　源文件：（纯文本条目）　｜　6,095 字符　｜　29 个片段　｜　更新于 2026-11-15
> 资料截至 2026-11-15；覆盖 2023-11 至 2026-11，重点 2024–2026。

# MC-17 农业与生活模组 2025–2026：Mystical Agriculture、Productive Bees、Farmer's Delight 生态

一句话结论：农业线在 2026 年出现了清晰的三层结构——Mystical Agriculture 已经跟着 26.1.2 走、Productive Bees 停在 1.21.1 且半年没有新文件、Farmer's Delight 本体停在 1.21.1 但它的附属生态是三者中最活的。做包时"版本覆盖"比"内容多少"更决定成败。

## 关键变化表：三个本体的版本现状

| 模组 | 最新版本 | 发布日期 | 声明支持 | 加载器 | 体量参考 |
| --- | --- | --- | --- | --- | --- |
| Mystical Agriculture | 9.0.11（26.1.2）；8.0.29（1.21.1） | 2026-10-22 | 26.1.2 / 1.21.1 / 1.21 / 1.20.1 等 | NeoForge（1.21+）/ Forge（旧版） | CurseForge 1.495 亿、Modrinth 133 万下载 |
| Productive Bees | 1.21.1-13.13.0 | 2026-01-13 | 1.21 / 1.21.1（更早 1.20.1） | NeoForge / Forge | Modrinth 27.5 万下载 |
| Farmer's Delight | 1.21.1-1.3.4 | 2026-08-29 | 1.21 / 1.21.1（更早 1.20.1） | NeoForge / Forge | Modrinth 2583 万下载 |

## 附属生态：谁还在更新

| 附属模组 | 依赖本体 | 最新支持 | 最近更新 | 备注 |
| --- | --- | --- | --- | --- |
| End's Delight | Farmer's Delight | 1.21.1 | 2026-08-18 | 末地食材线，990 万下载 |
| Farmer's Respite | Farmer's Delight | 1.20.1 | 2026-06-13 | 茶与饮品线，20.7 万下载 |
| Nether's Delight | Farmer's Delight | 1.20.1 | 2023-09-04 | 下界食材线，324 万下载，已停更 |
| Cultural Delights | Farmer's Delight | 声明 26.1.2 / 26.2 / 26.3 | 2026-11-12 | 168 万下载，版本声明与本体不一致，需实测 |

这张表说明一件很实际的事：Farmer's Delight 本体只在 1.21.1，但附属模组里已经有人把版本号写到 26.x。这种"附属比本体跑得快"的情况通常意味着附属只用了本体的稳定 API、或者作者提前按 26.x 分支编译；装包时必须本体与附属一起实测，不能只看单个页面上的版本标签。

## Mystical Agriculture：把资源变成作物

Mystical Agriculture 的定位非常清楚：把矿石、生物掉落、甚至其它模组的材料都变成"可以种的作物"。它提供 130 多种资源作物，覆盖原版资源（铁、铜、金、红石、青金石、钻石、绿宝石、下界合金、紫水晶、蜜脾、珊瑚、深板岩等）、生物掉落（猪、牛、羊、僵尸、骷髅、苦力怕、蜘蛛、幻翼、岩浆怪、恶魂、末影人、凋灵骷髅、犰狳、旋风人等），以及一批常见模组资源（锡、银、铅、镍、铂、铱、铀、钨、钛、石墨、钢、青铜、黄铜、康铜、艾特尼姆等）。

作物产出的是各级精华（Essence）。本体的五级进阶是 Inferium → Prudentium → Tertium → Imperium → Supremium；更高阶的 Awakened Supremium 与 Insanium 由姊妹模组 Mystical Agradditions 提供（本文未逐条核对这两个物品的当前配方，见"不确定项"）。玩家用精华合成材料、工具、护甲，6 套工具与护甲可以用 Augments 继续升级。

围绕种植它给了完整的一套机器：

- Growth Accelerator：放在作物下方加速生长，可以堆叠；Watering Can 手动加速。
- Scythe 与 Sickle：范围收割，减少来回跑动。
- Harvester：自动收割田地。
- 可升级 Furnace：比原版熔炉更快、可加升级。
- Seed Reprocessor：把多余种子重新处理成可用资源，解决"种子爆仓"。
- Soul Extractor / Soulium Spawner / Soulstone：把生物精华转成刷怪能力。
- Enchanter：用多余精华给工具、护甲、书附魔。

版本上它是最积极的一个：2026-10-22 同一天给 26.1.2 发了 9.0.11、给 1.21.1 发了 8.0.29。双线同日更新说明作者把 26.x 当成一等公民，而不是"有空再说"。对整合包作者的含义是：如果你的包在 1.21.1，MA 是一个不会突然断供的依赖；如果准备上 26.x，它已经可用。

需要注意的前置是 Cucumber（同一个作者的公共库）。很多"装不上"的报错其实是漏了它。

## Productive Bees：把产线交给蜜蜂

Productive Bees 的思路是把"资源生产"外包给蜜蜂：不同蜂种产出不同资源，玩家负责搭蜂箱、配花、做繁殖。它的规模在农业类模组里算大的：200 多种蜜蜂，覆盖原版资源与常见模组资源（铜、铅、银、锡、镍、铀，以及神秘农业精华、Powah、Tinkers、Thermal 等模组专属资源），并提供最多 32 种高级蜂箱木变体（用其它模组的木材做）。

机制上有几个容易踩的点，官方 FAQ 写得很直白：

- 野生的大多数是独居蜂（solitary），不会住进蜂箱，只会在巢里生活、不产出资源，主要用来繁殖。要产出资源必须养"社会化"蜂种。
- 每种蜂有指定的花。JEI 里对着蜂按 U 可以查花的需求。
- 蜜蜂会记住原来的家。换了蜂箱要把蜂装进笼子，潜行状态下在目标蜂箱旁释放。
- 拆蜂箱时蜜蜂会留在里面，不需要精准采集。
- 想让新蜂入住巢：把巢放在对应生物群系，然后用 Honey Treat 点击巢，每多用一份缩短 10% 的倒计时。

配置层面它几乎全部外置：可以新增自定义蜂、禁用已有蜂、改产出资源与速率。这对整合包非常重要——你可以把"某个模组资源"的蜜蜂禁掉，逼玩家走别的产线。

但它的更新时间停在 2026-01-13（1.21.1-13.13.0），上一个版本是 2025-03-03 的 13.6.11。也就是说 2026 年它只发过一次版本，且没有 26.x 分支。在 1.21.1 上它依然是最好用的"蜜蜂产线"，但如果你的包要上 26.x，就必须提前找替代方案。

另一个实测坑：如果同时装了 Radium（或其它优化 mod），需要在 Radium 配置里加一行 mixin.ai.poi=false，否则蜜蜂的 POI 判定会被优化掉、出现"蜂不进箱"的怪象。这条写在作者 FAQ 里，不是玄学。

## Farmer's Delight 生态：本体轻、附属重

Farmer's Delight 本体走的是"温和扩展"路线：一个烹饪锅（Cooking Pot）做炖菜与汤，一个切菜板（Cutting Board）处理食材，一个炉灶（Stove）当热源与烹饪台，一把刀（Knife）提高采集与处理效率，再加上肥沃土壤（Rich Soil）、有机堆肥、橱柜与篮子这类装饰与存储。它的设计哲学是"不新增一套科技"，而是把原版的"种—收—做—吃"补完整。

体量上它是本组最大的：Modrinth 2583 万下载，远超另外两个。但从版本看，它只在 1.21.1 与更早版本；1.3.2（2026-05-13）→ 1.3.3（2026-08-17）→ 1.3.4（2026-08-29）是 2026 年比较密集的一段维护，之后就停了。

真正撑起"生态"的是附属模组：End's Delight 补末地食材并已跟到 1.21.1，Farmer's Respite 补茶与饮品但停在 1.20.1，Nether's Delight 补下界食材但 2023-09 之后就没动过，Cultural Delights 补各地料理并把版本声明写到了 26.x。做包时的组合原则很简单：本体锁定 1.21.1，附属只选同样声明 1.21.1 的，剩下的一律当"1.20.1 怀旧内容"。

值得注意的是，Farmer's Delight 的 CurseForge/Modrinth 描述页里那段 FAQ 还写着"目前只更新 1.18.2 与 1.19+"，与实际的 1.21.1-1.3.4 明显不同步。这不影响使用，但说明文档维护滞后——判断版本永远以文件列表为准，不要以描述页为准。

## 三件套在整合包里的分工

| 阶段 | Mystical Agriculture | Productive Bees | Farmer's Delight |
| --- | --- | --- | --- |
| 前期 | 种 Inferium 作物换基础资源 | 抓野生蜂、做繁殖 | 烹饪锅解决饥饿与 buff |
| 中期 | Growth Accelerator + Harvester 做半自动农场 | 高级蜂箱 + 花配置做资源蜂 | 附属食材线、刀与砧板提效 |
| 后期 | 精华替代矿机，Soulium Spawner 做掉落农场 | 全自动蜂场供料给其它模组 | 只作装饰与食物多样性 |

一个常见的包设计错误是"三件套全给前期"。这三者的曲线是互相替代的：MA 的精华一旦能稳定产出铁/金，PB 的金属蜂就失去意义；FD 的食物 buff 如果被 MA 的作物直接喂饱，烹饪线也会变成装饰。比较稳的做法是让它们服务不同目标——MA 供"大宗材料"、PB 供"稀有/模组专属材料"、FD 供"生存体验与 buff"。

## 性能与自动化成本

农业模组最大的性能陷阱不是模组本身，而是"玩家用它做出的东西"：

- Growth Accelerator 的堆叠会显著提高作物随机刻的结算频率。一片 9×9 的高加速农田，在服务器上比十个普通农场更吃 CPU。
- Harvester / Scythe 的范围收割如果接上管道自动输出，会形成"每 tick 检查库存"的隐形负载；用缓冲区 + 批次输出比直连管道稳。
- PB 的高级蜂箱在大量堆叠时会带来实体 AI 与 POI 查询压力；配合 Radium 时必须应用上面那条 mixin 配置。
- FD 的烹饪锅本身很轻，但"每个灶台都接漏斗"的布局会让漏斗成为主要 tick 来源。

经验值：一台 4 核 8G 的 1.21.1 服务器，给单个玩家的自动化农业预留 1–2 个区块的常加载区就够；超过这个范围，预生成与实体上限比换 CPU 更有效。

## 常见坑

- MA 的 8.x 与 9.x 是两个分支（1.21.1 与 26.1.2），配方与物品 ID 不保证一致，跨版本抄任务书会出错。
- MA 缺 Cucumber 会直接启动失败，且报错不一定点名 Cucumber。
- PB 的独居蜂不产资源，这是设计而不是 bug；新手最常见的抱怨就是"蜂不进箱"。
- PB 与 Radium 的 POI 冲突需要手动加 mixin.ai.poi=false。
- Nether's Delight 最后更新是 2023-09-04，只到 1.20.1，装进 1.21.1 包会缺配方。
- FD 描述页的版本 FAQ 已过时，判断版本看文件列表。
- Cultural Delights 声明支持 26.x，但 FD 本体只到 1.21.1；在 26.x 上装它属于未验证组合。

## 来源

- https://api.modrinth.com/v2/project/mystical-agriculture 与 /version?game_versions=["26.1.2"]、["1.21.1"]（访问 2026-11-15）
- https://www.curseforge.com/minecraft/mc-mods/mystical-agriculture（访问 2026-11-15）
- https://api.modrinth.com/v2/project/productivebees 与 /version（访问 2026-11-15）
- https://api.modrinth.com/v2/project/farmers-delight 与 /version（访问 2026-11-15）
- https://api.modrinth.com/v2/project/ends-delight 、/nethers-delight 、/farmers-respite 、/cultural-delights（访问 2026-11-15）
- https://api.modrinth.com/v2/project/productivebees 描述正文（含 FAQ 与 Radium 配置项，访问 2026-11-15）

## 不确定项

- Mystical Agriculture 的五级精华命名与 Awakened Supremium / Insanium 归属（Mystical Agradditions）来自长期稳定的模组常识，本次未在官方 wiki 上逐条复核，建议以游戏内 JEI 为准。
- MA 9.x 相对 8.x 的具体改动（配方、机器行为）未读 changelog。
- Productive Bees 13.13.0 相对 13.6.11 的改动未读 changelog；"200+ 蜂"是官方描述口径，实际数量取决于安装了哪些联动模组。
- Farmer's Delight 1.3.x 的具体新增内容未逐条核对，文中只描述了稳定的核心方块与物品。
- Cultural Delights 声明 26.x 是否真能在 26.1.2 上运行未实测。
