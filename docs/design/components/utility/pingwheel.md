# Ping Wheel

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/Ping-Wheel-1.12.2-neoforge-1.21.1.jar`
- 标识与版本：mod_id = `pingwheel`；版本 `1.12.2`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Ping Wheel 提供类现代合作游戏的**标记（ping）系统**：玩家对准位置按键即可在世界中放置队友可见的标记。它支撑"长线运营优先"支柱下的多人协作体验——无需语音即可快速沟通位置与意图，降低组队探索/战斗的沟通成本。

## 玩家可见行为

- 对准方块/方向按 ping 键放置标记，队友在世界内与屏幕上看到标记与方向指示。
- 标记带音效、图标与队伍颜色；显示持续一段时间后自动消失。
- 配合 [Ping to Map](../integration/pingtomapxaeros.md)（integration）可将 ping 同步为 Xaero 小地图临时路径点。

## 集成关系

- **依赖**：NeoForge `>=21.0.143`、Minecraft `[1.21,1.21.1]`（required, BOTH）。
- **协作**：`pingtomapxaeros`（required 依赖本模组，integration 页）。
- **冲突**：无已知硬冲突。

## 配置意图

- **`config/pingwheel.json`（客户端）**：`pingDuration=7`（标记存活 7 秒，足够队友反应又不残留噪音）；`pingDistance=2048`、`raycastDistance=1024`（远距离标记支持大地图协作）；`teamColorMode=FULL`（完整队伍配色，便于区分来源）；`playerInfoMode=HOLD`（按住才显示玩家信息，减少常驻 UI 噪音）。
- **`config/pingwheel.server.json`（服务端）**：`playerTrackingEnabled=true`、`rateLimit=5`、`msToRegenerate=1000`（限流防刷屏）；`defaultChannelMode=AUTO`（频道自动分配）。

## 兼容性与性能

- BOTH：标记数据需服务端中转，**服务端必须安装**才能多人使用。
- 无存档影响；网络开销极小，限流配置已设。

## 验证

- [ ] 多人功能测试（标记发送/接收/限流）
- [ ] 与 Ping to Map 联动回归

## 风险与开放问题

1. 大型服务器中 ping 频率上限是否需按运营规模调整。
2. 与聊天/通知类模组（ChatNotify）的提示噪音叠加待观察。

## 历史

- 2026-07-19: 作为第四批视觉与协作体验批次添加
