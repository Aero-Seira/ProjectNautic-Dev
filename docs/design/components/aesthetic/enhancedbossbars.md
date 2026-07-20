# Enhanced Boss Bars

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified（元数据）；inferred（具体视觉行为）
- 来源路径：`mods/[更好的Boss血条] enhancedbossbars-1.0.0.jar`
- 标识与版本：mod_id = `enhancedbossbars`；版本 `1.0.0`；作者 Naku
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Enhanced Boss Bars 提供改进的 Boss 血条渲染（verified，mods.toml 描述："New improved boss bars!"）。具体视觉样式（分段、动画、配色）元数据未说明，标 inferred。该模组服务于战斗体验的视觉反馈层，与 GUI Tween 的 bossGroup 动画（Boss 血条出现/受击动画）存在叠加关系。

## 玩家可见行为

- Boss 战中屏幕顶部血条呈现改进样式（具体样式 inferred，待实机截图确认）。

## 集成关系

- **依赖**：NeoForge `[21.1.218,)`、Minecraft `[1.21.1]`（required, BOTH）。
- **协作/冲突**：与 GUI Tween 的 Boss 血条动画（`bossGroup`，当前随主开关关闭）、Overflowing Bars（玩家条，不冲突）同处血条视觉层；与 Gnetum 移除后的 HUD 渲染回归需观察。

## 配置意图

- 未产生独立配置文件（或尚未生成，unknown），使用默认样式。

## 兼容性与性能

- 主要客户端视觉；BOTH 声明。无存档影响；开销极低。

## 验证

- [ ] 启动或加载测试
- [ ] Boss 战血条视觉确认（截图存档）
- [ ] 多 Boss 同屏显示回归

## 风险与开放问题

1. 模组为 1.0.0 首版且 mods.toml 含未展开的构建占位符（`META-INF/mods.toml` 为模板残留），打包质量待观察。
2. 与 GUI Tween bossGroup 同时启用时可能动画冲突。

## 历史

- 2026-07-21: 作为第六批 HUD/血条体系模组添加
