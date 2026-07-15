# Inventory Particles

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/[物品栏粒子] InventoryParticles-2.6.0+1.21.1+neoforge.jar`
- 标识与版本：inventory_particles 2.6.0+1.21.1+neoforge
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Inventory Particles 为 ProjectNautic 的物品栏交互添加粒子反馈，直接服务于"UI 愉悦感"设计支柱。当玩家拾取、移动、丢弃物品时，物品会拖出一串小粒子，使原本枯燥的物品管理操作变得更有触感。

该模组与 Extra Sounds、Tiny Item Animations、Smooth Swapping 共同构成 ProjectNautic 的物品栏体验层：Extra Sounds 提供音效反馈，Tiny Item Animations 提供缩放动画，Smooth Swapping 提供位移动画，Inventory Particles 提供粒子轨迹。

## 玩家可见行为

- **可发现性**：玩家移动物品、拾取物品、使用快速移动、滚动物品栏或悬浮在格子上时，会看到粒子从物品位置散发或跟随光标移动。
- **交互**：无需主动配置即可生效；玩家可在配置文件中调整粒子数量、透明度、淡出时长、生成触发条件等。
- **进度/奖励**：无内置进度；粒子作为物品操作的视觉奖励。
- **摩擦/失败**：粒子数量过多可能遮挡物品栏内容；在频繁操作时可能产生视觉疲劳。

## 集成关系

- **依赖**：Minecraft 1.21.1（required, CLIENT）；依赖 **Particle Effects** 作为配置库。
- **冲突**：无已知严重冲突。
- **协作**：与 **Extra Sounds**、**Tiny Item Animations**、**Smooth Swapping** 共同强化物品栏交互反馈。
- **配方/标签**：无直接参与。
- **任务**：无。
- **世界生成**：无。
- **UI**：无新增独立 UI，粒子叠加在原有物品栏 UI 上。
- **资源**：依赖内置粒子纹理，无需额外资源包。
- **脚本**：无直接 KubeJS 交互。
- **加载顺序假设**：标准 CLIENT 端加载，需在 Particle Effects 之后初始化。

## 配置意图

`config/inventory_particles.json5` 当前配置在保持效果的同时做了明显克制，避免粒子泛滥：

- **白名单**：`whitelists` 下的 `cursor_config`、`hovered_slot_config`、`all_slots_config`、`gui_action_config` 全部 `mode: "disabled"`，表示不通过白名单限制特定物品，所有物品统一使用黑名单/全局规则。
- **家族生成**：`family_generation` 中 `mods_mode` 与 `items_mode` 均为 `blacklist`，`mods` 与 `items` 为空，表示默认对所有模组和物品生效，未来可按需屏蔽。
- **缓存**：`invalidate_mode="manual_invalidate"`，手动刷新缓存，减少运行时自动刷新的开销。
- **粒子参数**：
  - `max_particles=5000`：上限足够高，避免粒子被过早截断。
  - `particle_transparency=1.0`：完全不透明，保持清晰。
  - `fade_out_duration_ticks=0`：不淡出，粒子直接消失，降低持续渲染开销。
  - `particle_deletion_type="oldest"`：超出上限时删除最老的粒子，保证最新反馈可见。
- **触发开关**：
  - 启用：`cursor_spawn_enabled=true`（跟随光标）、`gui_action_spawn_enabled=true`（GUI 动作）、`gui_action_put_spawn_enabled=true`（放入）、`gui_action_take_spawn_enabled=true`（取出）、`gui_action_quick_move_spawn_enabled=true`（快速移动）。
  - 禁用：`hovered_slot_spawn_enabled=false`（悬浮格子不产生粒子）、`all_slots_spawn_enabled=false`（非悬浮/非动作状态不产生全屏粒子），避免无操作时物品栏持续有粒子干扰。
- **系数**：`gui_action_config` 的 `cooldown_coefficient=100.0` 明显高于其他（1.0），表示 GUI 动作类粒子的冷却较长，防止连续操作时粒子爆发。

## 兼容性与性能

- **客户端/服务端范围**：仅 **CLIENT**。
- **存档影响**：无。
- **已知不兼容**：无。
- **资源成本**：粒子为 CLIENT 端开销；`max_particles=5000` 上限虽高，但因触发条件克制，实际粒子数远低于上限。
- **缓解措施**：通过禁用悬浮与全屏常驻粒子、提高 GUI 动作冷却系数、零淡出时长来控制视觉噪音与性能。

## 验证

- [ ] 启动或加载测试
- [ ] 功能测试
- [ ] 多人游戏测试（如适用）
- [ ] 性能或世界生成测试（如适用）
- [ ] 相关系统的回归检查

## 风险与开放问题

1. `max_particles=5000` 在极端情况下可能成为性能隐患，需在高频操作场景中验证实际粒子数量。
2. 与 Smooth Swapping 的物品位移动画叠加时，粒子轨迹可能与动画终点错位，需实机测试。

## 历史

- 2026-07-16: 作为第三批 QoL 模组添加
