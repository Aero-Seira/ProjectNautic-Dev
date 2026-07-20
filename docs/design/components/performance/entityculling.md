# EntityCulling（实体渲染机制优化）

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/[实体渲染机制优化] entityculling-neoforge-1.10.5-mc1.21.1.jar`
- 标识与版本：mod_id = `entityculling`；版本 `1.10.5`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

EntityCulling 使用异步路径追踪（async path-tracing）跳过不可见实体与方块实体的渲染（verified，mods.toml 描述）。整合包内实体纹理（Entity Texture Features）、环境粒子与大量 HUD 模组已使渲染压力偏高，实体剔除是对 Sodium（地形渲染）与 Flerovium（实体/地形剔除 shortcut）在"被遮挡实体"维度上的直接补充，归入 performance。

## 玩家可见行为

- 无直接可见 UI；预期表现为实体密集场景（农场、刷怪区、多人聚集）下帧率更平稳。
- 被遮挡实体的名牌默认仍渲染（`renderNametagsThroughWalls=true`），避免隔墙"消失"的名牌造成困惑。

## 集成关系

- **依赖**：Minecraft `[1.21.1]`、NeoForge `[20.2,)`（required）；`displayTest=IGNORE_ALL_VERSION`，服务端可不装。
- **嵌入库**：jar 内 `META-INF/jars/` 携带 TRansition 1.0.21 与 TRender 1.0.15（tr7zw 渲染支持库），二者生成 `config/transition.json`、`config/trender.json`（全默认）。
- **协作**：与 Sodium、Flerovium、ImmediatelyFast 叠加；Entity Texture Features 的随机纹理在剔除边界切换时可能闪变，需回归。

## 配置意图

`config/entityculling.json`（configVersion 8）：

- **`tracingDistance=128`**：默认追踪距离，覆盖常规视距内的剔除判定。
- **`tickCulling=true`**：对不可见实体同时跳过 tick 渲染；白名单包含全部船类、display 实体、Create 结构等（模组预置，防止功能性实体被误剔除）。
- **白名单**：`blockEntityWhitelist` 含 `minecraft:beacon` 等模组预置项；均为默认，未做包级自定义。

## 兼容性与性能

- 主要作用于客户端渲染；`IGNORE_ALL_VERSION` 允许客户端独立安装。
- 异步追踪有少量 CPU 开销（`sleepDelay=10`、`captureRate=5` 为默认节流），净收益预期为正。

## 验证

- [ ] 启动或加载测试
- [ ] 实体密集场景帧时间对比
- [ ] 与 ETF / Flerovium / Sodium 的渲染回归（剔除边界闪烁、发光纹理）
- [ ] 名牌与战利品光束等特殊渲染回归

## 风险与开放问题

1. 与光影管线（Complementary + EuphoriaPatches）下实体阴影/发光的交互未验证。
2. `tickCulling` 白名单包含大量本包未装模组的条目（Create、Botania 等），属模组预置冗余，无影响。

## 历史

- 2026-07-21: 作为第六批 HUD/性能/机制模组添加（性能矩阵补齐实体剔除维度）
