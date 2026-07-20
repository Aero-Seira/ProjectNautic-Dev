# Configured（配置界面）

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/[配置界面] configured-neoforge-1.21.1-2.6.3.jar`
- 标识与版本：mod_id = `configured`；版本 `2.6.3`；作者 MrCrayfish
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Configured 为每个模组生成简单的游戏内配置 GUI（verified，mods.toml 描述："Creates a simple GUI config for every mod!"）。整合包模组破百且大量 Fuzs 系/性能模组仅有 toml 配置，游戏内统一配置入口显著降低玩家与运维的调整成本，属 QoL 基础设施。

## 玩家可见行为

- 模组列表中每个模组出现配置按钮，打开统一风格的设置界面。

## 集成关系

- **依赖**：NeoForge `[21.1.211,)`、Minecraft `[1.21.1,)`（required）；Framework `[0.13.5,)` 与 JEI `[19.25.0.322,)` 为 optional（本包有 JEI，JEI 配置提供器会激活）。
- **协作**：与 Cloth Config、YACL、Fzzy Config 等配置库共存（各库自带界面的模组不受其管）。

## 配置意图

`config/configured-client.properties`（22 行）保持默认，未做包级自定义。

## 兼容性与性能

- 仅客户端 UI；无存档影响；开销可忽略。
- 注意：`config/catalogue.properties`、`config/catalogue_favourites.txt` 是 MrCrayfish 另一模组 Catalogue（模组菜单）的配置，本包未安装 Catalogue jar——属孤儿配置，来源待确认（可能为试用残留），已在变更记录中标注。

## 验证

- [ ] 启动或加载测试
- [ ] 主要模组配置界面打开回归

## 风险与开放问题

1. Catalogue 孤儿配置的来源与清理待确认。

## 历史

- 2026-07-21: 作为第六批模组添加
