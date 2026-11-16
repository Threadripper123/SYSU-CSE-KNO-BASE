# 08 ATM 电力与资源模组（含自定义数值）

> 主题库：Minecraft 整合包：BMC 与 ATM 系列　｜　源文件：（纯文本条目）　｜　6,603 字符　｜　40 个片段　｜　更新于 2026-11-13
# ATM 电力与资源模组（含 ATM 自定义数值）

> ATM 对发电与机器数值做了大量修改（官方称 Power Gen Changes）。**下面的数字是 ATM9 的，ATM10 未必一致**，跨版本务必以 JEI/游戏内为准。

## 1. Mekanism

### 发电改动

| 发电机 | 改动 | 备注 |
|---|---|---|
| Bio Generator | 280 → **500 RF/t** | 加强，让它不只是 GBG 的垫脚石 |
| GBG（乙烯） | 78K → **8.5k RF/t** | **超级大砍**，但鼓励做正经的乙烯产线；燃烧速率更高意味着需要更多西瓜 |

### AE2 + Mekanism 冶金灌注机全自动（2 通道搞定）

**材料**：1× 冶金灌注机、1× 富集仓、1× 石英纤维、8× AE2 线缆、2× ME 样板供应器（方块形态）、1× ME 接口（面板形态）、2× ME 存储总线、8× 空白样板。

**搭法**：
1. 放灌注机，两侧各贴一个 ME 存储总线。
2. 灌注机后面放样板供应器，上面放 ME 接口，侧面贴石英纤维。
3. 用 7 根同色线缆连起所有 AE2 设备，再用另一色线缆接到主网。
4. 灌注机旁放富集仓，后面放第二个样板供应器，两者用线缆相连。
5. 同色线缆之间要放 **Cable Anchor**（或改用不同颜色）。

**输入输出设置**：灌注机左侧输入（红槽）、右侧输入「额外物品」（黄槽）；富集仓从背面输入/输出（紫槽）。

**存储总线过滤**：
- 左侧：Iron、Infused Alloy、Enriched Alloy、Obsidian Dust
- 右侧：Enriched Redstone、Diamond、Obsidian

**样板（必须是 Processing / 熔炉图标，不是 Crafting）**：

灌注机：
- 8 Iron + 1 Enriched Redstone = 8 Infused Alloy
- 4 Infused Alloy + 1 Enriched Diamond = 4 Enriched Alloy
- 2 Enriched Alloy + 1 Enriched Obsidian = 2 Atomic Alloy
- 1 Obsidian = 4 Obsidian Dust

富集仓：
- 1 Redstone = 1 Enriched Redstone
- 1 Diamond = 1 Enriched Diamond
- 1 Refined Obsidian Dust = 1 Enriched Obsidian
- 8 Obsidian Dust + 1 Enriched Diamond = 8 Refined Obsidian Dust

这两组配方的比例设计使灌注机**几乎不可能被残余材料堵住**。两台机器都要供电并开启 **Auto-Split**。

### 裂变反应堆温度计算

反应堆在 **>1200K** 开始受损。给定燃烧速率，用下面公式估算温度：

- 水冷：`burn_rate × 2 × [1,000,000 × (heat_capacity − 1)] + 373.15`（单位 mB）
- 钠冷：`burn_rate × [1,000,000 × (heat_capacity − 1)] + 373.15`（单位 mB）

**热容（heat_capacity）**：默认等于外壳方块数 × 1000（外壳包括反应堆玻璃、外壳、阀门等）。

```
heat_capacity = (2 × W²) + [(H − 2) × (W² − (W − 2)²)] × 1000
```

W/H 为反应堆的宽与高。

## 2. Powah

### 机制改动

- **Soul Lava 可用于 Thermo Generator**：不提高 RF/t，但**每 mB 产生的 RF 提高 9 倍**。
- 早期发电机全面加强，让 Powah 起步更舒服。

### 数值表（左：原值 → 右：ATM 值）

**Furnator / Magmator**（两者相同）

| 等级 | RF/t |
|---|---|
| Starter | 20 → 80 |
| Basic | 80 → 160 |
| Hardened | 200 → 400 |
| Blazing | 800 → 1000 |

**Thermo Generator**

