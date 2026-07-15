# Effect Descriptions

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/EffectDescriptions-v21.1.1-1.21.1-NeoForge.jar`
- 标识与版本：`effectdescriptions` `21.1.1`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Effect Descriptions 在药水、食物、效果组件等物品提示以及效果图标提示中显示状态效果的文字说明。它支撑整合包**信息透明**的设计意图，让玩家无需查阅外部资料即可理解各种模组新增效果的实际作用，降低学习成本。

## 玩家可见行为

- **可发现性**：鼠标悬停药水、喷溅药水、滞留药水、药箭、可疑 stew 等物品时，提示中出现对应效果的描述文字。
- **交互**：
  - 物品提示中默认直接显示效果描述（`shift_to_reveal = false`）。
  - 状态效果图标（生存/创造库存左侧）悬停时显示描述、属性与名称/持续时间。
- **进度/奖励**：无直接关联。
- **摩擦/失败**：若某些模组效果未提供本地化描述，可能显示为空或英文键名。

## 集成关系

- **依赖**：NeoForge `[21.1.21,)`（required, BOTH）；Minecraft `[1.21.1]`（required, BOTH）；**Puzzles Lib** `[21.1.7,)`（required, BOTH）。
- **冲突**：当前批次中无已知硬冲突。
- **协作**：与 JEI / JEED 互补，前者在配方浏览器中展示效果信息，后者在原版物品提示与效果图标中补充描述。
- **配方/标签**：默认支持 `minecraft:potion`、`splash_potion`、`lingering_potion`、`tipped_arrow`、`suspicious_stew`，可通过配置扩展。
- **任务**：无。
- **世界生成**：无。
- **UI**：在物品提示与效果组件提示中注入描述文本。
- **资源**：无。
- **脚本**：无 KubeJS 交互。
- **加载顺序假设**：依赖 Puzzles Lib，标准加载顺序即可。

## 配置意图

`config/effectdescriptions-client.toml` 选择最大化信息透明度：

- `description_indentation = 0`：描述文本不额外缩进，保持提示紧凑。
- `[items]` 分组：
  - `food_effects = true`：在食物提示中显示效果说明。
  - `supported_items` 保留默认列表，覆盖药水、药箭、可疑炖菜等常见含效果物品。
  - `shift_to_reveal = false`：无需 Shift 直接显示描述。
  - `description = true`：启用物品效果描述。
- `[widgets]` 分组：
  - `description = true`：在状态效果图标提示中显示描述。
  - `attributes = true`：显示效果带来的属性加成。
  - `name_and_duration = "NAME_AND_DURATION"`：即使图标本身已显示名称与持续时间，提示中仍重复显示，方便阅读。
  - `mod_name = false`、`internal_id = false`：不显示内部 ID 与模组名，避免提示过于冗长。

## 兼容性与性能

- **客户端/服务端范围**：标记为 BOTH，但实际功能为客户端提示增强；服务端仅用于加载依赖。
- **存档影响**：无。
- **已知不兼容**：无。
- **资源成本**：仅在渲染物品提示与效果提示时动态生成文本，开销可忽略。
- **缓解措施**：若未来某些模组效果描述缺失，可通过资源包或该模组的配置补充本地化键。

## 验证

- [ ] 启动或加载测试
- [ ] 功能测试（药水、食物、效果图标提示）
- [ ] 多人游戏测试（如适用）
- [ ] 性能或世界生成测试（如适用）
- [ ] 相关系统的回归检查

## 风险与开放问题

1. 模组新增效果若未提供中文本地化，提示可能显示英文或键名，影响中文玩家体验。
2. 与某些重写效果 HUD 的模组同时使用时，提示位置可能重叠。

## 历史

- 2026-07-16: 作为第三批 QoL 模组添加
