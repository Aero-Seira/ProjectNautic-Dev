# Distinguished Potions

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/DistinguishedPotions-v21.1.1-1.21.1-NeoForge.jar`
- 标识与版本：mod_id = `distinguishedpotions`；版本 `21.1.1`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Distinguished Potions（Fuzs）让药水更容易区分（verified，mods.toml 描述："Trouble telling your potions apart? Time to give you a helping hand with that."）——通过瓶塞/瓶身配色与提示强化区分不同效果的药水（具体形式 inferred，以实机为准）。服务于物品可读性的视觉层。

## 玩家可见行为

- 不同效果的药水在物品栏/手持时视觉可区分度提升（inferred）。

## 集成关系

- **依赖**：Minecraft 1.21.1、NeoForge `[21.1.21,)`、PuzzlesLib `[21.1.0,)`（required）。
- **协作**：与 Obscure Tooltips（提示框）、Effect Descriptions（效果说明）、JEED/MEED（JEI 效果描述）共同构成药水/效果信息层。

## 配置意图

`config/distinguishedpotions-client.toml`（17 行）与 `distinguishedpotions-common.toml`（7 行）保持默认，未做包级自定义。

## 兼容性与性能

- BOTH 端（客户端视觉为主）；无存档影响；开销可忽略。

## 验证

- [ ] 启动或加载测试
- [ ] 药水视觉区分实机确认
- [ ] 与 Obscure Tooltips 提示框叠加回归

## 风险与开放问题

- 无已知重大风险。

## 历史

- 2026-07-21: 作为第六批 Fuzs 系模组添加
