# Prism

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/Prism-1.21.1-neoforge-1.0.11.jar`
- 标识与版本：mod_id = `prism`；版本 `1.0.11`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Prism 是 Grend（Anthony Hilyard）出品的**颜色功能库**（verified，mods.toml 描述："A library all about color!"），为依赖它的模组提供颜色处理 API。玩家不可见，归入 library。

当前包内尚无显式声明依赖 Prism 的模组（inferred：可能为 Obscure Tooltips 或后续 Grend 系模组的前置/软依赖引入）；保留以支持未来 Grend 系 UI 模组（LegendaryTooltips、Item Borders 等）的加入。

## 玩家可见行为

- 无直接玩家可见行为。

## 集成关系

- **依赖**：NeoForge `>=21.1.60`、Minecraft `1.21.1`（required, BOTH）。
- **被依赖**：当前无包内声明方（unknown，待确认引入动机）。

## 配置意图

- 无独立配置文件。

## 兼容性与性能

- BOTH；纯库，开销可忽略，无存档影响。

## 验证

- [ ] 启动测试（作为库正常加载）

## 风险与开放问题

1. 引入动机未明确（哪个模组实际需要 Prism）——若是误装可考虑移除以减少模组数。

## 历史

- 2026-07-19: 作为第四批视觉与协作体验批次添加
