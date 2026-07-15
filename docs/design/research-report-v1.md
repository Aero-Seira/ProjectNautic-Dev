# ProjectNautic 1.21.1 NeoForge 长线运营整合包调研报告

> 调研日期：2026-07-15
> 目标版本：Minecraft 1.21.1 + NeoForge 21.1.x + Java 21
> 项目定位：服务器长线运营，类原版起步，分阶段内容投放

---

## 一、平台运行基线（已确认）

| 项目 | 状态 | 备注 |
|------|------|------|
| Minecraft 1.21.1 | ✅ 稳定 | 当前主流 modpack 长期停留版本 |
| NeoForge 21.1.235 | ✅ 已验证 | 系列已维护 235+ builds，极为成熟 |
| Java 21 | ✅ 必须 | NeoForge 1.21.1 硬性要求 |
| 服务端可行性 | ✅ 完全支持 | NeoForge 提供专用服务端构建 |

**关键判断**：1.21.1 是当前 NeoForge 生态最成熟的版本，绝大多数内容模组已在此版本稳定发布。对于长线运营服务器而言，选择此版本作为基线风险极低，且社区支持周期较长。

---

## 二、魔改框架生态（核心调研）

### 2.1 KubeJS 7.0 — 首选魔改引擎

KubeJS 在 1.21.1 上经历了重大重构（7.0 版本），是 NeoForge 1.21+ 的**首选且事实上唯一重点维护**的脚本魔改框架。

| 特性 | 1.21.1 NeoForge 支持状态 |
|------|------------------------|
| 基础配方增删改 | ✅ 完整支持，`ServerEvents.recipes` |
| 自定义物品注册 | ✅ 完整支持，`StartupEvents.registry('item')` |
| 自定义流体注册 | ✅ 改进后的薄/厚流体类型 |
| 自定义配方类型（JSON Schema） | ✅ 新增，无需依赖 mod jar |
| 战利品修改 | ⚠️ 已迁移至 **LootJS** addon |
| 世界生成修改 | ⚠️ 当前禁用，计划以独立 addon 形式回归 |
| 服务端专属脚本目录 | ✅ **新增** `local` 文件夹，服务端热更新脚本不覆盖 |
| 原生事件监听 | ✅ `NativeEvents`，支持 client/server/startup 三端 |
| 自动保存重载 | ✅ 可配置自动重载 |
| 配方 staging | ✅ `.stage(string)` 原生内置，无需 Recipe Stages |
| 配置格式 | ✅ 全部改为 `.json` |
| 不再依赖 Architectury API | ✅ 精简依赖 |

**策略建议**：第一版即引入 KubeJS 作为核心魔改框架，所有配方调整、事件脚本、自定义内容均通过 KubeJS 实现。这保证了后续大版本更新时魔改逻辑的可迁移性和一致性。

### 2.2 KubeJS 关键 Addon 生态（1.21.1 NeoForge 可用）

| Addon | 功能 | 状态 |
|-------|------|------|
| **LootJS** | 战利品表修改（替代 KubeJS 原生 loot 事件） | ✅ 1.21.1 NeoForge 活跃维护 |
| **KubeJS Additions** | 额外事件监听（Jade/ Fabric Arch 事件）、JEI 自定义分类、区块加载控制等 | ✅ 1.21.1 NeoForge 6.0.0+ |
| **KubeJS Data Component** | 1.21 物品组件系统（Item Components）操作 | ✅ 1.21.1 NeoForge 专用 addon |
| **KubeJS Create Automation** | Create 机器与 KubeJS 自动化桥接 | ✅ 1.21.1 NeoForge |
| **Ponder for KubeJS** | 自定义 Create Ponder 场景 | ✅ 1.21.1 NeoForge 2.4.0+ |
| **KubeJS Iron's Spells** | 自定义法术、法术书、魔法学派 | ✅ 1.21.1 NeoForge 4.0.3+ |
| **ProbeJS** | VSCode 自动补全与类型提示 | ✅ 持续支持 |

### 2.3 CraftTweaker — 备选/补充

CraftTweaker 同样支持 1.21.1 NeoForge（21.0.24 等版本），但社区趋势明显向 KubeJS 迁移。建议：
- 以 KubeJS 为主力魔改框架
- CraftTweaker 仅作为个别 mod 缺乏 KubeJS 支持时的补充
- 避免两者同时大规模使用，减少维护复杂度

---

## 三、GameStage 替代方案 — 核心发现

