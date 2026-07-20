# Leaves Be Gone

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/LeavesBeGone-v21.1.1-1.21.1-NeoForge.jar`
- 标识与版本：mod_id = `leavesbegone`；版本 `21.1.1`；作者 Fuzs
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Leaves Be Gone（Fuzs）让砍树后的树叶快速腐烂消失（verified，mods.toml 描述："Quick leaf decay from cutting down trees. Built for great performance and mod compat!"）。消除砍树后树冠长时间悬空的观感问题，同时以高性能实现避免腐烂计算压力，属 QoL 机制优化——不改变可获得内容，符合"类原版起步"支柱。

## 玩家可见行为

- 砍掉树干后，关联树叶在数秒内快速腐烂掉落（而非原版的缓慢随机腐烂）。

## 集成关系

- **依赖**：Minecraft 1.21.1、NeoForge `[21.1.21,)`、PuzzlesLib `[21.1.7,)`（required, BOTH）；`displayTest=IGNORE_SERVER_VERSION`。
- **协作**：与 CullLeaves（树叶渲染剔除）职责不同（逻辑腐烂 vs 渲染剔除），可并存。

## 配置意图

`config/leavesbegone-server.toml`（13 行）保持默认（服务端配置，控制腐烂速度等），未做包级自定义。

## 兼容性与性能

- 逻辑在服务端；无存档影响；可随时移除。

## 验证

- [ ] 启动或加载测试
- [ ] 砍树后树叶腐烂速度与掉落物回归

## 风险与开放问题

- 无已知重大风险。

## 历史

- 2026-07-21: 作为第六批 Fuzs 系模组添加
