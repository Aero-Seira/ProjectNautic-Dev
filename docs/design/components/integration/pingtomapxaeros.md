# Ping to Map: Xaero's Minimap & Ping Wheel Addon

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/pingtomapxaeros-1.1.0-neoforge-1.21.1.jar`
- 标识与版本：mod_id = `pingtomapxaeros`；版本 `1.1.0`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Ping to Map 是 [Ping Wheel](../utility/pingwheel.md) 与 Xaero 地图系之间的**联动桥**：队友（或自己）发出 ping 时，自动在 Xaero 小地图/世界地图上生成一个临时路径点（verified，mods.toml 描述：通过本地聊天书式触发 Xaero 路径点提示，Xaero 缺席时静默回退）。它把即时沟通信号沉淀为可持续导航的地图标记，是多人协作体验闭环的关键一环，归入 integration。

## 玩家可见行为

- 收到 ping 后聊天栏出现 Xaero "添加到路径点"提示，确认后生成临时路径点。
- 默认路径点与 ping 同生命周期（`syncWithPingWheel=true`），ping 消失时路径点一并消失，避免地图污染。

## 集成关系

- **依赖**：NeoForge `>=21.1`、Minecraft `1.21.1`、**Ping Wheel**（required, CLIENT, ordering AFTER）。
- **软依赖**：Xaero's Minimap / World Map（无声明式依赖，缺席时静默不动作）。
- 纯客户端（mods.toml `side=CLIENT`），服务端无需安装。

## 配置意图

- **`config/pingtomapxaeros-client.toml`**：`enabled=true`；`registerOwnPings=true`（自己的 ping 也生成路径点，方便回找）；`syncWithPingWheel=true`（路径点生命周期跟随 ping，体现"临时标记"而非"永久路径点"的设计定位）。

## 兼容性与性能

- 纯客户端，无存档影响；开销可忽略。

## 验证

- [ ] 启动测试
- [ ] 联动测试（ping → 路径点提示 → 到期消失）

## 风险与开放问题

1. 依赖 Xaero 聊天书式机制，Xaero 大版本更新后需回归验证（本次已同步升级 Xaero 两模组）。

## 历史

- 2026-07-19: 作为第四批视觉与协作体验批次添加
