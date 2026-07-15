# AppleSkin 苹果皮

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/[苹果皮] appleskin-neoforge-mc1.21-3.0.9.jar`
- 标识与版本：mod_id = `appleskin`；版本 `3.0.9+mc1.21`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

AppleSkin 是 ProjectNautic 整合包的食物与饥饿系统信息增强模组，在 HUD 与物品提示中显示饱和度、食物价值、消耗度与预计回复生命值等原版隐藏信息。它支撑整合包**Vanilla+ 起步**与**QoL 工具**的设计意图：在不改变原版饥饿机制的前提下，帮助玩家做出更明智的食物选择，降低因信息不透明导致的挫败感。

AppleSkin 为 CLIENT ONLY，仅影响客户端显示，不改变食物属性或玩家状态。

## 玩家可见行为

- **可发现性**：玩家打开背包、悬停食物物品或在 HUD 上观察饥饿条时，会看到额外的饱和度覆盖层与食物价值提示。
- **交互**：
  - 悬停食物时显示饥饿值、饱和度与预计回复生命值
  - 手持食物时 HUD 显示该食物将恢复的饥饿/饱和度覆盖层
  - 饥饿条后方显示消耗度进度条
- **进度/奖励**：无直接进度关联。间接帮助玩家在生存早期优化食物使用。
- **摩擦**：无额外摩擦。信息显示直观，符合原版 UI 风格。
- **失败状态**：配置错误可能导致 HUD 覆盖层重叠或信息缺失。

## 集成关系

- **依赖**：NeoForge `>=21.0.0-beta`（required, CLIENT）
- **冲突**：当前批次中无已知硬冲突。
- **协作**：与 JEI 的食物信息提示互补；与 Jade 的方块提示不冲突。
- **配方/标签**：无直接参与
- **任务**：无
- **世界生成**：无
- **UI**：在 HUD 饥饿条、生命值条与物品提示中注入信息
- **资源**：无
- **脚本**：无 KubeJS 脚本交互需求
- **加载顺序**：标准模组加载顺序即可

## 配置意图

`config/appleskin-client.toml` 中所有食物相关 HUD 信息功能均启用：

- **`showFoodValuesInTooltip = true`**：按住 Shift 时显示食物的饥饿与饱和度数值。
- **`showFoodValuesInTooltipAlways = true`**：无需 Shift 即可直接显示食物数值。
- **`showSaturationHudOverlay = true`**：在饥饿条上叠加当前饱和度显示。
- **`showFoodValuesHudOverlay = true`**：手持食物时显示将恢复的饥饿/饱和度覆盖层。
- **`showFoodValuesHudOverlayWhenOffhand = true`**：副手食物同样显示覆盖层。
- **`showFoodExhaustionHudUnderlay = true`**：在饥饿条后方显示消耗度进度条。
- **`showFoodStatsInDebugOverlay = true`**：在 F3 调试界面显示饥饿、饱和度与消耗度数值。
- **`showFoodHealthHudOverlay = true`**：在生命值条上显示食物预计回复的生命值。
- **`showVanillaAnimationsOverlay = true`**：覆盖层动画与原版图标动画同步。
- **`maxHudOverlayFlashAlpha = 0.65`**：覆盖层闪烁最大不透明度为 65%，既清晰可见又不刺眼。

该配置策略为"最大化食物信息透明度"，符合 Vanilla+ 生存包对信息辅助的期待。

## 兼容性与性能

- **客户端/服务端范围**：CLIENT ONLY。服务端无需安装。
- **存档影响**：无。AppleSkin 不修改存档数据或食物属性。
- **已知不兼容**：当前批次中无已知冲突。与某些自定义饥饿系统的模组可能显示不一致。
- **资源成本**：HUD 覆盖层渲染开销极低，对帧率影响可忽略。
- **缓解措施**：若玩家认为 HUD 过于拥挤，可关闭部分覆盖层（如 `showFoodExhaustionHudUnderlay`）。

## 验证

- [ ] 启动或加载测试
- [ ] 功能测试（食物提示、HUD 覆盖层、F3 信息）
- [ ] 多人游戏测试（如适用，CLIENT 端独立生效）
- [ ] UI 空间测试（与 Jade、Xaero's Minimap 等 HUD 模组共存）
- [ ] 相关系统回归检查

## 风险与开放问题

1. **HUD 拥挤度**：启用全部覆盖层后，饥饿条与生命值条区域信息量较大，可能与未来的状态栏模组重叠。
2. **与未来食物模组的数据一致性**：若未来添加修改饥饿/饱和度机制的内容模组，需验证 AppleSkin 的数值显示是否准确。

## 历史

- 2026-07-16: 作为第二批基础模组与 QoL 扩展批次添加
