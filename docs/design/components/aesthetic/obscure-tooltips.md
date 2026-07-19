# Obscure Tooltips

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/obscure_tooltips-neoforge-1.21.1-4.2.2.jar`
- 标识与版本：mod_id = `obscure_tooltips`；版本 `4.2.2`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Obscure Tooltips 将物品提示框升级为**风格化、带平滑动画与特效的 tooltip**（verified，mods.toml 描述），并附带 3D 盔甲预览、超长提示滚动等实用增强。它与 ModernUI、Effect Descriptions 共同构成整合包的信息呈现视觉层，支撑"UI 一致性"支柱。

## 玩家可见行为

- 悬停物品时 tooltip 带阴影、动画与风格化边框；盔甲类物品显示 3D 上身预览。
- 超长 tooltip 自动换行并可用滚轮滚动，不再溢出屏幕。

## 集成关系

- **依赖**：Minecraft `[1.21.1,1.22)`、NeoForge `>=21.1.80`、**Fragmentum `>=2.1.0`**（required, BOTH，见 library/fragmentum.md）。
- **协作**：与 JEI（悬停信息源）、Effect Descriptions（tooltip 内效果说明）、ModernUI（字体渲染）叠加；配置中已忽略 `quality_equipment:reforge_gui_button`（为尚未安装的 Quality Equipment 预留的排除项，inferred）。

## 配置意图

- **`config/obscuria/obscure_tooltips-client.toml`**：整体启用，标签第二行与阴影保留默认；`AutoWrapping` 自动换行启用；`Scrolling` 超长滚动启用（`scrollSpeed=5`）；`ArmorPreview` 3D 盔甲预览已于 2026-07-20 **关闭**（减少 tooltip 渲染开销与视觉噪音）——其余为默认值的确认保留，后续按玩家反馈收敛。

## 兼容性与性能

- 客户端视觉层，无存档影响。
- tooltip 动画与 3D 预览有少量渲染开销，纳入视听叠加低配验证。

## 验证

- [ ] 启动测试
- [ ] 功能测试（tooltip 动画、盔甲预览、超长提示滚动）
- [ ] 与 JEI / ModernUI / Effect Descriptions 的 tooltip 叠加回归

## 风险与开放问题

1. 多个 tooltip 增强模组（Effect Descriptions 等）叠加后的渲染层级需实测。

## 历史

- 2026-07-20: `armorPreviewEnabled` true → false（关闭 3D 盔甲预览，减少 tooltip 渲染开销/视觉噪音）
- 2026-07-19: 作为第四批视觉与协作体验批次添加