| 等级 | RF/t |
|---|---|
| Starter | 20 → 40 |
| Basic | 60 → 80 |
| Hardened | 125 → 150 |
| Blazing | 250 → 300 |
| Niotic | 500 → 600 |
| Spirited | 1250 → 1500 |
| Nitro | 2750 → 3500 |

配 Soul Lava 后 Nitro 档从约 25k RF/t 提到约 **31.5k RF/t**。

**Solar Generator**

| 等级 | RF/t |
|---|---|
| Starter | 20 → 40 |
| Basic | 60 → 120 |
| Hardened | 180 → 400 |
| Blazing | 540 → 1500 |
| Niotic | 1620 → 5000 |
| Spirited | 5000 → 16000 |
| Nitro | 20000 → 50000 |

太阳能虽然加成夸张，但**造价高于几乎所有发电机（包括反应堆）**，且每级需要 3 个下级，属于后期选择。

## 3. Extreme Reactors

### 尺寸上限

| 结构 | 最大高度 Y | 最大 X/Z |
|---|---|---|
| Basic Reactor | 5 | 5 |
| Hardened Reactor | 48 | 32 |
| Turbine | 32 | 32 |
| Fluidizer | 16 | 16 |

### 效率经验（社区总结）

- **横向加宽比加高更省燃料**。
- 燃料棒用**棋盘格**排列，最外圈燃料棒到内壁留 **3 格**。
- **导热块（Heat Manifold）**贴在反应堆内壁，同样棋盘格，位于任意燃料棒的正北/南/东/西方向。
- **被动冷却**直接发电；**主动冷却（水）**产蒸汽给涡轮。要先用被动模式跑一段时间产 **Cyanite** → **Blutonium** 才能造涡轮。
- 一个主动冷却反应堆的蒸汽可以带**多台涡轮**。
- Mekanism 的**液态钠**可用于主动冷却，产过热钠，再进换热器产更多蒸汽 → 带更多涡轮。但在 ATM 里换热器方案性价比不高（其它发电路线更强），属于「好玩但过度工程」。

### 导热块（Moderator）排行（14,694 FE/t 为 100%）

| 导热块 | FE/t | 相对 | μB/t |
|---|---|---|---|
| **Redfrigium** | 14,694 | 100.00% | 181 |
| Netherite | 14,629 | 99.56% | 183 |
| Terrasteel | 14,625 | 99.53% | 185 |
| Diamond | 14,573 | 99.18% | 184 |
| Elementium | 14,527 | 98.86% | 185 |
| Cryomisi | 14,502 | 98.69% | 186 |
| Emerald | 14,475 | 98.51% | 184 |
| Electrum | 14,466 | 98.45% | 187 |
| Tangerium | 14,422 | 98.15% | 184 |
| Graphite | 14,342 | 97.60% | 182 |
| Lead | 14,277 | 97.16% | 187 |
| Manasteel | 13,912 | 94.68% | 191 |
| Iron | 13,862 | 94.34% | 190 |
| Dry Ice | 13,525 | 92.06% | 198 |
| Air（空） | 13,406 | 91.23% | 198 |

**结论**：从 Air 到 Redfrigium，收益只差约 9.6%。**别为了导热块把进度卡死**，用铁/石墨就能拿到 94–97%。

### 整体发电与自动化

- ATM 里整体发电 **1× → 3×**；5×3 的反应堆约产 **15k RF/t**。
- ComputerCraft 集成：需要开启 HTTP API。改 `<世界名>/serverconfig/computercraft-server.toml`，把 `[http]` 下的 `enabled = false` 改成 `true`。**改这个文件必须停服**，否则不生效。

## 4. Generator Galore

| 发电机 | 改动（RF/t） |
|---|---|
| Copper | 8 → 20 |
| Iron | 16 → 40 |
| Gold | 32 → 80 |
| Diamond | 64 → 160 |
| Obsidian | 128 → 200 |
| Emerald | 256 → 300 |
| Netherite | 256 → 500 |
| Enchantment | 40 → **500** |
| Magmatic | 40 → 150 |
| Ender | 40/80 → 300/500（末影珍珠或末影之眼） |
| Culinary | 8 → 10（ATM 胡萝卜 320 → 400 RF/t 每根） |
| Halitosis（龙息） | 40 → **1k**；总产出 480k → 200k（龙息只能用 HNN 自动化） |
| NetherStar | 4k → **10k**（造星需要大量下界之星，所以值得） |

## 5. GregTechCEu Modern（ATM9）

