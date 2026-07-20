# Noisium

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/noisium-neoforge-2.7.0+mc1.21-1.21.1.jar`
- 标识与版本：mod_id = `noisium`；版本 `2.7.0+mc1.21-1.21.1`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Noisium 优化世界生成噪声计算性能（verified，mods.toml 描述："Optimises worldgen performance for a better gameplay experience"），填补原版世界生成管线中未被 C2ME（并发）覆盖的噪声采样效率空间。与 C2ME、Chunky（预生成）共同构成世界生成性能三角，归入 performance。

## 玩家可见行为

- 无 UI；预期表现为新区块生成更快、探索/跑图时卡顿减少。

## 集成关系

- **依赖**：Minecraft `>=1.21 <=1.21.1`、NeoForge `>=21.1.22`（required, BOTH）。
- **不兼容声明**：mods.toml 声明与 `biox` 模组 incompatible（世界生成崩溃）——本包未安装，无影响。
- **协作**：与 C2ME（区块并发）、Chunky（预生成）叠加。

## 配置意图

- 无独立配置文件（模组无可调项，verified by absence），行为全默认。

## 兼容性与性能

- BOTH 端；声称不改变世界生成结果（inferred，需用同种子对比验证）；无存档影响。

## 验证

- [ ] 启动或加载测试
- [ ] 同种子世界生成结果一致性抽查
- [ ] 新区块生成耗时对比（可结合 spark）

## 风险与开放问题

1. 世界生成优化若改变生成结果将影响存档连续性——必须同种子验证后再随服务端部署。

## 历史

- 2026-07-21: 作为第六批 HUD/性能/机制模组添加（性能矩阵补齐世界生成噪声维度）
