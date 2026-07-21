# GUI Tween

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/guitween-3.3.7-1.21.1+neoforge.jar`
- 标识与版本：mod_id = `guitween`；版本 `3.3.7`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

GUI Tween 为 GUI 添加动画效果（verified，mods.toml 描述："Added some GUI animation effects"），覆盖窗口打开/关闭、物品悬停/点击/拖拽、快捷栏、聊天栏、Boss 血条等多组动画。它是被移除的 SmoothGui 的替代者：SmoothGui 仅提供窗口打开/关闭动画，GUI Tween 覆盖面显著更广且按组可配。

**注意**：`config/guitween-client.toml` 顶层 `enable = true`（verified，2026-07-21 实机开启）——此前"试用遗留还是有意默认"的开放问题已由维护者以实际行动裁决：动画层正式启用，状态转为 active。

## 玩家可见行为

- 当前默认：全分组动画生效——窗口弹出带回弹位移（OUT_BACK）、物品悬停放大 1.2 倍、点击缩放、快捷栏选中物品名浮动、聊天栏滑入、Boss 血条出现/受击动画等。

## 集成关系

- **依赖**：NeoForge、Minecraft 1.21.1（required）。
- **替代关系**：替代 SmoothGui（2026-07-21 移除）。
- **协作/冲突**：与 Smooth Scrolling、Smooth Swapping、Chat Impressive Animation 动画层叠加；这也是主开关被关闭的首要怀疑原因（视听叠加风险，inferred）。内置 JEI 左右面板联动动画（`enableJeiLeft/Right=true`）。

## 配置意图

`config/guitween-client.toml`（238 行，按 windowGroup/screenItemGroup/hotbarGroup/chatGroup/bossGroup 分组）各组参数保持作者预设默认值，仅顶层 `enable` 由 false 改为 true。设计意图（verified by action）：维护者实机体验后认可作者预设动画参数，未做逐组微调即整组启用；与其他动画模组的叠加观感需在实机中继续观察。

## 兼容性与性能

- 仅客户端动画；无存档影响；开销低。

## 验证

- [ ] 启动或加载测试
- [ ] 启用后与 Smooth Scrolling/Swapping、Chat Impressive Animation 的动画叠加回归
- [ ] JEI 面板联动动画回归

## 风险与开放问题

1. ~~主开关 `enable=false` 是有意默认还是试用遗留~~ → 已裁决（2026-07-21 实机开启，转 active）。
2. 启用后与 Smooth Scrolling/Swapping、Chat Impressive Animation 的动画叠加观感需实机回归（对应"视听叠加风险"约束）。

## 历史

- 2026-07-21: 主开关 `enable` false → true（实机开启），状态 experimental → active；分组参数保持默认
- 2026-07-21: 作为第六批模组添加，替代 SmoothGui；主开关默认关闭，标为 experimental
