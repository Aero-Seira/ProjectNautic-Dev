# 惜命机制 / 死亡惩罚模组调研

> 调研时间：2026-07-20
> 需求来源：整合包"惜命"设计——让死亡有真实代价，并防止反复死亡刷资源/卡位置
> 核实方式：Modrinth API（版本、加载器、下载量、许可、维护状态）+ CurseForge 页面交叉验证；非核实内容均标注 inferred/unknown

---

## 一、需求定义

| 编号 | 机制 | 期望行为 |
|------|------|----------|
| 需求 1 | **重生惩罚** | 死亡重生后生命值/饱食度不重置为满，维持在较低水平（半颗心～几颗心、少量饱食度），让死亡有代价 |
| 需求 2 | **防刷命** | 短时间内连续死亡时改变复活点（随机复活 / 回世界出生点 / 远离原重生点），防止靠反复死亡刷资源、卡位置 |

---

## 二、候选模组核实结果（NeoForge 1.21.1）

### A. 需求 1 方向：重生血量/饱食度控制

#### 1. Brutal Respawn ✅ 支持

- **机制**：死亡重生后不再重置状态——重生时仅 **1 点生命值**，饱食度基于死亡前水平保留（verified，Modrinth 描述）。
- **版本核实**：`1.1`，loaders=`[neoforge]`，game_versions 含 `1.21.1`（verified via Modrinth API）。
- **下载量**：~1,271（很小众）；**更新**：2026-01-30；**许可**：MIT（开源，有源码链接）。
- **侧**：SERVER（server_side=required，客户端 unsupported——纯服务端模组，符合第 4/5 设计支柱）。
- **匹配度**：需求 1 = **★★★★★**（机制几乎逐字命中）；需求 2 = ☆。
- **配置粒度**：unknown（下载量太小，配置项需实测；若不可调"1 点血"可能过硬）。
- **冲突风险**：低。纯服务端、无渲染逻辑；与包内性能/HUD 模组无交集。与 Corail Tombstone 等死亡系统叠加时需确认顺序。
- **风险**：小众模组，长期维护性待观察（单一作者，2026-01 后无更新）。

#### 2. Limited Lives ✅ 支持（机制不同，作对照）

- **机制**：限制生命数，每次重生**最大生命值递减**（verified，Modrinth 描述）。是"每次死亡永久削上限"，而非"重生低血量"。
- **版本核实**：`1.4.0`，neoforge，1.21.1（verified）；有 1 个 required 依赖（Modrinth project `W6ROj0Hl`，其官方库）。
- **下载量**：~50,130；**更新**：2025-05-09；**许可**：LGPL-3.0（开源）。
- **匹配度**：需求 1 = ★★★（惩罚更重且永久化，与"重生低点数"定位不同）；需求 2 = ☆。
- **结论**：机制偏差，作为风格对照记录，不推荐用于本需求。

#### 3. Serilum "Hunger Remains" ❌ 未找到

- 用户需求中提及的 Serilum 系 "Hunger Remains" 在 Modrinth 搜索（`hunger remains`、`hunger death keep serilum` 等关键词）与 Serilum 官方渠道均无结果（verified 不存在于 Modrinth；CurseForge 侧未能直接检索，标 unknown）。
- Serilum 实际相关的死亡类模组为 **Respawn Delay**（见下）。结论：该名称疑为记忆偏差，放弃追踪。

#### 4. Respawn Delay（Serilum）✅ 支持（间接相关）

- **机制**：死亡后进入**旁观模式**，经可配置时长后才重生（verified）。不控制血量/饱食度，但"死亡=强制离场等待"本身是惩罚。
- **版本核实**：`1.21.1-5.0-fabric+forge+neo`，loaders 含 neoforge（verified）；依赖 Collective（包内已有 8.39 ✓）。
- **下载量**：~12,665；**更新**：2026-06-18；**许可**：All Rights Reserved（源码公开可见）。
- **匹配度**：需求 1 = ★★（时间惩罚而非状态惩罚）；需求 2 = ★（拉长死亡周期可减缓连死）。
- **定位**：可作为需求 1 的**补充层**（死亡时间成本），非主力方案。

#### 5. 其他 A 方向候选（核实后淘汰）

| 模组 | NeoForge 1.21.1 | 说明 |
|------|----------------|------|
| Foglia's Less Health on Respawn | ❌ 无对应构建 | 机制命中（可配置重生血量/饱食度），但无 1.21.1 NeoForge 版，下载量 176，不维护 |
| HarderSpawns | ❌ 无对应构建 | 停更于 2023 |
| TinyFish Death Penalty | ❌ 无对应构建（1.21.1 无 neoforge） | 机制接近（死亡倒计时+部分血量重生），但版本不符 |
| Reliable Requiem | ✅ `1.1.1-1.21.1-neoforge` | 基于 Corpse Complex 的**高可配置死亡惩罚/恢复**（掉装备耐久、效果、经验等组合惩罚），下载量 ~3,261，GPL-3.0，2026-07-13 活跃更新。机制偏"惩罚组合包"而非单纯重生低血量；作备选 |
| Death XP Penalty | ❌ 无对应构建 | 仅 XP 维度 |

### B. 需求 2 方向：复活点改变 / 随机复活

#### 1. Better Respawn ✅ 支持，但**方向相反，不推荐**

