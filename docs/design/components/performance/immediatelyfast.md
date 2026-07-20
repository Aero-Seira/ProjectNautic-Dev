# ImmediatelyFast

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/ImmediatelyFast-NeoForge-1.6.11+1.21.1.jar`
- 标识与版本：`immediatelyfast` 1.6.11+1.21.1
- 加载器或包格式：NeoForge >=21.0.0

## 设计作用

ImmediatelyFast 是 ProjectNautic 整合包的客户端 HUD/GUI 渲染优化模组，与 Sodium（区块渲染优化）形成互补分工：

- **Sodium** 负责优化世界几何的渲染（区块、实体、地形）
- **ImmediatelyFast** 负责优化屏幕空间的立即模式渲染（HUD、物品栏、字体、地图、粒子 UI）

这种分工确保了整合包在**性能优先**设计支柱下的渲染优化覆盖完整渲染管线。ImmediatelyFast 通过批量处理（batching）、纹理图集化（atlas generation）和缓冲区优化（buffer upload）来减少 HUD 渲染中的 draw call 数量和 CPU-GPU 同步开销。在含有大量 HUD 元素的场景（如打开大型箱子 UI、查看地图、复杂状态效果显示）中，其优化效果尤为明显。

## 玩家可见行为

- **可发现性**：玩家通常不会直接感知模组存在，但会在以下场景间接受益：
  - 打开背包/箱子 UI 时更流畅
  - 手持地图或使用地图模组时帧率更稳定
  - 聊天框大量消息滚动时无明显卡顿
  - 状态效果栏、经验条、血条等 HUD 元素更新更平滑
- **交互**：无直接交互界面。
- **进度/奖励**：无直接进度关联。间接奖励为 UI 交互的流畅度提升。
- **摩擦**：无额外摩擦。配置文件已预设最优参数，开箱即用。
- **失败状态**：若启用实验性功能（如 `experimental_screen_batching`）可能导致特定 UI 渲染异常。当前配置已禁用所有实验性功能，风险可控。

## 集成关系

- **依赖**：Minecraft [1.21, 1.21.1]（CLIENT）、NeoForge >=21.0.0（CLIENT）
- **冲突**：当前批次中无已知冲突。ImmediatelyFast 内置了模组冲突检测和资源包冲突处理机制。
- **配方/标签**：无直接关联
- **任务**：无
- **世界生成**：无
- **UI**：在 HUD 和 GUI 渲染管线中注入优化逻辑，不修改 UI 布局或视觉风格，保持 UI 一致性设计支柱
- **资源**：优化地图纹理和字体纹理的图集生成，可能轻微增加显存占用（通常 < 10 MB）
- **脚本**：无 KubeJS 脚本交互需求
- **加载顺序**：标准模组加载顺序即可，无特殊要求

## 配置意图

ImmediatelyFast 的配置文件 `config/immediatelyfast.json` 经过以下深思熟虑的设置：

### 常规优化（REGULAR）
- `font_atlas_resizing: true` — 启用字体纹理图集动态调整大小，确保字体渲染使用最优纹理尺寸，减少纹理切换开销。
- `map_atlas_generation: true` — 启用地图纹理图集化，将多张地图合并到单个纹理图集中，大幅降低地图渲染时的 draw call 数量。
- `hud_batching: true` — 启用 HUD 批量渲染，将多个 HUD 元素（血条、饥饿条、经验条、状态效果图标等）合并为更少的绘制调用，是模组最核心的优化之一。
- `fast_text_lookup: true` — 启用快速文本查找，优化字体渲染中的字形索引性能，加速聊天框、物品名称等文本渲染。
- `fast_buffer_upload: false` — 禁用快速缓冲区上传。当前配置选择保守策略，避免在某些驱动或 GPU 组合上因缓冲区路径变更导致的渲染异常；以少量潜在性能收益换取更高稳定性。

### 实验性功能（EXPERIMENTAL）
所有实验性功能均保持 `false`，体现保守且稳定优先的配置策略：
- `experimental_sign_text_buffering: false` — 禁用告示牌文本缓冲。启用可能在某些字体渲染场景下产生 glitch。
- `experimental_screen_batching: false` — 禁用屏幕批量渲染。虽然理论上可进一步优化 UI 渲染，但可能导致某些复杂 GUI（如合成台、附魔台）显示异常。
- `experimental_disable_error_checking: false` — 保留错误检查，确保在出现问题时能够及时发现和诊断。
- `experimental_disable_resource_pack_conflict_handling: false` — 保留资源包冲突处理机制，避免自定义资源包导致的渲染异常。

### 调试功能（DEBUG）
全部保持 `false`，这些选项仅在排查 ImmediatelyFast 自身问题时由开发者临时启用：
- `debug_only_and_not_recommended_disable_universal_batching: false`
- `debug_only_and_not_recommended_disable_mod_conflict_handling: false`
- 其余调试选项同理，保持关闭以获取完整优化效果。

### 仅 cosmetic 选项
- `dont_add_info_into_debug_hud: false` — 允许在 F3 调试界面显示 ImmediatelyFast 的状态信息，便于性能排查时获取数据。

## 兼容性与性能

- **客户端/服务端范围**：CLIENT ONLY。不适用于服务端安装。
- **存档影响**：无。ImmediatelyFast 不修改存档格式或游戏数据。
- **已知不兼容**：当前批次中无已知不兼容模组。模组内置了与 Sodium、Iris 等主流渲染模组的兼容性处理。
- **资源成本**：
  - 显存：字体和地图图集可能增加少量显存占用（通常 < 20 MB）
  - CPU：降低 HUD 渲染阶段的 CPU 开销
  - 内存：无显著影响
- **缓解措施**：
  - 已禁用所有实验性功能，最大化稳定性
  - 保留资源包冲突处理机制，确保自定义资源包兼容性
  - 保留模组冲突处理机制，降低与其他客户端模组冲突风险

## 验证

- [ ] 启动或加载测试
- [ ] 功能测试
- [ ] 多人游戏测试（如适用）
- [ ] 性能或世界生成测试（如适用）
- [ ] 相关系统的回归检查

## 风险与开放问题

1. **与 Sodium 的协同验证**：虽然 ImmediatelyFast 和 Sodium 优化不同渲染阶段，但两者均深入 OpenGL 渲染管线，需验证在长时间游戏过程中是否存在协同导致的稳定性问题（如显存泄漏、驱动兼容性问题）。
2. **自定义资源包兼容性**：若玩家自行添加高分辨率字体或复杂地图资源包，图集生成可能超出硬件限制。需关注社区反馈。
3. **未来 UI 模组的影响**：若后续添加如 JEI、REI、FTB Quests 等大量使用自定义 GUI 的模组，需评估 ImmediatelyFast 的 HUD batching 是否会产生交互问题。

## 历史

- 2026-07-21: `hud_batching` true → false（第六批 HUD 模组大量加入后关闭 HUD 批处理，规避与新 HUD 渲染层的叠加风险；Gnetum 同批移除，HUD 性能维度暂时回到无分帧/无批处理基线）
- 2026-07-16: 更新配置说明：`fast_buffer_upload` 实际为 `false`，采用保守策略以换取稳定性
- 2026-07-15: 作为基础性能模组批次添加
