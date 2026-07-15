# Effectual

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/effectual-neoforge-1.4.0-1.21.1.jar`
- 标识与版本：effectual 1.4.0-1.21.1
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Effectual 为 ProjectNautic 提供与玩家状态、环境交互和实体行为相关的沉浸式粒子与视觉效果，直接服务于"视觉沉浸感"设计支柱。它覆盖呼吸雾气、气泡、奔跑尘埃、雪地脚印、金属碰撞火花、矿车火花、凋灵腐蚀等细节，让玩家在世界中的存在感和操作反馈更加真实。

该模组与 Visuality、Particular、Ambiance 同属环境粒子音效层：Effectual 聚焦"玩家与实体行为反馈"，Visuality 聚焦"接触点微粒子"，Particular 聚焦"宏观自然现象"，Ambiance 聚焦"特定对象氛围"。

## 玩家可见行为

- **可发现性**：玩家在寒冷环境呼吸会看到 mouthSteam；在水中、箱子/罐子旁看到气泡；奔跑时扬起尘埃；雪地留下脚印与残留；攻击金属类目标或矿车运行时看到火花；靠近凋灵看到腐蚀粒子；破坏/放置方块、剥皮有额外效果。
- **交互**：无需主动操作；效果由玩家行为与状态自动触发。
- **进度/奖励**：无内置进度；效果作为在世界中活动的视觉奖励。
- **摩擦/失败**：大量脚印与尘埃在持续移动中可能增加粒子负载；雪地脚印在多人场景下若同步异常可能出现视觉不同步。

## 集成关系

- **依赖**：Minecraft 1.21.1（required, CLIENT）。
- **冲突**：无已知严重冲突。
- **协作**：与 **Visuality**、**Particular**、**Ambiance** 同属环境粒子层；与 **Particular** 的洞穴尘埃、气泡效果存在互补。
- **配方/标签**：无直接参与。
- **任务**：无。
- **世界生成**：无。
- **UI**：无新增 UI，配置依赖配置文件。
- **资源**：依赖内置粒子纹理，无需额外资源包。
- **脚本**：无直接 KubeJS 交互。
- **加载顺序假设**：标准 CLIENT 端加载。

## 配置意图

`config/effectual.json` 当前启用了绝大多数效果，仅在洞穴尘埃频率上保持克制：

- **呼吸与空气**：`mouthSteam=true`（寒冷呼气雾气）、`dynamicBreathSpeed=true`（动态呼吸速度）、`bubbleBreath=true`（水下呼吸气泡）、`airTrail=true`（移动空气拖尾）全部开启，强化玩家存在感。
- **容器/水源气泡**：`bubbleChests=true`、`bubblePots=true`、`waterDrip=true` 开启，为容器与水滴添加生活气息。
- **火焰改进**：`fireImprovements=true`、`campfireImprovements=true`、`torchImprovements=true`、`lanternImprovements=true`、`fireEntitySparks=true` 全部开启，提升火源的视觉丰富度。
- **金属与矿车火花**：`metalHitSparks=true`、`furnaceSparks=true`、`minecartSparks=true` 开启，增强战斗与机械反馈。
- **方块交互**：`blockPlaceEffect=true`、`stripEffect=true`、`cauldronFillEffect=true` 开启，为放置、剥皮、装水提供额外粒子。
- **洞穴尘埃**：`caveDust=true`、`caveDustFrequency=5`，开启但频率较低，避免与 Particular 的洞穴尘埃过度叠加。
- **运动反馈**：`runDust=true`（奔跑尘埃）、`snowFootprints=true`（雪地脚印）、`snowResidue=true`（雪地残留）开启，提升移动真实感。
- **环境威胁**：`witherDecay=true` 开启，强化凋灵威胁的视觉表现。

## 兼容性与性能

- **客户端/服务端范围**：仅 **CLIENT**。
- **存档影响**：无。
- **已知不兼容**：无。
- **资源成本**：脚印、尘埃、火花等效果对 GPU 粒子填充率有持续压力；长距离奔跑或大量矿车运行时可能短暂增加负载。
- **缓解措施**：`caveDustFrequency=5` 与 Particular 的洞穴尘埃错开密度；所有效果均可在配置中单独关闭。

## 验证

- [ ] 启动或加载测试
- [ ] 功能测试
- [ ] 多人游戏测试（如适用）
- [ ] 性能或世界生成测试（如适用）
- [ ] 相关系统的回归检查

## 风险与开放问题

1. Effectual 与 Particular 都提供洞穴尘埃/气泡类效果，需实机验证是否产生重复粒子。
2. 雪地脚印与奔跑尘埃在持续移动中可能积累大量粒子，需在低端设备上测试稳定性。

## 历史

- 2026-07-16: 作为第三批 QoL 模组添加
