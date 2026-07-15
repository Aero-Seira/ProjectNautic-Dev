# Sodium

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/[钠] sodium-neoforge-0.8.12+mc1.21.1.jar`
- 标识与版本：`sodium` 0.8.12+mc1.21.1
- 加载器或包格式：NeoForge >=21.1.82

## 设计作用

Sodium 是 ProjectNautic 整合包的客户端渲染优化基线。它替代了原版 Minecraft 的渲染引擎，通过更高效的区块网格生成、剔除策略和 GPU 命令提交，显著提升帧率并减少卡顿。Sodium 直接支撑整合包的核心设计支柱——**性能优先**，确保中低端设备也能获得可接受的视觉体验，同时为后续可能的视觉效果模组留下性能余量。

由于 Sodium 是客户端专用模组，服务端无需安装，这降低了多人环境的服务端维护复杂度。

## 玩家可见行为

- **可发现性**：玩家在启动整合包后通常不会直接感知 Sodium 的存在，但会在视频设置中发现新增的"Sodium"设置按钮，提供比原版更细粒度的渲染选项。
- **交互**：玩家可通过 Sodium 的视频设置界面调整渲染距离、质量预设、动画策略等。
- **进度/奖励**：无直接进度关联。间接奖励为更流畅的游戏体验（更高的 FPS 和更少的微卡顿）。
- **摩擦**：无额外摩擦。配置文件中已预设适合大多数场景的优化参数，开箱即用。
- **失败状态**：若与 Embeddium 等不兼容模组同时安装，游戏将直接崩溃并提示冲突原因。

## 集成关系

- **依赖**：Minecraft 1.21.1（CLIENT）、NeoForge >=21.1.82（CLIENT）
- **冲突**：与 Embeddium 不兼容（原因：两者均为渲染引擎替代，功能重叠会导致冲突）
- **配方/标签**：无直接关联
- **任务**：无
- **世界生成**：不干预世界生成逻辑
- **UI**：在"视频设置"中注入 Sodium 专用设置页
- **资源**：无额外资源需求，不修改纹理或模型
- **脚本**：无 KubeJS 脚本交互需求
- **加载顺序**：标准模组加载顺序即可，无特殊要求

## 配置意图

Sodium 的配置文件 `config/sodium-options.json` 经过以下深思熟虑的设置：

### quality 区块
- `weather_quality: DEFAULT` — 保留原版天气渲染质量，不做降级处理，维持视觉一致性。
- `leaves_quality: DEFAULT` — 同样保留树叶渲染质量，避免植被外观异常。
- `enable_vignette: true` — 保留原版暗角效果，维持视觉沉浸感。
- `use_closest_point_entity_sort: false` — 禁用最近点实体排序，换取渲染性能提升。在常规游戏场景中此优化对视觉影响极小。

### advanced 区块
- `use_advanced_staging_buffers: true` — 启用高级暂存缓冲区，优化 CPU-GPU 数据传输效率，这是 Sodium 核心渲染优化的一部分。
- `cpu_render_ahead_limit: 3` — 限制 CPU 渲染预取队列深度，避免过度预渲染导致不必要的 CPU 占用，同时保持足够的 GPU 利用率。
- `enable_memory_tracing: false` — 禁用内存追踪，减少运行时开销。仅在调试内存问题时启用。

### performance 区块
- `chunk_builder_threads: 0` — 使用自动检测的线程数（通常为可用 CPU 核心数），避免手动配置导致的过线程或欠线程问题。
- `chunk_build_defer_mode: ALWAYS` — 始终延迟区块网格构建，优先保证主线程响应性，减少区块加载时的卡顿感。
- `animate_only_visible_textures: true` — 仅对可见纹理执行动画更新，显著降低纹理动画的 GPU 开销。
- `use_entity_culling: true` — 启用实体剔除，不渲染被遮挡的实体，降低复杂场景中的实体渲染负载。
- `use_fog_occlusion: true` — 利用雾效进行遮挡剔除，减少远处不可见区块的渲染工作。
- `use_block_face_culling: true` — 启用方块面剔除，不渲染被相邻方块遮挡的面，这是 Sodium 最基础也是最高效的优化之一。
- `use_no_error_g_l_context: true` — 使用无错误检查 OpenGL 上下文，避免驱动层频繁的 GL 错误查询开销。
- `quad_splitting_mode: SAFE` — 使用安全的四边形分割策略，在兼容性和性能之间选择稳妥方案。

### debug 区块
- `terrain_sorting_enabled: true` — 保持地形排序启用，确保透明方块（水、玻璃等）正确渲染。

## 兼容性与性能

- **客户端/服务端范围**：CLIENT ONLY。服务端无需安装，也不应将其放入服务端模组目录。
- **存档影响**：无。Sodium 不修改存档格式或游戏数据。
- **已知不兼容**：Embeddium（功能重叠，启动时直接冲突）
- **资源成本**：内存占用略有增加（用于更高效的缓冲区和数据结构），但在现代硬件上可忽略不计。
- **缓解措施**：已设置 `chunk_build_defer_mode: ALWAYS` 以最大化主线程响应性；`quad_splitting_mode: SAFE` 保证在各类 GPU 驱动上的稳定性。

## 验证

- [ ] 启动或加载测试
- [ ] 功能测试
- [ ] 多人游戏测试（如适用）
- [ ] 性能或世界生成测试（如适用）
- [ ] 相关系统的回归检查

## 风险与开放问题

1. **GPU 驱动兼容性**：`use_no_error_g_l_context` 在部分老旧驱动上可能存在兼容性问题，若收到此类报告需评估回退方案。
2. **与视觉模组的潜在冲突**：未来若添加如 Iris 等光影模组，需验证与 Sodium 的兼容性（Sodium 本身支持 Iris）。
3. **Embeddium 误装风险**：玩家在手动添加模组时可能误装 Embeddium，需在整合包说明文档中明确标注此冲突。

## 历史

- 2026-07-15: 作为基础性能模组批次添加
