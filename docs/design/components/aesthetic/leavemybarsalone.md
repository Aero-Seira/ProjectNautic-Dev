# Leave My Bars Alone（改进的骑乘 HUD）

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/[改进的骑乘HUD] LeaveMyBarsAlone-v21.1.2-1.21.1-NeoForge.jar`
- 标识与版本：mod_id = `leavemybarsalone`；版本 `21.1.2`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Leave My Bars Alone（Fuzs）让玩家骑乘（马等）时仍能看到自己的饥饿条与经验条（verified，mods.toml 描述："Makes your food and experience bars visible when riding on a horse."）。原版骑乘时这两条被坐骑生命条完全替换，玩家无法判断自身饥饿状态——这是典型的类原版 QoL 修复。

## 玩家可见行为

- 骑乘时饥饿条与经验条与坐骑生命条并存显示。

## 集成关系

- **依赖**：Minecraft 1.21.1、NeoForge `[21.1.21,)`、PuzzlesLib `[21.1.14,)`（required）。
- **协作**：与 AppleSkin、Overflowing Bars 同处条带区，骑乘场景布局需回归。

## 配置意图

`config/leavemybarsalone-client.toml`（6 行）保持默认。

## 兼容性与性能

- 仅客户端渲染；无存档影响；开销可忽略。

## 验证

- [ ] 启动或加载测试
- [ ] 骑乘场景 HUD 布局回归（含 AppleSkin 叠加）

## 风险与开放问题

- 无已知重大风险。

## 历史

- 2026-07-21: 作为第六批 HUD 体系模组添加
