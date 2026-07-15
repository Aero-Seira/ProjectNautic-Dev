# Better Advancements 更好的进度

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/[更好的进度] BetterAdvancements-NeoForge-1.21.1-0.4.3.21.jar`
- 标识与版本：`betteradvancements` `0.4.3.21`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Better Advancements 重绘原版进度界面，使进度树、标签页和完成线更清晰可读。它支撑整合包**信息透明与 UI 一致性**的设计意图，让玩家在探索大量模组内容时能快速定位进度目标，减少在杂乱界面中迷失的挫败感。

该模组为 CLIENT ONLY，仅改变客户端显示，不修改进度数据本身。

## 玩家可见行为

- **可发现性**：玩家按 `L` 打开进度屏幕时，会看到重新排版的标签页、彩色完成线和更直观的图标布局。
- **交互**：
  - 已完成的进度以金色标题/图标高亮显示。
  - 部分完成的进度可展开查看已获取与未获取的标准（`criteriaDetail = "Default"`）。
  - 标签页保持原版按模组分组，但未按字母排序（`orderTabsAlphabetically = false`）。
- **进度/奖励**：无直接进度关联；仅作为查看原版的辅助界面。
- **摩擦/失败**：若与其他重写进度屏幕的模组共存，可能出现 UI 重叠或渲染异常。

## 集成关系

- **依赖**：NeoForge `>=21.1.0`（required, CLIENT）；Minecraft `[1.21.1, 1.22)`（required, CLIENT）。
- **冲突**：与同样替换原版进度界面的模组（如其他 Advancements UI 重写模组）可能冲突。
- **协作**：与 Clickable Advancements 协作，玩家从聊天点击进度名称后进入的即为此界面。
- **配方/标签**：无。
- **任务**：无。
- **世界生成**：无。
- **UI**：替换并增强原版进度屏幕（`AdvancementsScreen`）。
- **资源**：无额外资源包需求。
- **脚本**：无 KubeJS 交互。
- **加载顺序假设**：标准加载顺序即可；`displayTest="NONE"` 表示无需服务端同步。

## 配置意图

`config/betteradvancements-client.toml` 选择保留原版风格的同时增强可读性：

- `defaultCompletedTitleColor = "#DBA213"` / `defaultCompletedIconColor = "#DBA213"`：已完成进度使用金色标题与图标，视觉反馈明确。
- `defaultUncompletedTitleColor = "#0489C1"` / `defaultUncompletedIconColor = "#FFFFFF"`：未完成进度使用蓝色标题与白色图标，区分度高。
- `criteriaDetail = "Default"`：默认列出已获取的进度标准，帮助玩家了解还差哪些条件。
- `criteriaDetailRequiresShift = false`：无需按住 Shift 即可查看标准详情，降低操作成本。
- `doAdvancementsBackgroundFade = true`：启用进度界面背景渐变，提升视觉舒适度。
- `uiScaling = 100`：保持默认缩放，避免低分辨率屏幕上的布局异常。
- `addInventoryButton = false`：不在背包界面额外添加进度按钮，保持原版交互路径。
- `defaultDrawDirectLines = false`、`defaultHideLines = false`：使用默认连线显示方式，不过度简化进度树。

## 兼容性与性能

- **客户端/服务端范围**：CLIENT ONLY。服务端无需安装。
- **存档影响**：无。不修改进度数据。
- **已知不兼容**：与其他重写进度屏幕的模组可能冲突。
- **资源成本**：仅替换一个 GUI 屏幕，渲染开销可忽略。
- **缓解措施**：保持配置中不添加额外背包按钮，避免与 HUD/背包模组产生界面重叠。

## 验证

- [ ] 启动或加载测试
- [ ] 功能测试
- [ ] 多人游戏测试（如适用）
- [ ] 性能或世界生成测试（如适用）
- [ ] 相关系统的回归检查

## 风险与开放问题

1. 未来若添加自定义进度树或大量隐藏进度，默认连线与颜色可能显得拥挤，需要重新评估缩放与显示选项。
2. 与其他进度 UI 模组的兼容性需要实际加载验证。

## 历史

- 2026-07-16: 作为第三批 QoL 模组添加
