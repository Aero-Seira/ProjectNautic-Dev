# Brutal Respawn

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/brutal_respawn-1.1-(1.21-1.21.8).jar`
- 标识与版本：mod_id = `brutal_respawn`；版本 `1.1`；作者 vlad0-0
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Brutal Respawn 让死亡重生更具挑战性：重生时低血量、低饱食并附加短暂惩罚效果（verified，mods.toml 描述："A simple mod that makes the resurrection mechanics a little more challenging."）。**这是调研结论的落地**：`docs/design/research/death-penalty-mods.md`（2026-07-20）针对"重生低血量/低饱食度惩罚"需求核实了 12+ 候选模组后推荐 Brutal Respawn 1.1（需求 1）；需求 2（连续死亡改复活点）按计划由 KubeJS 脚本自实现，尚未落地。

## 玩家可见行为

- 死亡重生后：生命值降至 **1**（`healthOnRespawn=1`，半颗心），饥饿值最低 **0**、饱和度 **0**（`minFoodLevelOnRespawn=0`、`minSaturationOnRespawn=0.0`）。
- 死亡额外扣除 **3** 点饥饿（`deathHungerPenalty=3`）。
- 重生后附加 10 秒"brutally respawned"惩罚效果（`brutallyRespawnedEffectDuration=10`）。
- 摩擦/失败：重生点若不安全（怪堆、高空），1 血重生极易形成连死循环——这正是需求 2（连死改复活点）要缓解的场景。

## 集成关系

- **依赖**：NeoForge `[21,21.9)`、Minecraft `[1.21,1.21.9)`、`yet_another_config_lib_v3 [3.6.0,)`（required）——YACL 已在包内（3.8.2）。
- **协作**：惩罚效果图标由 Stylish Effects / Inventory HUD+ 显示；与 BetterDays（睡眠机制）同属机制层但无交互。
- **预留**：KubeJS 连死改复活点脚本将与本模组共同构成完整死亡惩罚体系。

## 配置意图

`config/brutal_respawn.json` 的数值即调研推荐的最严配置：1 血 + 0 饥饿 + 0 饱和重生，惩罚效果 10 秒。设计意图是把死亡从"免费回城"改为有实质代价的事件，强化生存张力；数值偏硬核，上线后需依玩家反馈回调。

## 兼容性与性能

- BOTH 端（逻辑在服务端）；无存档影响；开销可忽略。
- 仅修改重生数值，可随时移除或调参。

## 验证

- [ ] 启动或加载测试
- [ ] 死亡重生数值实测（血量/饥饿/惩罚效果）
- [ ] 危险重生点连死场景评估（需求 2 脚本落地前的风险敞口）

## 风险与开放问题

1. 数值硬核（1 血 0 食），连死循环风险在 KubeJS 连死改复活点脚本落地前存在。
2. 多人服务器上新玩家死亡体验需观察。

## 历史

- 2026-07-21: 作为第六批机制类模组添加，落地 `research/death-penalty-mods.md` 推荐结论（需求 1）
