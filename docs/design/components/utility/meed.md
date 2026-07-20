# Moderately Enough Effect Descriptions（MEED）

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/[Moderately Enough Effect Descriptions] meed-1.21.1-8.0.1.jar`
- 标识与版本：mod_id = `meed`；版本 `8.0.1`；作者 Neon_Cranberries
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

MEED 为 JEI 的效果查看页提供来自 100+ 模组的 500+ 条效果描述文本（verified，mods.toml 描述："Adds over 500 effect descriptions from 100+ mods to Minecraft!"）。**与包内已有 Just Enough Effects Descriptions（JEED）功能重叠**：JEED 是 JEI 效果描述的框架模组（提供效果页与描述机制），MEED 则是描述数据集的补充实现。二者并存时效果描述来源需实测确认去重/覆盖行为；若显示重复描述，应二选一（开放问题）。

## 玩家可见行为

- 在 JEI 中查看状态效果时显示描述文本（覆盖大量模组效果）。

## 集成关系

- **依赖**：NeoForge `[4,)`、Minecraft `[1.21,1.22)`（required, BOTH）。注意其 mods.toml 内部依赖块命名为 `jeed`（疑似打包残留），不声明对 JEED 模组的硬依赖。
- **重叠**：与 [JEED](jeed.md)（JEI 效果描述框架）、[Effect Descriptions](effectdescriptions.md)（物品 tooltip 效果说明）同处效果信息层；前二者功能重叠，后者互补。

## 配置意图

- 未产生独立配置文件（纯数据/描述模组），使用默认行为。

## 兼容性与性能

- BOTH 端；无存档影响；开销可忽略。

## 验证

- [ ] 启动或加载测试
- [ ] 与 JEED 并存时效果描述是否重复/冲突的实机检查（决定去留）

## 风险与开放问题

1. 与 JEED 功能重叠，若描述重复显示需移除其一；当前倾向保留覆盖更广者，待实机对比。

## 历史

- 2026-07-21: 作为第六批模组添加；与 JEED 的重叠关系待实机裁决