### 3.1 Chapters — 1.21.1 NeoForge 的 GameStages 继任者

**这是本调研最重要的发现。** Chapters 是专为 NeoForge 1.21.1 设计的阶段进度系统，直接继承 GameStages + ItemStages 的设计哲学，并做了深度整合。

| 功能 | 支持状态 |
|------|----------|
| 物品阶段锁定（按 ID / Tag / 整个 Mod） | ✅ |
| 流体阶段锁定 | ✅ |
| Mekanism 化学品锁定 | ✅ |
| 配方阶段锁定（按配方 ID） | ✅ |
| 数据包驱动阶段定义 | ✅ `data/<namespace>/chapters/stages/<stage_id>.json` |
| KubeJS 脚本驱动 | ✅ `ChaptersEvents.defineStage()` / `PlayerStages` API |
| JEI 联动（隐藏锁定内容） | ✅ |
| FTB Teams 团队共享进度 | ✅ 全队共享阶段 |
| FTB Quests 任务奖励/要求阶段 | ✅ 原生 Stage Reward / Stage Task / Stage Required |
| 自动丢弃锁定物品 | ✅ 玩家在线时定期检测，无法通过潜影盒规避 |

**使用示例（KubeJS）**：
```js
ServerEvents.loaded(event => {
  ChaptersEvents.defineStage('mypack:tier1', [
    'minecraft:netherite_ingot',
    '#minecraft:swords',
    'recipe:minecraft:diamond_pickaxe',
    'fluid:minecraft:lava',
    'chemical:mekanism:hydrogen',
    '@create'
  ])
})

// 授予/检查阶段
PlayerStages.of(player).add('mypack:tier1')
PlayerStages.of(player).has('mypack:tier1')
```

**策略建议**：第一版即引入 Chapters 作为核心进度控制框架。即使第一版接近原版，也应预留阶段体系，为未来模组内容的分阶段解锁打好基础。

---

## 四、长线运营工具链

### 4.1 FTB 生态（1.21.1 NeoForge 全可用）

| Mod | 功能 | 运营价值 |
|-----|------|----------|
| **FTB Library** | 基础 GUI/组件库 | 必需前置 |
| **FTB Teams** | 团队系统 | 玩家组队、领地共享 |
| **FTB Quests** | 任务/进度系统 | **核心**：引导玩家、版本更新时可新增任务章节 |
| **FTB Chunks** | 区块声明/保护 | 防止恶意破坏 |
| **FTB XMod Compat** | 跨 mod 兼容层 | 确保 FTB 与 KubeJS/其他 mod 兼容 |
| **FTB Quests Optimizer** | 任务系统性能优化 | 服务器端降低 Quest 性能开销 |

**FTB Quests 与 Chapters 的联动**：FTB Quests 可以设置 "Stage Reward" 作为任务奖励自动解锁 Chapters 阶段，也可以设置 "Stage Task" 要求玩家达到某个阶段才能进行任务。这是长线运营中"内容解锁"的理想工作流。

### 4.2 版本管理与更新通知

| Mod | 功能 | 状态 |
|-----|------|------|
| **VersionerReborn** | 整合包版本检查工具，从远程 JSON 获取版本信息，主界面显示当前/最新版本，支持 KubeJS 调用 | ✅ 国创，1.21.1 NeoForge 专用 |

**策略建议**：部署 VersionerReborn，在启动器层面告知玩家有新版本可用。配合服务器端的更新公告系统，形成完整版本更新闭环。

### 4.3 服务端性能优化模组

| 模组 | 作用 | 部署端 |
|------|------|--------|
| **ModernFix** | 全能优化：启动加速、内存降低、bug 修复 | 客户端+服务端 |
| **FerriteCore** | 内存占用优化（模型/方块状态） | 客户端+服务端 |
| **Lithium** / **Moonrise** | 游戏逻辑优化（AI/区块/物理） | 服务端为主 |
| **Starlight** | 光照引擎重写 | 客户端+服务端（不共存） |
| **Clumps** | 经验球合并 | 服务端 |
| **Chunky** | 区块预生成 | 服务端 |
| **Entity Culling** | 实体渲染剔除 | 客户端 |
| **Embeddium** | 渲染优化（NeoForge 版 Sodium） | 客户端 |
| **ImmediatelyFast** | 渲染批处理优化 | 客户端 |
| **spark** | 性能分析工具 | 服务端 |

---

## 五、内容模组生态（1.21.1 NeoForge 可用）