- **机制**：死亡后**在死亡点附近**可配置半径内复活（verified，Modrinth 描述）。
- **版本核实**：neoforge 1.21.1 构建 `neoforge-1.21.1-2.0.6` 存在（verified）；下载量 ~124,448，2026-06-18 更新，ARR。
- **判断**：该模组让死亡**更便宜**（不用跑尸），与"防刷命"目标完全相反。**不推荐**。

#### 2. Random Respawn 系 ❌ 全军覆没

核实 `random-respawn`（dl 6.6k）、`random-respawns`（dl 2.4k）、`ubi-sumus`、`random-player-respawn`、`randomized-respawn` 等：均无 NeoForge 1.21.1 构建（多数停留在旧版本或仅 Fabric/插件）。其中 `ubi-sumus` 2026-07-07 仍有更新但无 1.21.1 neoforge 构建。
- **结论**：需求 2 方向**无现成可用的 NeoForge 1.21.1 模组**，脚本化自实现成为主路径。

### C. 综合死亡系统（对照评估）

| 模组 | NeoForge 1.21.1 | 机制与判断 |
|------|----------------|-----------|
| **Corail Tombstone** | ✅ `tombstone-neoforge-1.21.1-9.4.9 / 9.5.0`（CurseForge 页面 verified） | 坟墓保留物品 + 死亡技能树/传送回坟。它**降低**死亡代价（物品不丢、可传送回死亡点），与"惜命"定位相反；若未来引入坟墓机制，需配合死亡惩罚模组重新平衡。当前需求下**不推荐** |
| **PlayerRevive** | ✅ 2.1.2（dl 315 万） | 倒地待救援机制，是**减轻**死亡后果的多人协作模组，方向相反；不改变重生状态 |
| **Hardcore Revival** | ✅ 21.1.17（dl 843 万） | 同上，倒地扶起机制 |
| **Majrusz's Progressive Difficulty** | ❌ 无 1.21.1 构建 | 渐进难度（随时间增强怪物），含部分死亡相关机制但版本不符 |

### D. 脚本化自实现（KubeJS 兜底）

包内已有 KubeJS 2101.7.2 + Rhino 2101.2.8 + Additions + DataComponent + LootJS（verified，mods/ 目录）。两个机制均可用 KubeJS server_scripts 实现：

**需求 1（重生惩罚）**：`PlayerEvents.respawned` 中 `player.setHealth(n)`、`player.foodLevel = m` / `player.saturation = s` 即可。约 10 行脚本，配置粒度完全自由（可做阶梯：按死亡次数加深惩罚）。
**需求 2（防刷命）**：`PlayerEvents.died` 记录时间戳到 `player.persistentData`，重生时统计滑动窗口内死亡次数；超阈值后 `player.teleportTo(维度, x, y, z, yaw, pitch)` 传送至世界出生点或随机安全坐标（随机点需简单搜寻安全落点逻辑，约 30-50 行）。

- **优势**：零新增模组（控制模组数）、配置粒度最高、与"魔改即基础设施"支柱完全一致、服务端热重载友好。
- **成本**：需开发与测试；随机复活的安全落点（避免卡方块/落水/高空）是唯一有复杂度的点。
- **风险**：低。KubeJS 已在依赖链核心位置，事件 API 成熟（verified 事件存在性；具体字段名以 2101.7.2 API 为准，实现时核对）。

---

## 三、推荐结论

### 首选方案：Brutal Respawn（需求 1）+ KubeJS 脚本（需求 2）

- **Brutal Respawn 1.1**（NeoForge 1.21.1，MIT，纯服务端）：直接命中需求 1——重生 1 点血、饱食度保留死亡前水平。零依赖、零客户端要求，符合服务端/客户端分离支柱。引入后先实测"1 点血"是否过硬；若过硬且不可配置，降级到方案 B。
- **KubeJS 脚本实现需求 2**：Modrinth 上随机复活类模组全部无 NeoForge 1.21.1 构建，自实现是唯一路径，且滑动窗口连死判定 + 传送逻辑完全可由 server script 承担。

### 备选方案 B：全 KubeJS 自实现

若 Brutal Respawn 实测不可配置或停止维护，需求 1 同样用 `PlayerEvents.respawned` 实现（10 行级脚本）。两机制统一进 `kubejs/server_scripts/death_penalty.js`，与阶段系统（Chapters）未来联动（如某章节后放宽惩罚）也更顺滑。

### 可选补充：Respawn Delay（Serilum）

若希望死亡另有**时间成本**（旁观 N 秒），可叠加；依赖 Collective（已在包内）。非必需，建议在需求 1 实测手感后再定。

### 不推荐项

| 模组 | 理由 |
|------|------|
| Better Respawn | 方向相反：死亡点附近复活，让死亡更便宜 |
| Corail Tombstone / PlayerRevive / Hardcore Revival | 均为减轻死亡后果的系统，与"惜命"定位冲突（若未来要坟墓机制再单独评估并重新平衡） |
| Limited Lives | 永久削生命上限，惩罚模型与需求不符 |
| Random Respawn 全系 | 无 NeoForge 1.21.1 构建 |
| Majrusz's Progressive Difficulty | 无 1.21.1 构建 |

---

## 四、后续行动

1. 下载 Brutal Respawn 1.1 实测重生血量/饱食度数值与可配置性。
2. 编写 `kubejs/server_scripts/` 死亡惩罚脚本（连死检测 + 改复活点），单机与多人各测一轮。
3. 确认后走 maintain-modpack-design 流程入库并建组件页。
