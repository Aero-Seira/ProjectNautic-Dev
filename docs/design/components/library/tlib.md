# TLib

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/tlib-neoforge-1.5.0-1.21.1.jar`
- 标识与版本：mod_id = `tlib`；版本 `1.5.0-1.21.1`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

TLib 是 TLMods 系列模组的共享工具库，提供注册、网络、配置与通用工具方法。它在包内属于基础设施层：依赖它的模组可以复用这些通用实现，减少重复代码。TLib 同时依赖 Architectury，因此也间接受益于跨加载器抽象。

在 ProjectNautic 中，它支撑**可维护性**支柱，使 effectual 等 TLMods 内容能够稳定地运行于 NeoForge 21.1.235。

## 玩家可见行为

普通玩家不会直接看到 TLib 的名称。其功能通过依赖它的模组间接体现。

## 集成关系

- **依赖**：NeoForge `[21.1,)`、Minecraft `[1.21.1]`、Architectury `[13.0.8,)`（required, BOTH）
- **被依赖**：
  - **effectual**：需要 TLib `>=1.5.0-1.21.1`（required, CLIENT）
- **冲突**：当前批次中无已知硬冲突。
- **协作**：作为 TLMods 生态的基础设施，与 Architectury 协同工作；与未来可能添加的其他 TLMods 共享。
- **配方/标签**：无直接参与。
- **任务**：无。
- **世界生成**：无。
- **UI**：无直接 UI。
- **资源**：无。
- **脚本**：无 KubeJS 脚本交互需求。
- **加载顺序假设**：标准。TLib 需在 Architectury 之后、effectual 之前完成加载。

## 配置意图

TLib 本身不暴露 Minecraft 级别的配置文件。其行为由以下因素决定：

- **版本锁定**：effectual 要求 TLib `>=1.5.0-1.21.1`。当前安装的 `1.5.0-1.21.1` 满足此约束。
- **依赖链**：TLib 要求 Architectury `>=13.0.8`，因此 Architectury 的版本同步也需一并考虑。

## 兼容性与性能

- **客户端/服务端范围**：BOTH。作为库模组，两侧均需安装。
- **存档影响**：无直接存档影响。
- **已知不兼容**：当前批次中无已知冲突。
- **资源成本**：库模组开销极低。
- **缓解措施**：保持与 effectual 和 Architectury 的版本同步；避免手动删除或替换。

## 验证

- [ ] 启动或加载测试
- [ ] effectual 功能测试
- [ ] 多人游戏测试（如适用）
- [ ] 与 effectual、Architectury 的回归检查

## 风险与开放问题

1. **依赖链版本绑定**：TLib 的更新与 effectual 和 Architectury 均紧密耦合，升级任一组件时需同时验证。
2. **生态扩展风险**：若未来添加更多 TLMods，需确保它们对 TLib 与 Architectury 的版本要求一致。

## 历史

- 2026-07-16: 作为第三批 QoL 模组添加
