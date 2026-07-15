# Architectury

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/architectury-13.0.8-neoforge.jar`
- 标识与版本：mod_id = `architectury`；版本 `13.0.8`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Architectury 是跨加载器抽象库，为同时支持 NeoForge/Fabric 的模组提供统一的注册、网络、事件与平台检测接口。它在包内属于基础设施层：隐藏加载器差异，让依赖它的模组只需维护一套代码库，从而降低跨平台维护成本。

在 ProjectNautic 中，它支撑**技术一致性**与**可维护性**支柱，使效果、UI 等模组能在不同加载器版本间平滑迁移，而不需要为 NeoForge 单独重写大量逻辑。

## 玩家可见行为

普通玩家不会直接感知 Architectury。它没有任何玩家可见的 UI、物品或游戏机制，所有功能都通过依赖它的模组间接体现。

## 集成关系

- **依赖**：Minecraft `[1.21,)`、NeoForge `[21.0.110-beta,)`（required, BOTH）
- **被依赖**：
  - **effectual**：需要 Architectury `>=13.0.8`（required, CLIENT）
  - **tlib**：需要 Architectury `>=13.0.8`（required, BOTH）
- **冲突**：当前批次中无已知硬冲突。
- **协作**：作为底层抽象层，与 Cloth Config、TLib 等库协同工作。
- **配方/标签**：无直接参与。
- **任务**：无。
- **世界生成**：无。
- **UI**：无直接 UI。
- **资源**：无。
- **脚本**：无 KubeJS 脚本交互需求。
- **加载顺序假设**：标准。NeoForge 按依赖关系自动排序，Architectury 需在 effectual、tlib 之前完成加载。

## 配置意图

Architectury 本身不暴露 Minecraft 级别的配置文件。其行为由以下因素决定：

- **版本锁定**：effectual 与 tlib 均要求 Architectury `>=13.0.8`。当前安装的 `13.0.8` 满足此约束。更新 Architectury 前需先确认所有消费者的兼容性声明。
- **平台抽象**：包内所有使用 Architectury 的模组均运行于 NeoForge，因此 Fabric 分支相关功能不会被触发，保持最小化干预。

## 兼容性与性能

- **客户端/服务端范围**：BOTH。作为库模组，两侧均需安装。
- **存档影响**：无直接存档影响。
- **已知不兼容**：当前批次中无已知冲突。
- **资源成本**：库模组开销极低，仅在启动时注册少量事件与网络通道。
- **缓解措施**：保持与依赖它的模组版本同步；避免手动删除或替换。

## 验证

- [ ] 启动或加载测试
- [ ] 依赖模组（effectual、tlib）功能测试
- [ ] 多人游戏测试（如适用）
- [ ] 与依赖模组的回归检查

## 风险与开放问题

1. **版本绑定**：Architectury 的更新与 effectual、tlib 等消费者紧密耦合，不能独立升级 major 版本。
2. **跨加载器语义差异**：虽然抽象层统一了接口，但 NeoForge 与 Fabric 在事件顺序、侧端行为上仍有细微差异，若未来引入 Fabric 分支需额外测试。

## 历史

- 2026-07-16: 作为第三批 QoL 模组添加
