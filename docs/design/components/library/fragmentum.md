# Fragmentum

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/fragmentum-neoforge-1.21.1-2.2.4.jar`
- 标识与版本：mod_id = `fragmentum`；版本 `2.2.4`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Fragmentum 是 **Obscuria 系列模组的轻量级跨平台框架**（verified，mods.toml 描述），玩家不可见，归入 library。当前包内唯一下游是 [Obscure Tooltips](../aesthetic/obscure-tooltips.md)。

## 玩家可见行为

- 无直接玩家可见行为。

## 集成关系

- **依赖**：Minecraft `[1.21.1,1.22)`、NeoForge `>=21.1.80`（required, BOTH）。
- **被依赖**：`obscure_tooltips` 要求 `fragmentum >=2.1.0`（required, BOTH）——升级 Fragmentum 时须同步验证 Obscure Tooltips。

## 配置意图

- 无独立配置文件。

## 兼容性与性能

- BOTH；纯库，开销可忽略，无存档影响。

## 验证

- [ ] 启动测试（与 Obscure Tooltips 共同加载）

## 风险与开放问题

1. 未来若引入更多 Obscuria 系列模组（Obscure API 生态），统一在此页登记依赖链。

## 历史

- 2026-07-19: 作为第四批视觉与协作体验批次添加（Obscure Tooltips 前置）
