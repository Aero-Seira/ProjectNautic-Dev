# Stylish Effects

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/StylishEffects-v21.1.3-1.21.1-NeoForge.jar`
- 标识与版本：mod_id = `stylisheffects`；版本 `21.1.3`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Stylish Effects（Fuzs）重做状态效果显示：更紧凑的效果挂件，并可在任意菜单中显示效果（verified，mods.toml 描述："Status effect display overhaul: Display them in any menu! And way more compact."）。它接管原版与物品栏内的效果图标渲染，提供倒计时、放大等级标注等，是被移除的 EffectTimerPlus 的功能上位替代（计时、等级、紧凑显示全部覆盖且可与 JEI 体系共存）。

## 玩家可见行为

- 物品栏左侧以紧凑挂件列出当前效果（`screen_side="LEFT"`、`renderer_type="GUI_COMPACT"`），悬停显示完整 tooltip 与持续时间。
- HUD 右侧（`gui_renderer`）以紧凑挂件显示效果，alpha 0.85。
- 效果剩余时间闪烁提示（`blinking_alpha=true`）、环境效果边框区分。

## 集成关系

- **依赖**：Minecraft 1.21.1、NeoForge `[21.1.21,)`、PuzzlesLib `[21.1.9,)`（required）。
- **替代关系**：替代 EffectTimerPlus（2026-07-21 移除）；与 Effect Descriptions（tooltip 效果说明）、JEED/MEED（JEI 效果描述）职责不同，可并存。
- **协作**：与 Inventory HUD+ 的 potionshud 模块功能重叠（均显示 HUD 效果图标）——需在两者间选定一个 HUD 效果来源，避免双重渲染（开放问题）。

## 配置意图

`config/stylisheffects-client.toml`（250 行）保持默认：物品栏左侧紧凑挂件 + HUD 右侧紧凑挂件、倒计时开启、`menu_blacklist` 预置 curios/tconstruct/mekanism（本包未装，冗余无影响）。未做包级自定义。

## 兼容性与性能

- 仅客户端渲染；无存档影响；开销极低。

## 验证

- [ ] 启动或加载测试
- [ ] 与 Inventory HUD+ potionshud 的双重渲染检查（二选一）
- [ ] 与 ModernUI/Obscure Tooltips 的 tooltip 回归

## 风险与开放问题

1. 与 Inventory HUD+ 药水 HUD 重叠，需决策保留哪一方。

## 历史

- 2026-07-21: 作为第六批模组添加，替代 EffectTimerPlus
