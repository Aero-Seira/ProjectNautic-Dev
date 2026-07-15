# KubeJS Additions

## 记录

- 类型：mod / integration
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/kubejsadditions-neoforge-1.21.1-6.0.0.jar`
- 标识与版本：mod_id = `kubejsadditions`；版本 `1.21.1-6.0.0`
- 加载器或包格式：NeoForge 21.1.235

## 设计作用

KubeJS Additions 是 KubeJS 的功能扩展层，为整合包提供额外的脚本事件和 API，补充 KubeJS 核心未覆盖的场景。

支持的设计支柱：
- **可扩展性（KubeJS）**：通过扩展 KubeJS 的可用事件集合，使脚本能够响应更多游戏内行为，实现更精细的魔改控制。
- **UI 一致性**：与 Jade 的可选集成允许 KubeJS 脚本向 Jade 注册自定义提示信息，确保魔改内容在信息提示层面与原生模组内容体验一致。

KubeJS Additions 的定位是**增强器而非替代者**：它不做 KubeJS 本身能做到的事，而是做 KubeJS 做不到或做不好的事。

## 玩家可见行为

### 可发现性
- KubeJS Additions 本身对玩家**不可见**。玩家看到的是其脚本产出的效果。

### 交互
- 玩家通过常规游戏系统与 KubeJS Additions 扩展的功能交互，如：
  - 通过 Jade 查看 KubeJS 自定义方块的详细信息
  - 触发 KubeJS Additions 新增的特定事件（如某些实体交互事件）

### 进度与奖励
- KubeJS Additions 提供的新事件类型可以被 KubeJS 脚本监听，进而触发进度更新或奖励发放。

### 摩擦与失败状态
- 若 KubeJS Additions 与 KubeJS 版本不兼容，可能导致启动时崩溃或脚本加载失败。
- 过度依赖 KubeJS Additions 的扩展事件可能导致脚本与 KubeJS 核心事件的混淆，增加维护难度。

## 集成关系

### 依赖
- **NeoForge** `>=21.1.31`（required, AFTER, BOTH）
- **KubeJS** `>=2101.7.1-build.192`（required, AFTER, BOTH）—— 核心依赖，KubeJS Additions 没有独立存在价值

### 可选依赖
- **JEI** `>=19.14.1.143`（optional, AFTER, CLIENT）—— 支持在 JEI 中注册 KubeJS Additions 提供的额外配方信息
- **Jade** `>=15.10.5+neoforge`（optional, AFTER, CLIENT）—— 支持 KubeJS 脚本为 Jade 提供自定义提示内容

### 冲突
- 与提供类似 KubeJS 扩展功能的模组（如某些事件桥接模组）可能存在功能重叠，但通常不会直接冲突。

### 配方与标签
- KubeJS Additions 本身不注册新配方或标签，但扩展的 API 可能被 KubeJS 脚本用于更复杂的配方条件判断。

### 任务与世界生成
- 无直接关联。

### UI
- 通过可选的 Jade 集成，KubeJS Additions 允许脚本为特定方块或实体注册 Jade 提示提供者。例如：
  ```javascript
  // 示例：为自定义机器方块添加 Jade 能量显示
  JadeEvents.information(event => {
    event.add('kubejs:custom_machine', (tooltip, block) => {
      tooltip.add(`Energy: ${block.getEnergyStored()} / ${block.getMaxEnergyStored()} FE`)
    })
  })
  ```

### 资源
- 无独立资源需求。依赖 KubeJS 的资源加载机制。

### 加载顺序
- KubeJS Additions 必须在 KubeJS 之后加载（`AFTER`）。其注册的事件和 API 在 KubeJS 脚本执行时才能被调用。

## 配置意图

KubeJS Additions 本身不暴露独立的配置文件。其行为完全由 KubeJS 脚本通过其扩展 API 控制。

整合包对 KubeJS Additions 的使用策略：
- **按需启用**：不为了使用而使用。只有在 KubeJS 核心 API 确实无法满足需求时才引入 KubeJS Additions 的功能。
- **文档记录**：任何使用 KubeJS Additions 扩展 API 的脚本，必须在代码注释中说明依赖的是 KubeJS Additions 的哪个功能，以便后续维护时理解为何不用 KubeJS 原生 API。
- **版本锁定**：KubeJS Additions 的版本必须与 KubeJS 版本严格匹配。更新 KubeJS 时必须同时验证 KubeJS Additions 的兼容性。

## 兼容性与性能

### 客户端/服务端范围
- 标记为 `BOTH`。其提供的事件扩展在两侧都可能使用，具体取决于脚本需求。

### 存档影响
- 无直接存档数据。所有行为通过 KubeJS 脚本间接实现。

### 已知不兼容
- KubeJS 主版本升级时（如 2101.x 到 2201.x），KubeJS Additions 可能需要同步更新。两者版本不匹配是已知的风险点。

### 资源成本
- KubeJS Additions 本身开销极小，主要成本来自其启用的脚本逻辑。若脚本使用了大量扩展事件监听，性能影响取决于脚本实现而非 KubeJS Additions 本身。

### 缓解措施
- 在 `mods` 目录中严格维护 KubeJS 和 KubeJS Additions 的版本对应关系。
- 更新流程中增加"验证 KubeJS Additions 兼容性"步骤。

## 验证

- [ ] 启动或加载测试
- [ ] 功能测试
- [ ] 多人游戏测试（如适用）
- [ ] 性能或世界生成测试（如适用）
- [ ] 相关系统的回归检查

## 风险与开放问题

1. **文档不足风险**：KubeJS Additions 的扩展 API 文档通常不如 KubeJS 核心完善。使用其高级功能时可能需要阅读源码或社区讨论。
2. **维护活跃度**：作为 All Rights Reserved 许可的模组，KubeJS Additions 的更新频率和长期维护依赖于单一作者。若作者停止维护，而 KubeJS 继续更新，可能出现版本断层。
3. **功能边界模糊**：随着 KubeJS 核心不断更新，部分 KubeJS Additions 的功能可能被吸收到核心中。需要定期审查 KubeJS 更新日志，判断是否有可以"上游化"的脚本（即从 KubeJS Additions API 迁移到 KubeJS 原生 API）。

## 历史

- 2026-07-15: 作为基础 QoL 与魔改框架批次添加
