# Configuration

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/configuration-neoforge-1.21.1-3.1.1.jar`
- 标识与版本：mod_id = `configuration`；版本 `3.1.1`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Configuration 是配置库模组，为依赖它的模组提供声明式配置定义、序列化与同步能力。它在包内属于基础设施层：开发者通过注解或构建器描述配置项，而 Configuration 负责生成配置文件、处理命令与网络同步。

在 ProjectNautic 中，它支撑**可配置性**与**可维护性**支柱，使氛围（ambiance）等模组能够以统一方式向玩家暴露设置，而无需各自维护一套配置系统。

## 玩家可见行为

普通玩家不会直接看到 Configuration 的名称。其功能通过依赖它的模组间接体现，例如 ambiance 的配置界面与配置文件读写。

## 集成关系

- **依赖**：NeoForge `[21.1.1,22)`、Minecraft `[1.21.1,1.22)`（required, BOTH）
- **被依赖**：
  - **ambiance**：需要 Configuration `*`（required, BOTH）
- **冲突**：当前批次中无已知硬冲突。
- **协作**：作为配置基础设施，与依赖模组共同管理游戏内设置。
- **配方/标签**：无直接参与。
- **任务**：无。
- **世界生成**：无。
- **UI**：无直接 UI，配置界面由依赖模组暴露。
- **资源**：无。
- **脚本**：无 KubeJS 脚本交互需求。
- **加载顺序假设**：标准。Configuration 需在 ambiance 之前完成加载。

## 配置意图

`config/configuration-options.json`：

```json
{
  "advancedMode": false
}
```

- **`advancedMode: false`**：保持 Configuration 的简化模式，不向普通玩家暴露高级配置选项。该设置主要影响配置界面或命令的可选项范围，当前保持关闭以降低玩家认知负担。

Configuration 本身的核心配置由依赖它的模组各自持有（如 `config/ambiance.json`）。

## 兼容性与性能

- **客户端/服务端范围**：BOTH。配置同步机制在两侧都需要存在。
- **存档影响**：无直接存档影响。
- **已知不兼容**：当前批次中无已知冲突。
- **资源成本**：库模组开销极低，配置读写仅在启动或设置变更时发生。
- **缓解措施**：保持与 ambiance 的版本同步；避免手动删除或替换。

## 验证

- [ ] 启动或加载测试
- [ ] ambiance 配置可正常读写与同步
- [ ] 多人游戏测试（如适用）
- [ ] 与 ambiance 的回归检查

## 风险与开放问题

1. **版本绑定**：Configuration 的更新与 ambiance 等消费者紧密耦合。
2. **高级模式未启用**：若未来 ambiance 需要暴露更多调试或高级选项，需重新评估是否开启 `advancedMode`。

## 历史

- 2026-07-16: 作为第三批 QoL 模组添加
