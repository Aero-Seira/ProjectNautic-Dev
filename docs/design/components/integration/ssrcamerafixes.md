# Shoulder Surfing Reloaded: Camera Fixes & Additions（SSR Camera Fixes）

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/ssrcamerafixes-neoforge-1.21.1-2.0.0.jar`
- 标识与版本：mod_id = `ssrcamerafixes`；版本 `2.0.0`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

SSR Camera Fixes 是 [Shoulder Surfing Reloaded](../aesthetic/shouldersurfing.md) 的**相机修复与扩展插件**（verified，mods.toml 描述），为 Epic Fight、Better Lockon、Iron's Spells、Better Combat 等战斗类模组提供越肩视角兼容。当前这些战斗模组均未安装，本模组处于**前向兼容空窗期**——预先就位以便后续战斗内容模组加入时相机体验不破碎，归入 integration。

## 玩家可见行为

- 当前：为越肩视角提供 OVERHEAD 预设等附加相机行为调整。
- 未来：安装 Epic Fight 等模组后自动修正其动画与越肩相机的错位问题。

## 集成关系

- **依赖**：NeoForge `>=21.1`、Minecraft `[1.21.1,1.22)`、**Shoulder Surfing `>=1.21.1-4.22.0`**（required, CLIENT, ordering AFTER）。
- **可选下游**：epicfight、betterlockon、irons_spellbooks、bettercombat（均 optional, CLIENT，当前未安装）。

## 配置意图

- **`config/ssrcamerafixes-client.toml`**：`disableFollowPlayerRotations=true`、`idleBehavior=DECOUPLED`（空闲时相机保持解耦，不自动回正到玩家朝向——与 Shoulder Surfing 的解耦相机定位一致，减少视角漂移感）；`cameraOverheadOffsetY=1.2`（俯视预设垂直偏移默认值）。

## 兼容性与性能

- 纯客户端，无存档影响；开销可忽略。

## 验证

- [ ] 启动测试
- [ ] 越肩视角空闲行为主观验证
- [ ] 未来安装 Epic Fight 等模组后的兼容回归（届时补测）

## 风险与开放问题

1. 战斗模组未到位期间价值有限，属有意的前置投资；若战斗线计划取消可移除。

## 历史

- 2026-07-19: 作为第四批视觉与协作体验批次添加
