# JEI / Jade 附属模组生态调研

> 调研目标：Minecraft 1.21.1 NeoForge 生态下，JEI 与 Jade 的可用附属/插件模组
> 调研时间：2026-07-15

---

## 一、JEI 附属模组

### ✅ 已确认支持 1.21.1 NeoForge

| 模组 | 版本 | 作者 | 作用 | 设计角色 | 推荐度 | 备注 |
|------|------|------|------|----------|--------|------|
| **Just Enough Resources (JER)** | 1.6.0.17 | - | 查询矿石生成层、生物掉落、村民交易、植物生长条件 | `utility` | ⭐⭐⭐⭐⭐ | **几乎必备**。Client-side only，不增加服务端负担 |
| **Just Enough Effect Descriptions (JEED)** | 2.3.0+ | MehVahdJukaar | 在 JEI 中显示药水/状态效果的详细描述 | `utility` | ⭐⭐⭐⭐ | 同时支持 JEI/REI/EMI，轻量无依赖 |
| **Collapsible Groups (JEI)** | 最新 | - | 将物品列表按组折叠，减少滚动量 | `utility` | ⭐⭐⭐ | 模组数量 >30 后体验提升明显 |
| **FTB JEI Extras** | 最新 | FTB Team | 桥接 FTB 生态（FTB Quests/Teams 等）与 JEI | `integration` | ⭐⭐⭐ | 若后续加入 FTB 系列则必需 |
| **Just Enough Archaeology** | 1.2.0 | LobsterJonn | 考古相关 JEI 扩展 | `utility` | ⭐⭐ | 若加入考古玩法内容模组时考虑 |

### ❌ 已确认不支持/未更新到 1.21.1

| 模组 | 最后支持版本 | 说明 |
|------|-------------|------|
| **JEI Integration** | 1.18.2 Forge | 集成和工具提示，长期未更新 |
| **JEITweaker** | 1.18.2 | CraftTweaker 桥接，当前项目使用 KubeJS 而非 CraftTweaker |
| **JEI Bees** | 1.12.2 | 林业蜜蜂扩展，极老，与当前生态无关 |

### 🔮 待观察

| 模组 | 状态 | 说明 |
|------|------|------|
| **KubeJS JEI** | KubeJS 内置 | KubeJS 已通过可选依赖 `jei` 暴露配方事件，无需额外模组 |
| **EMI** | 可用但替代方案 | EMI 是 JEI 的现代替代品，支持 1.21.1 NeoForge。若未来 JEI 维护停滞可考虑迁移，但当前 JEI 活跃维护中 |

---

## 二、Jade 附属模组

### ✅ 已确认支持 1.21.1 NeoForge

| 模组 | 版本 | 作者 | 作用 | 设计角色 | 推荐度 | 备注 |
|------|------|------|------|----------|--------|------|
| **Jade Addons** | 6.1.0 | Snownee | Jade 官方插件，为 Create、Tinkers Construct、Lootr、EnderIO、Supplementaries、Artifacts 等模组提供信息提示 | `integration` | ⭐⭐⭐⭐ | **当前建议暂不添加**，因上述目标模组尚未加入。每加入一个目标模组时同步添加对应 Jade Addon 支持 |
| **Villager Inventory Plugin** | 最新 | - | 显示村民/悦灵/猪灵的背包内容 | `utility` | ⭐⭐⭐ | 小巧实用，与 Jade 无缝集成 |

### ⚠️ 竞品/替代方案

| 模组 | 状态 | 说明 |
|------|------|------|
| **WTHIT** | 活跃维护 | "What The Hell Is That"，Jade 的竞品。Hwyla 的现代继任者。与 Jade 功能重叠，**不建议同时安装** |
| **HWYLA** | 已弃用 | 原始 WAILA 分支，已停止维护。Jade 和 WTHIT 均兼容其插件 API |

---

## 三、推荐添加批次

### 第一批：JEI 核心附属（建议现在加）

1. **Just Enough Resources (JER)** — 世界生成信息查询，对玩家探索和资源规划至关重要
2. **Just Enough Effect Descriptions (JEED)** — 药水效果说明，Vanilla+ 起步即可感受到价值

> 这两个模组轻量、无额外依赖、与现有 JEI 无缝集成。

### 第二批：Jade 功能扩展（随内容模组同步添加）

1. **Jade Addons** — 当前目标模组（Create、Tinkers Construct、Lootr 等）均未加入，暂不添加。每加入一个受支持的模组时，同步添加 Jade Addons。
2. **Villager Inventory Plugin** — 可随时添加，与当前模组无冲突。

### 第三批：待观察

1. **Collapsible Groups (JEI)** — 模组数量达到 30+ 时评估是否添加，当前 10 个模组折叠意义不大。

---

## 四、与 ProjectNautic 设计支柱的关联

| 设计支柱 | 关联模组 |
|----------|----------|
| **性能优先** | JER 为 Client-side only，不增加服务端负担；JEED 同样纯客户端 |
| **Vanilla+ 起步** | JER 帮助玩家理解原版世界生成规律；JEED 解释原版药水效果 |
| **长线运营** | JER 的村民交易查询功能对未来经济系统有帮助；JEED 随着模组增加药水效果时价值上升 |
| **服务端客户端同源** | 两个附属均为客户端模组，服务端无需同步 |

---

## 五、版本兼容性汇总

| 模组 | Minecraft | NeoForge | 范围 | 依赖 |
|------|-----------|----------|------|------|
| JER | 1.21.1 | ✅ | CLIENT | JEI |
| JEED | 1.21.1 | ✅ | CLIENT | JEI |
| Jade Addons | 1.21.1 | ✅ | BOTH | Jade |
| Villager Inventory Plugin | 1.21.1 | ✅ | BOTH | Jade |

---

*调研完成，可直接进入添加流程。*