### 5.1 第一版（类原版 Vanilla+）推荐模组

第一版目标：贴近原版体验，但提供足够的可扩展性和魔改空间。模组数量控制在 30-50 个左右。

| 分类 | 推荐模组 | 魔改接口价值 |
|------|----------|-------------|
| **农业/食物** | Farmer's Delight (+ 附属) | 食谱可 KubeJS 修改，经营玩法基础 |
| **QoL** | Waystones, JEI/REI, Jade, Xaero's Minimap/World Map, Supplementaries | 服务端基础设施 |
| **存储** | Sophisticated Backpacks / Traveler's Backpack | 玩家成长空间 |
| **世界生成** | Terralith + Terrablender, YUNG's 系列结构 | 增强探索但不改变核心机制 |
| **结构** | When Dungeons Arise, Towns and Towers | 冒险内容 |
| **生物** | Alex's Mobs (需确认), Mowzie's Mobs | 生态丰富度 |
| **装饰/建筑** | Chipped, Handcrafted, FramedBlocks | 玩家创造空间 |
| **工具** | Apotheosis（神化） | 装备成长系统，KubeJS 支持附魔/神器修改 |
| **保护** | FTB Chunks | 领地系统 |
| **任务** | FTB Quests | 引导系统 |
| **阶段** | Chapters | 进度控制 |
| **魔改核心** | KubeJS + LootJS + KubeJS Additions | 全部魔改能力 |

### 5.2 后续大版本可投放的内容模组

以下模组已确认支持 1.21.1 NeoForge，可按主题分版本加入：

#### 科技线（Tech）
| 模组 | 定位 | 版本投放建议 |
|------|------|-------------|
| **Create** | 机械动力/自动化 | 科技线首版 |
| **Mekanism** | 高科技/五倍矿 | 科技线深化版 |
| **Applied Energistics 2** | 存储/自动化网络 | 与 Mekanism 同批或稍后 |
| **Ender IO** | 管道/机械 | 科技补充 |
| **Immersive Engineering** | 复古工业美学 | 可选主题包 |
| **Modern Industrialization** | 格雷式科技线 | 硬核科技版 |
| **Oritech** | 新科技模组 | 视社区热度 |

#### 魔法线（Magic）
| 模组 | 定位 | 版本投放建议 |
|------|------|-------------|
| **Ars Nouveau** | 现代魔法/法术编写 | 魔法线首版 |
| **Iron's Spells 'n Spellbooks** | RPG 式魔法 | 魔法线首版/次版 |
| **Occultism** | 召唤/仪式魔法 | 魔法深化版 |
| **Malum** | 灵魂魔法 | 可选 |
| **Mahou Tsukai** | 强力魔法系统 | 魔法线高潮版 |

#### 冒险/战斗线（Adventure）
| 模组 | 定位 | 版本投放建议 |
|------|------|-------------|
| **Ice and Fire** | 龙/神话生物 | 冒险首版 |
| **L_Ender's Cataclysm** | 强力 Boss | 冒险深化 |
| **Bosses of Mass Destruction** | 地下城 Boss | 冒险深化 |
| **The Aether** | 天境维度 | 维度扩展版 |
| **Twilight Forest** | 暮色森林 | 维度扩展版 |
| **RPG Series** | 职业系统（战士/法师/盗贼等） | 系统性 RPG 改版 |

#### 其他系统模组
| 模组 | 定位 |
|------|------|
| **MineColonies** | 殖民地建设 |
| **Cobblemon** | 宝可梦系统 |
| **Draconic Evolution** | 终极科技/装备 |
| **Mystical Agriculture** | 资源作物（需确认 1.21.1 状态） |
| **Thermal Expansion** | 经典科技（需确认 1.21.1 状态） |
| **Simple Storage Network / Tom's Simple Storage** | 前期存储 |
| **Refined Storage** | AE2 替代存储 |

---

## 六、版本更新机制设计建议

### 6.1 更新策略框架

基于 Chapters + FTB Quests 的分阶段解锁机制，设计"版本即章节"的长线运营模式：

```
V1.0 (开荒期)      → 原版增强 + 基础 QoL + 阶段系统框架
V1.1 (生活期)      → 新增农业/食物/建筑模组 + 配套任务章节
V2.0 (觉醒期)      → 开放第一批科技/魔法模组 + 新阶段解锁
V2.1 (深化期)      → 科技/魔法线深化 + 新增 Boss/冒险内容
V3.0 (次元期)      → 开放新维度模组 + 跨维度任务线
V3.x (终局期)      → 终极装备/挑战/创造级内容
```

