# Placebo

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/Placebo-1.21.1-9.9.1.jar`
- 标识与版本：mod_id = `placebo`；版本 `9.9.1`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Placebo 是 Shadows_of_Fire 系列模组的共享工具库，提供注册辅助、数据生成、配方处理、容器工具以及流浪商人交易的数据包加载能力。它在包内属于基础设施层：依赖它的模组可以复用这些经过验证的通用实现，降低重复开发成本。

在 ProjectNautic 中，它支撑**可维护性**与**可扩展性**支柱，使 FastSuite 等 Shadows_of_Fire 模组能够稳定、高效地处理配方与数据包逻辑。

## 玩家可见行为

普通玩家不会直接看到 Placebo 的名称。其功能通过依赖它的模组间接体现，例如 FastSuite 的配方匹配优化。

## 集成关系

- **依赖**：Minecraft `[1.21.1,)`、NeoForge `[21.1.187,)`（required, BOTH）
- **被依赖**：
  - **fastsuite**：需要 Placebo `>=9.9.0`（required, BOTH）
- **冲突**：当前批次中无已知硬冲突。
- **协作**：作为 Shadows_of_Fire 生态的基础设施，与未来可能添加的 Apotheosis、Gateways to Eternity 等模组共享底层工具。
- **配方/标签**：通过数据包扩展流浪商人交易列表（当前未清空默认交易）。
- **任务**：无。
- **世界生成**：无。
- **UI**：无直接 UI。
- **资源**：无。
- **脚本**：无 KubeJS 脚本交互需求。
- **加载顺序假设**：标准。Placebo 需在 fastsuite 之前完成加载。

## 配置意图

`config/placebo.cfg`：

```cfg
wandering_trader {
    B:"Clear Generic Trades"=false
    B:"Clear Rare Trades"=false
}
```

- **`Clear Generic Trades: false`**：不清空流浪商人的普通交易列表。数据包加载的自定义交易将追加到默认交易之后。
- **`Clear Rare Trades: false`**：不清空流浪商人的稀有交易列表。保持默认行为，避免意外移除原版或其他数据包添加的稀有交易。

这两项设置均为服务端权威（server-authoritative），影响多人游戏服务器上的交易生成。

## 兼容性与性能

- **客户端/服务端范围**：BOTH。作为库模组，两侧均需安装。
- **存档影响**：无直接存档影响。流浪商人交易在每次生成时动态计算。
- **已知不兼容**：当前批次中无已知冲突。
- **资源成本**：库模组开销极低；数据包交易加载在生成流浪商人时产生短暂 CPU 开销。
- **缓解措施**：保持与 fastsuite 的版本同步；避免手动删除或替换。

## 验证

- [ ] 启动或加载测试
- [ ] fastsuite 配方性能测试
- [ ] 多人游戏测试（如适用）
- [ ] 流浪商人交易数据包行为检查

## 风险与开放问题

1. **版本绑定**：Placebo 的更新与 fastsuite 紧密耦合，且 Placebo 9.x 与 Shadows_of_Fire 生态的其他模组共享版本节奏。
2. **交易覆盖策略**：当前未清空默认交易，若未来需要通过数据包彻底重写流浪商人交易，需将两项 `Clear` 设置为 `true`。

## 历史

- 2026-07-20: 升级 9.9.1 → 9.9.2（例行小版本升级，替换旧 jar；下游 FastSuite 需回归）
- 2026-07-19: jar 文件更新（版本仍为 9.9.1，无功能 delta）
- 2026-07-16: 作为第三批 QoL 模组添加
