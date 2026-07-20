# Alternate Current

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/alternate_current-mc1.21-1.9.0.jar`
- 标识与版本：mod_id = `alternate_current`；版本 `1.9.0`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Alternate Current 重写红石粉信号传播：用高效、非位置依赖（non-locational）的 BFS 实现替代原版逐块更新算法（verified，mods.toml 描述："An efficient and non-locational redstone dust implementation"）。大型红石装置是服务端 MSPT 的常见杀手，本模组在不改变红石语义的前提下压缩红石线计算成本，与 Lithium 的逻辑优化互补，归入 performance。符合"类原版起步"支柱：不改变可获得内容，只优化机制性能。

## 玩家可见行为

- 无 UI；红石装置行为应与原版一致，大型红石机器场景下卡顿减少。
- 风险在于极少数依赖原版红石更新顺序/位置性的"特性机器"可能行为变化（inferred）。

## 集成关系

- **依赖**：NeoForge、Minecraft 1.21.1（required, BOTH）。
- **协作**：与 Lithium（逻辑优化）叠加；Lithium 亦含部分红石优化，二者叠加顺序与覆盖关系需回归验证。

## 配置意图

- 未产生包级自定义配置（模组本身几乎无配置项，unknown），使用默认行为。

## 兼容性与性能

- BOTH 端；服务端部署时收益最大（红石计算在服务端 tick）。
- 无存档影响；移除后红石恢复原版实现，存档安全。

## 验证

- [ ] 启动或加载测试
- [ ] 大型红石装置 MSPT 对比（服务端）
- [ ] 常见红石机器（飞行器、漏斗钟、0-tick 类）行为回归
- [ ] 与 Lithium 红石优化的叠加回归

## 风险与开放问题

1. 依赖原版红石位置性更新的特殊装置可能失效，需在说明中告知红石玩家。

## 历史

- 2026-07-21: 作为第六批 HUD/性能/机制模组添加（性能矩阵补齐红石维度）
