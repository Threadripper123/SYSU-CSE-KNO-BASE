# 09 ATM 农业、养蜂与刷怪模组

> 主题库：Minecraft 整合包：BMC 与 ATM 系列　｜　源文件：（纯文本条目）　｜　6,237 字符　｜　28 个片段　｜　更新于 2026-11-13
# ATM 农业、养蜂与刷怪模组

## 1. Mystical Agriculture（神秘农业）

### ATM9 的灌注改动

- ATM9 新增 **分级水晶（Tiered Crystals）**：升级必须用**同档水晶**（例如要做 Supremium Essence，必须用 Imperium Crystal）。
- **Master Infusion Crystal 现在需要 4 个 Supremium Insanium**。

### 常见问题

**Q：洒水壶/镰刀不工作？**
神秘农业默认**禁止假玩家浇水**。改 `config/mysticalagriculture-common.toml` → `fakePlayerWatering = true`。

**Q：模块化路由器不工作？**
因为你工作在已认领区块里，需要给路由器加 **Security Upgrade**（自己的区块也要）。

### 全自动产能：AE2 + 热力膨胀 Phytogenic Insolator（KJM 方案）

这是 ATM9 里最经典的「精华农场」设计，本质是一台**用 AE2 驱动的巨型作物工厂**。

**最小可行材料**：3× ME 接口、2× ME 存储总线、2× 石英纤维/线缆锚、2× AE2 合成卡、2× Phytogenic Insolator、1× ME 控制器、1× 量子环对、1× Sink/Eternal Water Block、若干染色线缆与致密线缆。

**满配材料**：129× ME 接口、2× ME 存储总线、2× 石英纤维、128× AE2 合成卡、256× Phytogenic Insolator、1× ME 控制器、1× 量子环对、1× 无限水源、1× 某色线缆、2× 另一色线缆、68× 第三色致密线缆。

**「大脑」（控制核心）**：
1. 在量子环的两个角放无限水源和 ME 接口，两者之间放 2 个石英纤维。
2. 上面放 2 根同色线缆 + ME 控制器；另一色线缆把量子环和 ME 接口连起来。
3. **确保两个量子环已配对**。
4. 在 ME 接口与水源上各贴一个 ME 存储总线，相对放置。
5. ME 接口里**备货 Water 和 Phyto-Gro**，优先级设 **−1**。
6. 水源上的存储总线设为 **Extract Only**，过滤 Water。

**模块（每列）**：
- 每列 = 2 台 Phyto + 两侧 ME 接口，**高 8 格**（因为无控制器的 AE2 子网只有 8 通道）。
- 每面墙用满一根致密线缆的 32 通道。
- ME 接口插**合成卡**，过滤 Water 与 Phyto-Gro。
- Phyto 面向接口的那一侧设为 **Auto Input/Output**。
- 每台 Phyto 装 **1× Abyssal Integral Component + 3× Abyssal Flux Linkages**（最高效率组合；没凑齐就用低一档的）。
- 最多堆 **8 高**，排成 **4 面墙**。

**省事技巧**：
- 用 AE2 **Memory Card** 复制 ME 接口设置（Shift+右键复制，右键粘贴）。
- 用 **Redprint** 复制 Phyto 设置（右键复制/粘贴）。

**接线与供电**：致密线缆每个分支带 4 列接口；Phyto 用任意能量管道供电。

## 2. Productive Bees（产能蜜蜂）

官方攻略站收录了 Kaimonick 与 Jonh09 两套指南，以下是**基因系统**的核心，也是最容易走弯路的部分。

### 六种基因

| 基因 | 作用 | 等级 | 最佳 |
|---|---|---|---|
| Productivity（产量） | 每趟产更多蜂巢 | Normal / Medium / High / Very High | **Very High** |
| Weather Tolerance（天气耐受） | 能在什么天气工作 | None / Rain / Any | **Any** |
| Behavior（作息） | 什么时段工作 | Diurnal / Nocturnal / Metaturnal | **Metaturnal** |
| Endurance（体质） | 最大生命 | Weak / Normal / Medium / Strong | Strong |
| Temper（性情） | 攻击性 | Passive / Normal / Aggressive / Hostile | Passive |
| 第 6 种（蜂种基因） | 用于制作该蜜蜂的刷怪蛋 | — | 按需 |