这是 ATM9 后期最硬的技术线，概念必须分清：

| 概念 | 定义 | 触发条件 |
|---|---|---|
| **Recipe Tier Skip**（配方越级） | 多方块能执行**高一档**的配方（如 LV 能源仓跑铝） | **必须有两个同档能源仓**（安培数无关） |
| **Overclock**（超频） | 多方块以高于能源仓的电压档运行（需足够安培） | 4A 总输入 = 1 次超频；16A = 2 次 |
| **Hatch Sharing**（能源仓共享） | 用一个 2A 仓给两台多方块各 1A | 既不要越级也不要超频时用 |
| **Parallel**（并行） | 同一时间做 x 份配方，耗 x 份电压 | 并行**在超频之前**结算，比不完美超频更高效 |
| **Subtick Overclock** | 超频超过 1 tick（0.05s）下限后，把超频转成**免费并行** | 4× 电压 / 0.5× 时长 |
| **Perfect Subtick Overclock** | 同上，但用完美超频规则 | 4× 电压 / 0.25× 时长，并行 100% 效率 |

**共享仓的坑**：
- 不要在两台多方块间共享「一对 2A 仓」或「单个 4A/16A 仓」，否则第二台没有可用安培。
- EV 之后想同时要越级 + 超频：在两台之间共享**一对 4A/16A 仓**，既满足「两个仓」的越级条件，又各给 4A/16A。

**GT 终端自动建造**：放下多方块控制器，手持 GT 终端 **潜行 + 右键**控制器，只要背包材料够就会自动搭。但它对结构的假设很多，仓/总线位置常需手动调整。

### ATM9 新增多方块

| 多方块 | 作用 |
|---|---|
| Greenhouse | 种作物，只要加水 |
| Apiary MK1 | 从蜜蜂产蜂巢块，会伤害蜜蜂 |
| Apiary MK2 | 从「完美蜜蜂」产蜂巢块，不伤害 |
| **Star Forge** | 合成 **ATM Star** 与 **Gregstar** |
| Micro Universe Orb (MUO) | 产电、产矿，或产创造物品 |
| Advanced Large Chemical Reactor (ALCR) | 带并行的大化学反应釜 |
| Mega Fusion Reactor | 带并行的聚变反应堆 |
| Void Miner | 产矿 |

### 下界之星减负

**Inert Nether Stars**：**1 个凋灵骷髅头 → 16 个下界之星**。Mekanism + GT 结合还能更高。这是 ATM9 终局能玩下去的关键设计。

### 血魔法陨石（Mark of the Falling Tower）

| 召唤材料 | 陨石内容 |
|---|---|
| Intricate Hellforged Parts + 5,000,000 LP | Demonite Ore、Reinforced Speed Runes |
| Unobtainium Ingot + 2,500,000 LP | Allthemodium、Vibranium、Unobtainium 矿 |
| Naquadah Ingot + 2,500,000 LP | Naquadah、Plutonium、Tungstate 矿 |
| Titanium Ingot + 1,250,000 LP | Sphalerite、Sulfur、Tetrahedrite 矿 |
| Arsenic Dust + 625,000 LP | Tantalite、Vanadium Magnetite、Cobaltite 矿 |
| Compressed Diamond Block + 1,000,000 LP | 钻石块核心 + 钻石/绿宝石/煤 |
| Palladium Ingot + 2,500,000 LP | Chromite、Bauxite、Palladium 矿 |
| Compressed Nether Star Block + 7,500,000 LP | 下界之星块、灵魂沙、凋灵骷髅头 |

**大陨石变体**：Samarium Iron Arsenic Oxide Block（1.25M LP）、Indium Tin Barium Titanium Cuprate Block（2.5M LP）、Uranium Rhodium Dinaquadide Block（5M LP）、Rhodium Plated Palladium Block（5M LP）、Unobtainium Vibranium Alloy Block（5M LP）、Wither's Compass（9.75M LP）。

## 6. Mob Grinding Utils

唯一改动：**Mob Masher 的 Looting 升级配方**。原来是红石粉 + 4 蓝染料 + 4 金粒，现在**把红石粉换成一本带抢夺（任意等级）的附魔书**。其余不变。

**为什么重要**：抢夺是刷怪塔产出的核心倍率，这个改动意味着你需要先有附魔台/附魔书来源才能上抢夺。
