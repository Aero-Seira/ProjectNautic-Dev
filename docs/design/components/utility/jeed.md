# Just Enough Effects Descriptions

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/[JEI药水效果] jeed-1.21-2.3.2.jar`
- 标识与版本：mod_id = `jeed`；版本 `1.21-2.3.2`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Just Enough Effects Descriptions（JEED）是 ProjectNautic 整合包的状态效果信息扩展模组，作为 JEI/REI/EMI 的插件，为玩家提供状态效果的详细描述、来源配方与效果层级说明。它支撑整合包**QoL 工具**与**Vanilla+ 起步**的设计意图：在不改变原版药水系统的前提下，解决玩家面对大量模组效果时"这个效果到底有什么用"的信息缺口。

JEED 主要作用于 CLIENT 侧，依赖 JEI 的配方查看界面展示效果信息。

## 玩家可见行为

- **可发现性**：玩家在 JEI 中搜索状态效果或悬停效果图标时，会看到 JEED 提供的描述、颜色框、原料列表与相关配方。
- **交互**：
  - 查看状态效果的文字描述
  - 查看酿造该效果所需的原料
  - 查看效果的持续时间、等级与颜色标识
- **进度/奖励**：无直接进度关联。帮助玩家理解药水与效果系统。
- **摩擦**：无额外摩擦。信息集成在 JEI 界面中。
- **失败状态**：若 `hidden_effects` 配置不当，可能意外隐藏某些效果。

## 集成关系

- **依赖**：Minecraft `[1.21,1.22)`、NeoForge `>=4`（required, BOTH）
- **冲突**：当前批次中无已知硬冲突。
- **协作**：作为 JEI 插件存在；与 JER、AppleSkin 等共同构成完整的信息查询生态。
- **配方/标签**：无直接参与
- **任务**：无
- **世界生成**：无
- **UI**：在 JEI 中新增状态效果信息页
- **资源**：无
- **脚本**：无 KubeJS 脚本交互需求
- **加载顺序**：需在 JEI 之后加载

## 配置意图

`config/jeed-client.toml`：

```toml
effect_color = true
effect_box = true
ignore_derivative_potions = true
sort_ingredients = false
hidden_effects = [""]
ingredients_list = true
render_slots = false
replace_vanilla_tooltips = true
```

- **`effect_color = true`**：在提示中显示效果颜色，便于快速识别。
- **`effect_box = true`**：在效果图标后方绘制黑色背景框，提升可读性。
- **`ignore_derivative_potions = true`**：忽略延长/强化药水的衍生效果，避免 JEI 中效果条目过度重复。
- **`ingredients_list = true`**：显示效果原料列表，帮助玩家理解如何获得该效果。
- **`replace_vanilla_tooltips = true`**：替换原版在效果图标缩小时的提示，使用 JEED 提供更完整的信息。
- **`sort_ingredients = false`** / `render_slots = false`：使用默认排序与渲染模式。
- **`hidden_effects = [""]`**：未隐藏任何效果；保留空字符串占位，未来可扩展黑名单。

该配置策略在信息完整性与界面简洁性之间取得平衡，优先保证玩家能理解每个效果的作用。

## 兼容性与性能

- **客户端/服务端范围**：标记为 BOTH，但实际功能以 CLIENT 侧为主。
- **存档影响**：无。JEED 不修改存档数据或效果机制。
- **已知不兼容**：当前批次中无已知冲突。与某些自定义药水系统的模组可能显示不一致。
- **资源成本**：效果信息索引在 JEI 初始化时完成，运行时开销极低。
- **缓解措施**：若 JEI 中效果条目过多，可调整 `ignore_derivative_potions` 或 `hidden_effects`。

## 验证

- [ ] 启动或加载测试
- [ ] 功能测试（JEI 中查看效果描述、原料、配方）
- [ ] 多人游戏测试（如适用）
- [ ] 与 JEI 的回归检查

## 风险与开放问题

1. **衍生效果过滤边界**：`ignore_derivative_potions = true` 减少了重复条目，但可能让玩家遗漏延长/强化版本的信息，需根据实际药水模组规模调整。
2. **与未来药水模组的兼容性**：若未来添加大量自定义效果的内容模组，需验证 JEED 是否能正确读取其描述文本。

## 历史

- 2026-07-16: 作为第二批基础模组与 QoL 扩展批次添加