### 提取基因

1. 3 格高空间：底部放 **Bottler**（装好玻璃瓶），顶部放**朝下的活塞** + 按钮/拉杆。
2. 把蜜蜂放到中间，快速按按钮 → 蜜蜂被压成 **Squashed Bee Material**。
3. 丢进 **Centrifuge（离心机）** → 得到 6 种基因。

**每次只给约 20% 纯度的基因，要重复 4–5 次**才能凑满。把基因与 **Honey Treat** 在工作台合成，然后**右键蜜蜂**即可赋予该性状。

### 半自动基因农场

1. **投放**：原版发射器 + RFTools Timer，每 20 tick 发一次脉冲，把笼中蜜蜂放到活塞区。用**一次性笼子**避免杂物。
2. **压榨**：活塞保持伸出状态，蜜蜂被推进去就自动压扁；记得给 Bottler 持续供瓶。
3. **收集**：任意 Item Collector 收 Squashed Bee Material → 送离心机。
4. **提纯**：基因送入 **Gene Indexer**，给红石信号后会把同种基因合并到 **100%**。
5. **筛选（可选）**：Pipez 物品管道按 NBT 过滤 100% 基因：`item: productivebees:gene`，`NBT: {productivebees_gene_purity:100}`。

### 最佳基因的获取方式

- **Productivity: Very High** —— 只能来自 **Kamikaze Bee（神风蜂）**。戴 **Bee Nest Helmet**（钻石头盔 + 蜂巢）被敌对生物打中时，有 **30%** 概率生成神风蜂。它们极小极快，建议在封闭空间内、手持捕虫笼操作。抓 4–5 只压榨，基本能出 100% 基因。**神风蜂之间不能繁殖。**
- **Behavior: Metaturnal** —— 把蜜蜂放在外面几天，会从 Diurnal 变 Nocturnal 再变 Metaturnal。用拴绳 + 栅栏固定，并定期喂 Honey Treat（在外面会掉血）。
- **Weather Tolerance: Any** —— 两条路：慢慢放外面适应；或**钓鱼**。在**河流**群系用普通钓竿钓，有概率钓出 **Water Bee**（会很生气，准备好笼子），抓 2 只繁殖。

**推荐做法**：把好基因集中到一只易繁殖的蜜蜂上 → 繁殖 → 压榨 → 用得到的基因做 Honey Treat（可一次放多个基因，俗称「蜜蜂激素」）→ 给所有蜜蜂升级。

### 繁殖时的基因继承（重点）

- **左侧蜜蜂是「主蜂」**，基因有优先级；**右侧蜜蜂只决定基因等级范围**。
- 子代**不会低于主蜂的等级**。例：主蜂 Productivity=Medium、副蜂=Very High，子代可能是 Medium / High / Very High。
- 所以**先把主蜂拉满**，再用它当主蜂繁殖。
- **ATM10 / 1.21+ 变化**：**父母双方必须拥有相同基因，子代才会得到相同基因**。

### 自动化 NBT 参考

- 导入幼蜂到 BaBee Incubator（白名单）：`item: productivebees:bee_cage`，`NBT: {"minecraft:custom_data":{Age:-24000}}`
- 导出成蜂：同 tag，改**黑名单**。

## 3. Apotheosis（ATM9 附魔体系）

ATM9 的附魔上限远超原版，核心是**堆特定书架 + 灌注**。

### 起步：Seashelf 循环

