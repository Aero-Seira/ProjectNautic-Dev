# Iceberg

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/[冰山] Iceberg-1.21.1-neoforge-1.3.2.jar`
- 标识与版本：mod_id = `iceberg`；版本 `1.3.2`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Iceberg 是 Grend（Anthony Hilyard）出品的**模组开发工具库**（verified，mods.toml 描述：事件、助手与实用工具集合），玩家不可见，归入 library。其 mods.toml 为 Advancement Plaques、Legendary Tooltips、Equipment Compare、Merchant Markers、Item Borders 声明了 optional 依赖——这些均为 Grend 系 UI 增强模组，Iceberg 的加入为该系列预留了扩展空间。

## 玩家可见行为

- 无直接玩家可见行为。

## 集成关系

- **依赖**：NeoForge `>=21.1.54`、Minecraft `1.21.1`（required, BOTH）。
- **可选下游**（当前均未安装）：advancementplaques、legendarytooltips、equipmentcompare、merchantmarkers、itemborders。
- **同作者库**：与 [Prism](prism.md)（颜色库）同属 Grend 系基础设施。

## 配置意图

- 无独立配置文件。

## 兼容性与性能

- BOTH；纯库，开销可忽略，无存档影响。

## 验证

- [ ] 启动测试

## 风险与开放问题

1. 与 Prism 同样存在"前置已装、主体未装"的空窗状态，若后续不引入 Grend 系模组可一并移除。

## 历史

- 2026-07-19: 作为第四批视觉与协作体验批次添加
