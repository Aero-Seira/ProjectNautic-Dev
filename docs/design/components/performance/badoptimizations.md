# BadOptimizations

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/BadOptimizations-2.4.1-1.21.1.jar`
- 标识与版本：`badoptimizations` 2.4.1
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

BadOptimizations 是一组针对“非渲染”环节的客户端微优化，通过缓存光照图、天空颜色、实体/方块实体渲染器、FOV 计算等结果，减少每帧重复计算。它与 Sodium（渲染管线）和 Flerovium（渲染剔除）形成互补，进一步压低 CPU 端的每帧开销，支撑整合包“性能优先”的支柱。模组默认开启大多数优化，玩家无需手动干预。

## 玩家可见行为

- **可发现性**：日常游玩中几乎无感知，仅在 F3 左上角看到 `BadOptimizations <version>` 字样。偶尔在复杂场景或方块实体密集处感受到更稳定的帧时间。
- **交互**：无独立 GUI 或设置页，全部通过 `config/badoptimizations.txt` 调整。
- **进度/奖励**：无直接关联。间接奖励是更平滑的帧率与更低的微卡顿。
- **摩擦/失败**：若与实体渲染缓存存在已知冲突的模组（如 Twilight Forest、BedrockSkinUtility、SkinShuffle）同时安装，模组会自动禁用对应优化；强行设置 `ignore_mod_incompatibilities: true` 可能导致启动崩溃。

## 集成关系

- **依赖**：Minecraft 1.21.1（CLIENT）、NeoForge
- **冲突**：实体渲染缓存与 Twilight Forest、BedrockSkinUtility、SkinShuffle 存在已知不兼容（已由模组自动处理）
- **协作**：与 Sodium、Lithium、Flerovium 协同，覆盖渲染与逻辑两侧的性能优化
- **配方/标签**：无直接关联
- **任务**：无
- **世界生成**：不干预世界生成
- **UI**：在 F3 界面注入版本标识
- **资源**：无额外资源需求
- **脚本**：无 KubeJS 脚本交互
- **加载顺序假设**：标准加载顺序即可

## 配置意图

`config/badoptimizations.txt` 采用以默认优化全开为基础的保守策略：

- `enable_lightmap_caching: true` / `lightmap_time_change_needed_for_update: 80` — 启用光照图缓存，每 80 tick（4 秒）才在无需求时更新一次，降低光照计算频率。
- `enable_sky_color_caching: true` / `skycolor_time_change_needed_for_update: 3` — 启用天空颜色缓存，在生物群系边界外每 3 tick 重新计算一次。
- 微优化区块全部启用，包括粒子管理器空队列跳过、toast 优化、实体/方块实体渲染器缓存、冗余 FOV 计算跳过、教学提示移除等。
- `show_f3_text: true` — 在 F3 中显示版本，便于排查与确认加载。
- `ignore_mod_incompatibilities: false` — 保留模组自带的冲突自动降级逻辑，不强行开启可能冲突的优化。
- `log_config: true` — 启动时打印完整配置，便于问题定位。

注意：`enable_entity_flag_caching` 在 1.20.5+ 已无效；`enable_sky_color_caching` 等在 1.21.11+ 会失效，但当前版本 1.21.1 仍可生效。

## 兼容性与性能

- **客户端/服务端范围**：CLIENT ONLY
- **存档影响**：无。不修改存档格式或游戏数据
- **已知不兼容**：Twilight Forest、BedrockSkinUtility、SkinShuffle（实体渲染缓存相关，已由模组自动规避）
- **资源成本**：主要降低 CPU 重复计算开销，内存占用增加可忽略
- **缓解措施**：保持 `ignore_mod_incompatibilities: false`，让模组自动关闭与冲突模组的优化项；通过 `log_config: true` 在报告问题时提供完整配置快照

## 验证

- [ ] 启动或加载测试
- [ ] 功能测试
- [ ] 多人游戏测试（如适用）
- [ ] 性能或世界生成测试（如适用）
- [ ] 相关系统的回归检查

## 风险与开放问题

1. **自定义实体渲染模组的潜在冲突**：自动不兼容检测清单可能未覆盖所有修改实体渲染器的模组，若玩家手动添加此类模组，实体渲染缓存可能导致崩溃。
2. **版本升级后的配置失效**：1.21.11+ 部分缓存逻辑将失效，未来升级 MC 版本时需要重新评估该模组价值与配置项。

## 历史

- 2026-07-16: 作为第三批性能模组添加