1. 做 16 个 **Seashelf**，15 个摆成附魔台阵型。
2. 第 16 个放进附魔台**灌注**（3 EXP），替换掉一个未灌注的，重复到全部 16 个都是 **Infused Seashelf**（会多出 1 个）。**需要等级 45+**。
3. 破掉 9 个 Infused Seashelf → 做 2× **Heart-Forged Seashelf** + 7× **Crystalline Seashelf** 摆回原位。
4. 做 10× **Dormant Deepshelf** 并灌注成 **Deepshelf**（**等级 60+**）。
5. 用它们做 5× **Soul-Touched Deepshelf** + 5× **Echoing Deepshelf**，再升级成 **Sculkshelf** 变体（JEI 查配方）。
6. 做 3× **Shelf of End-Fused Rectification** + 1× **Draconic Endshelf**。
   - **Infused Dragon's Breath**：摆 10 个 Sculkshelf + 10 个 **Melonshelf** 来灌注龙息（**等级 80+**）。
7. 移除所有 Melonshelf，摆上 3 个 End-Fused Rectification + Draconic Endshelf → **满级附魔台**。

### 关键灌注配方（物品 → 书架组合）

| 目标 | 书架 |
|---|---|
| Unbreakable Potion Charms | 5× Draconic Endshelf、2× Echoing Deepshelf、1× Melonshelf、1× Endshelf |
| Tome of Superior Scrapping | 3× Echoing Deepshelf、1× Soul-Touched Deepshelf、1× Endshelf、1× Draconic Endshelf |
| Tome of Extraction | 3× Echoing Deepshelf、2× Draconic Endshelf、1× Soul-Touched Deepshelf |
| Library of Alexandria | 7× Echoing Deepshelf、3× Draconic Endshelf、2× Soul-Touched Deepshelf |
| Superior Sigil of Socketing | **0.3.0 版本已移除**（原：5× Echoing Deepshelf、2× Draconic Endshelf、2× Soul-Touched Deepshelf） |

### Infused Teleportation Core（ATM Star 必需件）

每种都有两套方案，挑好凑的那套：

- **Nether**：满级架方案 = 10× Melonshelf、5× Soul-Touched Sculkshelf、5× Echoing Sculkshelf；替代方案 = 8× Glowing Hellshelf、8× Hellshelf、3× Melonshelf、2× Echoing Deepshelf、1× Draconic Endshelf
- **Ender**：满级架方案 = 4× Draconic Endshelf、3× Melonshelf、2× Echoing Sculkshelf、1× Soul-Touched Sculkshelf；替代方案 = 4× Draconic Endshelf、2× Echoing Deepshelf、2× Glowing Hellshelf、2× Infused Seashelf、1× Stoneshelf、1× Melonshelf
- **Draconic**：满级架方案 = 7× Melonshelf、5× Soul-Touched Sculkshelf、5× Echoing Sculkshelf、1× Draconic Endshelf；替代方案 = 8× Deepshelf、4× Echoing Deepshelf、2× Draconic Endshelf、1× Melonshelf、1× Oak Bookshelf

### 刷怪笼改动

| 效果 | 原来 | 现在 |
|---|---|---|
| Remove AI | Chorus Fruit | **Golden Apple** |
| Ignore Players | Nether Star | **Conduit** |
| Increase Entities | Ghast Tears（上限 32） | **Piglich Heart**（上限 16） |
| Increase Spawn Count | Fermented Spider's Eye（上限 16） | **Piglich Heart**（上限 8） |
| Decrease Min Spawn Delay | Sugar（下限 20） | **AllTheModium Ingot**（下限 100） |
| Decrease Max Spawn Delay | Clock（下限 20） | **Unobtainium Ingot**（下限 100） |

设计意图：让「去掉 AI」更容易、给潮涌核心更多用途、鼓励玩家去世界找好刷怪笼。

## 4. Mahou Tsukai（ATM9 魔法）

- **Innate Cap 上限设为 5000**。
- 提升 Innate Cap：把 **Caliburn + 末影升级宝珠（最多 26 个）一次性全部丢进 Mahou 湖**。
- **把 Caliburn 变成 Morgan**：用 Caliburn **击杀监守者（Warden）**。

> **一次性警告**：所有末影升级宝珠必须**和 Caliburn 同一次投掷**。如果你先丢 10 个，之后再丢 16 个，湖**不会接受**剩下的 16 个——你只能重做一把 Caliburn。