### 6.2 技术实现方案

| 机制 | 实现方式 |
|------|----------|
| 内容锁定 | Chapters 阶段控制 + KubeJS 配方/事件脚本 |
| 任务引导 | FTB Quests 按版本添加新章节 |
| 新玩家体验 | 新玩家默认解锁所有已发布阶段；老玩家通过新任务获取新阶段 |
| 存档兼容 | 新模组加入时不会生成旧区块中；新维度/结构仅在新区块生成 |
| 版本通知 | VersionerReborn 客户端提示 + 服务端公告 |
| 魔改更新 | KubeJS 脚本服务端 `local` 目录热更新，无需重启 |

### 6.3 关键风险与规避

| 风险 | 规避策略 |
|------|----------|
| 新增模组导致 ID 冲突 | 使用 KubeJS 注册新物品时指定固定 ID；添加模组前用 NBTExplorer/数据包检查 |
| 世界生成不兼容 | 新维度模组不影响主世界；新结构模组仅影响新区块 |
| 阶段系统回溯 | Chapters 支持移除阶段（`/chapters remove`），但设计上应避免回溯 |
| 玩家进度差距 | FTB Teams 确保团队共享；设计"追赶机制"任务 |
| 服务端性能衰减 | 每个版本更新前用 spark 基线测试；保持优化模组更新 |

---

## 七、第一版（V1.0）推荐模组清单

以下是贴近原版、低侵入性、高可扩展性的第一版建议：

### 核心魔改层（必须）
- KubeJS (NeoForge 1.21.1)
- Rhino (KubeJS 前置)
- LootJS (KubeJS 战利品 addon)
- KubeJS Additions
- KubeJS Data Component
- Chapters (阶段系统)
- FTB Library + FTB Teams + FTB Quests + FTB XMod Compat + FTB Quests Optimizer
- ProbeJS (开发工具，可选客户端)

### 优化层（必须）
- ModernFix
- FerriteCore
- Clumps
- spark (服务端)
- 客户端可选：Embeddium, Entity Culling, ImmediatelyFast

### 原版增强层
- Farmer's Delight
- Supplementaries
- Amendments
- Waystones
- Nature's Compass

### 世界与探索层
- Terralith
- YUNG's 结构系列（选 2-3 个）
- Towns and Towers
- When Dungeons Arise

### 玩家成长层
- Apotheosis
- Curios API / Accessories
- Sophisticated Backpacks 或 Traveler's Backpack

### 基础设施层
- JEI / REI
- Jade
- Controlling
- Mouse Tweaks

### 版本管理
- VersionerReborn

---

## 八、开放问题与待确认项

1. **Alex's Mobs 1.21.1 NeoForge 状态**：需确认是否已移植或有无替代
2. **Thermal Expansion / Mystical Agriculture 1.21.1 状态**：调研中未看到明确确认，需进一步核实
3. **服务端托管方案**：是否需要集成 Docker/面板部署文档？
4. **跨版本存档迁移策略**：是否允许玩家将 V1 存档带入 V2？（建议：允许，但新内容需新区块）
5. **经济/交易系统**：是否需要引入服务器专属经济模组（如 Quest Shop）？

---

## 九、总结与下一步建议

### 核心结论
1. **1.21.1 NeoForge 是长线运营的绝佳基线**：生态成熟、维护活跃、模组丰富
2. **KubeJS 7.0 + Chapters 是现代魔改的黄金组合**：完全替代了旧时代的 CraftTweaker + GameStages 方案
3. **FTB 生态完整支持**：Quests/Teams/Chunks 全部可用，构成运营基础设施
4. **分阶段投放完全可行**：通过 Chapters 的 item/fluid/recipe/mod 级别锁定，实现"版本更新=内容解锁"

### 建议的下一步行动
1. 确认本报告中的第一版模组清单，进行实际兼容性测试
2. 搭建第一个 KubeJS + Chapters 的阶段解锁原型（如：锁定下界合金装备直到完成某个任务）
3. 设计 FTB Quests 的第一版任务线框架
4. 确定服务端部署方案（纯净 NeoForge 服务端 / 容器化）
5. 制定版本更新节奏（建议：小版本 2-4 周，大版本 2-3 个月）

---

*报告结束。如需针对某个具体模组或系统深入调研，可继续展开。*
