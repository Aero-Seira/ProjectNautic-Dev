# Shoulder Surfing Reloaded

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/[越肩视角重制] ShoulderSurfing-NeoForge-1.21.1-5.0.7.jar`
- 标识与版本：mod_id = `shouldersurfing`；版本 `1.21.1-5.0.7`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Shoulder Surfing Reloaded 提供高度可配置的**越肩第三人称视角**（verified，mods.toml 描述），将原版第三人称升级为现代化的解耦相机。它为整合包提供动作化、沉浸式的视角选项，配合 [SSR Camera Fixes](../integration/ssrcamerafixes.md) 修正与战斗类模组的相机兼容性，属视觉体验层。

## 玩家可见行为

- 切换第三人称时默认进入越肩视角；相机与玩家朝向解耦（`decoupled_camera=true`），可自由环视。
- 相机靠近头部时自动隐藏玩家模型；传送/上下载具时相机自动定向。

## 集成关系

- **依赖**：Minecraft `>=1.21`、NeoForge `>=21.0.143`（required, CLIENT）。
- **冲突声明（verified，mods.toml）**：与 betterthirdperson、camerautils、nimble、customcameraview 不兼容；与 valkyrienskies `<=2.4.11` 不兼容——未来引入瓦尔基里天空需注意版本。
- **扩展**：`ssrcamerafixes`（required 依赖本模组 `>=1.21.1-4.22.0`，CLIENT）。

## 配置意图

- **`config/shouldersurfing-client.toml`**：保留默认解耦相机与过渡速度（`camera_transition_speed_multiplier=0.25`，平滑不晕眩）；`view_bobbing_mode=INHERIT` 继承原版视角摇晃设置；`center_player_sounds=false` 不以相机为中心重定位玩家音效（避免与 Presence Footsteps 等音效空间感冲突，inferred）。

## 兼容性与性能

- 纯客户端（displayTest=NONE），服务端无需安装；无存档影响。

## 验证

- [ ] 启动测试
- [ ] 功能测试（越肩视角切换、解耦相机、战斗场景）
- [ ] 与 SSR Camera Fixes 联动回归

## 风险与开放问题

1. 解耦相机在 PvP/精准操作场景的玩家接受度待反馈；保留切换回原版第三人称的途径。

## 历史

- 2026-07-19: 作为第四批视觉与协作体验批次添加
