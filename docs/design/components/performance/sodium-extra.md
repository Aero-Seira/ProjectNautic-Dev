# Sodium Extra 钠 · 扩展

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/[钠 · 扩展] sodium-extra-neoforge-0.9.3+mc1.21.1.jar`
- 标识与版本：mod_id = `sodium_extra`；版本 `0.9.3+mc1.21.1`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Sodium Extra 是 ProjectNautic 整合包 Sodium 渲染优化的扩展模组，为 Sodium 增加额外的视频设置选项、HUD 覆盖层与渲染控制功能。它在不牺牲 Sodium 核心性能收益的前提下，为玩家提供更多可调整的视觉与性能参数，支撑整合包**性能优先**与**UI 一致性**的设计支柱。

该模组为 CLIENT ONLY，作为 Sodium 的配套组件存在，与 Sodium 共同构成完整的客户端渲染优化方案。

## 玩家可见行为

- **可发现性**：玩家在视频设置中会看到 Sodium Extra 新增的"Extra""Details""Animation""Particle""Render"等设置页，以及左上角的 FPS 覆盖层。
- **交互**：
  - 在 Sodium 视频设置中开关各类动画、粒子、细节渲染
  - 通过 FPS 覆盖层实时查看帧率信息
  - 调整调试 HUD 的刷新行为
- **进度/奖励**：无直接进度关联。
- **摩擦**：默认保留原版动画/粒子/细节效果，不会因安装模组而突然关闭视觉效果。
- **失败状态**：配置错误可能导致 FPS 覆盖层信息异常，但不会影响游戏核心功能。

## 集成关系

- **依赖**：Minecraft `[1.21.1,1.21.2)`、Sodium `>=0.8.12+mc1.21.1`（required, CLIENT）；greenlight（optional, CLIENT）
- **冲突**：当前批次中无已知硬冲突。
- **协作**：作为 Sodium 的扩展存在；FPS 覆盖层与 Sodium 的视频设置界面风格统一。
- **配方/标签**：无直接参与
- **任务**：无
- **世界生成**：无
- **UI**：扩展 Sodium 视频设置页，注入 FPS/坐标覆盖层
- **资源**：无额外资源需求
- **脚本**：无 KubeJS 脚本交互需求
- **加载顺序**：需在 Sodium 之后加载

## 配置意图

### `config/sodium-extra-options.json`

- **动画设置**：全部保持 `true`（水、岩浆、火、传送门、方块动画、幽匿感测体），保留原版动态视觉效果。
- **粒子设置**：全部保持 `true`（粒子总开关、雨滴溅落、方块破坏、方块破坏中、其他），不减少游戏反馈粒子。
- **细节设置**：天空、太阳、月亮、星星、雨雪、生物群系颜色、天空颜色全部启用，保持原版氛围。
- **渲染设置**：
  - `fog_settings` 使用默认原版雾效，未启用高级雾控制。
  - `light_updates: true`、`item_frame: true`、`armor_stand: true` 等细节渲染全部保留。
- **额外设置**：
  - `overlay_corner: TOP_LEFT`：FPS 覆盖层位于左上角，避免遮挡中央准星与重要 HUD。
  - `show_fps: true` / `show_f_p_s_extended: true`：显示扩展 FPS 信息，便于玩家与开发者快速评估性能。
  - `show_coords: false`：不显示坐标，避免与潜在的小地图模组坐标显示重复。
  - `steady_debug_hud: true`：稳定调试 HUD 刷新，避免 F3 信息闪烁。

### `config/sodium-extra.properties`

仅包含默认空提示文件，说明该文件仅用于调试，不做覆盖。

整体配置策略为"保留原版视觉体验，仅添加性能监控覆盖层"，符合 Vanilla+ 起步的设计理念。

## 兼容性与性能

- **客户端/服务端范围**：CLIENT ONLY。服务端无需安装。
- **存档影响**：无。Sodium Extra 不修改存档数据。
- **已知不兼容**：当前批次中无已知冲突。
- **资源成本**：FPS 覆盖层增加极少量 HUD 渲染开销；其余选项保持默认，无额外性能负担。
- **缓解措施**：若玩家不需要 FPS 覆盖层，可在 Sodium 设置中关闭。

## 验证

- [ ] 启动或加载测试
- [ ] 功能测试（Sodium 设置页扩展、FPS 覆盖层）
- [ ] 多人游戏测试（如适用，CLIENT 端独立生效）
- [ ] 性能测试（FPS 覆盖层对帧率的影响）
- [ ] 与 Sodium 的渲染回归检查

## 风险与开放问题

1. **与未来 HUD 模组的空间竞争**：FPS 覆盖层位于左上角，若未来添加小地图或状态模组，需统一规划 HUD 布局。
2. **坐标显示选择**：`show_coords: false` 避免了重复，但若玩家没有小地图，可能需要额外坐标来源。

## 历史

- 2026-07-16: 作为第二批基础模组与 QoL 扩展批次添加
