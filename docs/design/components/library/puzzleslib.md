# Puzzles Lib

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/PuzzlesLib-v21.1.52-1.21.1-NeoForge.jar`
- 标识与版本：mod_id = `puzzleslib`；版本 `21.1.52`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Puzzles Lib 是 Fuzs 系列模组的共享工具库，提供跨加载器的注册、配置、网络、事件与数据生成抽象。它在包内属于基础设施层：依赖它的模组可以编写一次逻辑，由 Puzzles Lib 负责适配 NeoForge 与 Fabric 的差异。

在 ProjectNautic 中，它支撑**可维护性**与**跨平台一致性**支柱，使效果描述（EffectDescriptions）等 Fuzs 模组能够在 NeoForge 环境下稳定运行。

## 玩家可见行为

普通玩家不会直接看到 Puzzles Lib 的名称。其功能通过依赖它的模组间接体现。

## 集成关系

- **依赖**：NeoForge `[21.1.21,)`、Minecraft `[1.21.1]`（required, BOTH）
- **被依赖**：
  - **effectdescriptions**：需要 Puzzles Lib `>=21.1.7`（required, BOTH）
- **冲突**：当前批次中无已知硬冲突。
- **协作**：作为 Fuzs 生态的基础设施，与未来可能添加的更多 Fuzs 模组共享底层能力。
- **配方/标签**：无直接参与。
- **任务**：无。
- **世界生成**：无。
- **UI**：无直接 UI。
- **资源**：无。
- **脚本**：无 KubeJS 脚本交互需求。
- **加载顺序假设**：标准。Puzzles Lib 需在 effectdescriptions 之前完成加载。

## 配置意图

Puzzles Lib 本身不暴露 Minecraft 级别的配置文件。其行为由以下因素决定：

- **版本锁定**：effectdescriptions 要求 Puzzles Lib `>=21.1.7`。当前安装的 `21.1.52` 满足此约束。
- **跨加载器抽象**：包内所有使用 Puzzles Lib 的模组均运行于 NeoForge，因此 Fabric 分支相关功能不会被触发。

## 兼容性与性能

- **客户端/服务端范围**：BOTH。作为库模组，两侧均需安装。
- **存档影响**：无直接存档影响。
- **已知不兼容**：当前批次中无已知冲突。
- **资源成本**：库模组开销极低。
- **缓解措施**：保持与 effectdescriptions 的版本同步；避免手动删除或替换。

## 验证

- [ ] 启动或加载测试
- [ ] effectdescriptions 功能测试
- [ ] 多人游戏测试（如适用）
- [ ] 与 effectdescriptions 的回归检查

## 风险与开放问题

1. **版本绑定**：Puzzles Lib 的更新与 effectdescriptions 等 Fuzs 模组紧密耦合。
2. **生态扩展风险**：若未来添加更多 Fuzs 模组，需确保它们对 Puzzles Lib 的版本窗口一致。

## 历史

- 2026-07-16: 作为第三批 QoL 模组添加
