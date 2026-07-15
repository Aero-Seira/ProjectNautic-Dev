# MossyLib

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/MossyLib-1.5.0+1.21.1+neoforge.jar`
- 标识与版本：mod_id = `mossylib`；版本 `1.5.0+1.21.1+neoforge`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

MossyLib 是 MossyMods 系列模组的共享工具库，提供注册、网络、配置与通用工具方法。它在包内属于基础设施层：依赖它的 MossyMods 可以复用这些通用实现，减少重复代码。

在 ProjectNautic 中，它支撑**可维护性**支柱，使物品栏粒子等 MossyMods 内容能够稳定地运行于 NeoForge 21.1.235。

## 玩家可见行为

普通玩家不会直接看到 MossyLib 的名称。其功能通过依赖它的模组间接体现。

## 集成关系

- **依赖**：NeoForge `[1,)`、Minecraft `1.21.1`（required, BOTH）
- **被依赖**：
  - **inventory_particles**：需要 MossyLib `>=1.0.5`（required, BOTH）
- **冲突**：当前批次中无已知硬冲突。
- **协作**：作为 MossyMods 生态的基础设施，与未来可能添加的其他 MossyMods 共享。
- **配方/标签**：无直接参与。
- **任务**：无。
- **世界生成**：无。
- **UI**：无直接 UI。
- **资源**：无。
- **脚本**：无 KubeJS 脚本交互需求。
- **加载顺序假设**：标准。MossyLib 需在 inventory_particles 之前完成加载。

## 配置意图

MossyLib 本身不暴露 Minecraft 级别的配置文件。其行为由以下因素决定：

- **版本锁定**：inventory_particles 要求 MossyLib `>=1.0.5`。当前安装的 `1.5.0` 满足此约束。
- **最小化干预**：包内未对 MossyLib 做任何自定义配置，依赖模组的具体配置保留在各自的配置文件中。

## 兼容性与性能

- **客户端/服务端范围**：BOTH。作为库模组，两侧均需安装。
- **存档影响**：无直接存档影响。
- **已知不兼容**：当前批次中无已知冲突。
- **资源成本**：库模组开销极低。
- **缓解措施**：保持与 inventory_particles 的版本同步；避免手动删除或替换。

## 验证

- [ ] 启动或加载测试
- [ ] inventory_particles 功能测试
- [ ] 多人游戏测试（如适用）
- [ ] 与 inventory_particles 的回归检查

## 风险与开放问题

1. **版本绑定**：MossyLib 的更新与 inventory_particles 紧密耦合。
2. **生态扩展风险**：若未来添加更多 MossyMods，需确保它们对 MossyLib 的版本要求一致。

## 历史

- 2026-07-16: 作为第三批 QoL 模组添加
