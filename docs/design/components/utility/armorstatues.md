# Armor Statues

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/ArmorStatues-v21.1.0-1.21.1-NeoForge.jar`
- 标识与版本：mod_id = `armorstatues`；版本 `21.1.0`；作者 Fuzs
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Armor Statues（Fuzs）解锁盔甲架的全部潜力：摆出任意姿势、精细调整、无底座等，且兼容纯原版服务器（verified，mods.toml 描述："Unlock the full potential of armor stands! Works on vanilla servers, too."）。服务于建筑/展示玩法，属创作向 QoL——不新增注册表内容，仅扩展原版盔甲架交互，符合"类原版起步"支柱。

## 玩家可见行为

- 对盔甲架打开姿势编辑界面，可逐关节调整或套用预设姿势。

## 集成关系

- **依赖**：Minecraft 1.21.1、NeoForge `[21.1.21,)`、PuzzlesLib `[21.1.8,)`、statuemenus（required）——`statuemenus` 未在 mods/ 中出现独立 jar，应为由 PuzzlesLib 提供或内嵌（verified 依赖声明；满足方式 inferred，启动测试需确认无缺失依赖报错）。

## 配置意图

`config/armorstatues-client.toml`（12 行）保持默认。

## 兼容性与性能

- BOTH 端（可连原版服务器表明核心逻辑客户端可用）；无存档影响（盔甲架数据走原版 NBT）；开销可忽略。

## 验证

- [ ] 启动或加载测试（重点确认 statuemenus 依赖被满足）
- [ ] 盔甲架姿势编辑功能测试

## 风险与开放问题

1. `statuemenus` 依赖的提供者待启动日志确认。

## 历史

- 2026-07-21: 作为第六批 Fuzs 系模组添加
