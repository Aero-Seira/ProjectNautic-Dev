# KubeJS Data Component

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/kubejs_datacomponent-1.0.1.jar`
- 标识与版本：mod_id = `kubejs_datacomponent`；版本 `1.0.1`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

KubeJS Data Component 是 ProjectNautic 整合包 KubeJS 魔改框架的扩展插件，用于在 KubeJS 脚本中读取、修改和创建 Minecraft 1.21+ 引入的 `DataComponent` 数据组件系统。它是整合包**魔改即基础设施**设计支柱的关键组成部分：在 1.21 版本中，大量物品、方块与实体的附加数据已从 NBT 迁移到 DataComponent，缺少该插件将严重限制 KubeJS 对现代 Minecraft 数据系统的操作能力。

该模组属于 `integration` 分类：它本身不添加独立游戏内容，而是扩展 KubeJS 与 Minecraft 数据组件系统之间的集成能力。

## 玩家可见行为

- **可发现性**：普通玩家不会直接感知 KubeJS Data Component 的存在；其效果体现在 KubeJS 脚本能够正确创建和操作带有自定义数据组件的物品。
- **交互**：无直接交互界面。能力通过 KubeJS 脚本间接暴露给玩家（如自定义物品属性、修改工具提示等）。
- **进度/奖励**：无直接进度关联。它是未来魔改内容的基础设施。
- **摩擦**：无额外摩擦。无配置界面，安装即生效。
- **失败状态**：若 KubeJS 脚本使用了该插件未支持的 DataComponent API，可能导致脚本错误或功能缺失。

## 集成关系

- **依赖**：Minecraft `[1.21.1,1.22)`、NeoForge `>=21`（required, BOTH）；KubeJS（实际运行依赖）
- **冲突**：当前批次中无已知硬冲突。
- **协作**：
  - **KubeJS**：主魔改框架，该插件为其提供 DataComponent API
  - **KubeJS Additions / LootJS**：同属 KubeJS 扩展生态，未来可能共同用于复杂的内容魔改
- **配方/标签**：无直接参与
- **任务**：无
- **世界生成**：无
- **UI**：无
- **资源**：无
- **脚本**：KubeJS 脚本通过该插件访问 DataComponent 系统。典型使用场景包括：
  - 为自定义物品附加耐久、附魔、染色等组件数据
  - 在事件脚本中读取或修改物品的 `custom_data`、`damage`、`enchantments` 等组件
  - 与 LootJS 配合，在战利品中生成带有特定 DataComponent 的物品
- **加载顺序**：需在 KubeJS 之后加载

## 配置意图

未检测到独立的 KubeJS Data Component 配置文件。该模组为纯 API 扩展，无额外配置项。其能力完全通过 KubeJS 脚本调用，因此相关设计意图将在具体脚本设计文档中记录。

在 1.21 中，DataComponent 已逐步替代 NBT 成为物品附加数据的主流载体，该插件的存在确保 KubeJS 脚本不会落后于 Minecraft 的数据模型演进。

## 兼容性与性能

- **客户端/服务端范围**：BOTH。作为 KubeJS 扩展，需与 KubeJS 同步部署。
- **存档影响**：无直接存档影响。DataComponent 操作由脚本定义，需遵循脚本级别的存档兼容性规范。
- **已知不兼容**：当前批次中无已知冲突。
- **资源成本**：纯 API 插件，无运行时开销。
- **缓解措施**：保持与 KubeJS 版本同步；脚本中使用 DataComponent 时需参考该插件的 API 文档。

## 验证

- [ ] 启动或加载测试
- [ ] 功能测试（编写简单的 KubeJS 脚本读取/修改 DataComponent）
- [ ] 多人游戏测试（服务端与客户端脚本同步）
- [ ] 与 KubeJS 的回归检查

## 风险与开放问题

1. **文档与 API 覆盖度**：该插件版本为 `1.0.1`，DataComponent API 覆盖范围需在实际脚本开发中验证。
2. **与 KubeJS 版本绑定**：作为 KubeJS 插件，需密切关注 KubeJS 与 NeoForge 升级时的兼容性。
3. **脚本规范需求**：DataComponent 操作涉及物品数据持久化，未来需建立明确的脚本规范以避免存档兼容性问题。

## 历史

- 2026-07-16: 作为第二批基础模组与 QoL 扩展批次添加
