# Pick Up Notifier（拾取提示）

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/[拾取提示] PickUpNotifier-v21.1.1-1.21.1-NeoForge.jar`
- 标识与版本：mod_id = `pickupnotifier`；版本 `21.1.1`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Pick Up Notifier（Fuzs）在拾取物品时于屏幕角落显示"刚捡到了什么"的通知列表（verified，mods.toml 描述："Be notified about all the things you've just collected."）。解决战斗中/快速移动时拾取反馈缺失的问题，属 QoL 信息提示层。

## 玩家可见行为

- 拾取物品/经验时屏幕角落滑出物品图标与数量的简短通知，超时淡出。

## 集成关系

- **依赖**：Minecraft 1.21.1、NeoForge `[21.1.21,)`、PuzzlesLib `[21.1.0,)`（required）。
- **协作**：与 Inventory Particles（物品栏粒子）、Extra Sounds（拾取音效）共同构成拾取反馈层；HUD 角落位置与 ChatNotify、Xaero 小地图的布局需回归。

## 配置意图

`config/pickupnotifier/pickupnotifier-client.toml`（79 行）与 `pickupnotifier-server.toml`（6 行）保持默认，未做包级自定义。

## 兼容性与性能

- 客户端渲染 + 轻量服务端组件；无存档影响；开销极低。

## 验证

- [ ] 启动或加载测试
- [ ] 大量拾取场景通知堆积表现
- [ ] HUD 布局回归（与小地图/聊天通知）

## 风险与开放问题

- 无已知重大风险。

## 历史

- 2026-07-21: 作为第六批 Fuzs 系模组添加
