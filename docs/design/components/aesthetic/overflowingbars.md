# Overflowing Bars

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/OverflowingBars-v21.1.1-1.21.1-NeoForge.jar`
- 标识与版本：mod_id = `overflowingbars`；版本 `21.1.1`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Overflowing Bars（Fuzs）将生命、护甲、护甲韧性条扩展到原版上限之外：超过 20 点生命/护甲时以紧凑的多行/图标方式显示，而非混乱的变色堆叠（verified，mods.toml 描述："Expand health, armor and armor toughness bars beyond vanilla. Nice and compact without confusing colors."）。当前类原版阶段玩家属性不超标，此模组主要为未来内容章节（高护甲装备、生命提升效果）预留显示能力。

## 玩家可见行为

- 属性不超原版上限时无可见变化；超标时条带分行/计数显示而非重叠。

## 集成关系

- **依赖**：Minecraft 1.21.1、NeoForge `[21.1.21,)`、PuzzlesLib `[21.1.15,)`（required）。
- **协作**：与 AppleSkin（饥饿/饱和 HUD）、Leave My Bars Alone（骑乘 HUD）同处快捷栏上方条带区，布局叠加需回归。

## 配置意图

`config/overflowingbars-client.toml`（90 行）保持默认，未做包级自定义。

## 兼容性与性能

- 仅客户端渲染；无存档影响；开销极低。

## 验证

- [ ] 启动或加载测试
- [ ] 与 AppleSkin / Leave My Bars Alone 的 HUD 布局回归
- [ ] 超标属性场景（药水效果堆叠）显示验证

## 风险与开放问题

1. 类原版阶段无超标属性来源，模组价值在未来内容落地前无法验证。

## 历史

- 2026-07-21: 作为第六批 HUD/血条体系模组添加
