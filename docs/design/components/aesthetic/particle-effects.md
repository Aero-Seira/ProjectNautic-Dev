# Particle Effects

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/ParticleEffects-1.5.0+1.21.1+neoforge.jar`
- 标识与版本：particle_effects 1.5.0+1.21.1+neoforge
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Particle Effects 是 ProjectNautic 的粒子效果配置库，为其他依赖它的模组提供统一的粒子配置与运行时基础设施。它本身不直接向玩家暴露新行为，但作为 **Inventory Particles** 等模组的底层依赖，确保粒子系统的配置语义和性能控制一致。

在整合包中，该模组的定位是"美学层基础设施"：它本身不是视觉效果的来源，但决定了上层模组如何读取配置、如何限制粒子数量、如何淡出粒子。

## 玩家可见行为

Particle Effects 作为库模组，玩家不会直接感知其存在。其功能通过依赖它的模组（如 **Inventory Particles**）间接体现：物品栏中的粒子生成规则、最大粒子数、透明度、删除策略等均可能受本库影响。

## 集成关系

- **依赖**：Minecraft 1.21.1（required, CLIENT）；被 **Inventory Particles** 依赖。
- **冲突**：无已知严重冲突。
- **协作**：作为 **Inventory Particles** 的底层配置库。
- **配方/标签**：无直接参与。
- **任务**：无。
- **世界生成**：无。
- **UI**：无新增 UI。
- **资源**：无额外资源需求。
- **脚本**：无直接 KubeJS 交互。
- **加载顺序假设**：作为库模组，需在依赖它的模组之前加载；标准 NeoForge 加载顺序自动处理。

## 配置意图

`config/particle_effects.json5` 当前极为精简，仅包含：

- `"mod_enabled": true`：启用库本身。

该文件是 Particle Effects 库的全局开关。具体粒子规则（如最大粒子数 5000、透明度 1.0、删除策略 oldest 等）目前由上层模组 Inventory Particles 在 `config/inventory_particles.json5` 中自行配置。因此本库的配置意图是"保持默认启用，作为配置入口占位，未来若库提供全局参数可在此统一调整"。

## 兼容性与性能

- **客户端/服务端范围**：仅 **CLIENT**（作为 CLIENT 端粒子的配置库）。
- **存档影响**：无。
- **已知不兼容**：无。
- **资源成本**：库本身开销可忽略；实际粒子开销由上层模组决定。
- **缓解措施**：通过上层模组的配置控制粒子数量与行为。

## 验证

- [ ] 启动或加载测试
- [ ] 功能测试
- [ ] 多人游戏测试（如适用）
- [ ] 性能或世界生成测试（如适用）
- [ ] 相关系统的回归检查

## 风险与开放问题

1. 当前配置几乎为空，未来若 Particle Effects 升级引入新的全局配置键，需要同步更新此文件以利用新能力。
2. 作为 Inventory Particles 的依赖，其版本兼容性需在 Inventory Particles 更新时一并检查。

## 历史

- 2026-07-16: 作为第三批 QoL 模组添加
