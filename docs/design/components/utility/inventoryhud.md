# Inventory HUD+（物品栏 HUD+）

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/[物品栏HUD+] inventoryhud.neoforged.1.21.1-3.4.28.jar`
- 标识与版本：mod_id = `inventoryhud`；版本 `3.4.28`；作者 DmitryLovin
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Inventory HUD+ 在 HUD 上常驻显示三类信息：迷你物品栏、护甲/主副手耐久、药水效果（verified，配置分 `[inventoryhud]`/`[armorhud]`/`[potionshud]` 三组）。让玩家不打开物品栏即可掌握装备耐久与背包状态，属信息展示型 QoL。默认迷你物品栏关闭（`byDefault=false`），护甲与药水 HUD 开启。

## 玩家可见行为

- 屏幕底部常驻护甲件与主副手物品及耐久百分比（`armView="PERCENTAGE"`）。
- 药水效果 HUD（含进度条 `barDuration=300`，含隐藏效果 `showHiddenEffects=true`）。
- 迷你物品栏默认关闭，可按需开启。

## 集成关系

- **依赖**：NeoForge、Minecraft 1.21.1（required）。
- **重叠**：`potionshud` 与 Stylish Effects 的 HUD 效果挂件功能重叠——两者同时显示效果图标，需决策保留一方（开放问题）。
- **协作**：与 AppleSkin、Overflowing Bars、Leave My Bars Alone 同处 HUD 层，布局叠加需回归。

## 配置意图

`config/inventoryhud-client.toml`（249 行）保持默认：护甲百分比常驻 + 药水 HUD 开启 + 迷你物品栏默认关。设计意图是提供装备耐久常驻可见性，同时不强制占用屏幕空间的迷你背包。

## 兼容性与性能

- 仅客户端渲染；无存档影响；开销低。

## 验证

- [ ] 启动或加载测试
- [ ] 与 Stylish Effects 的药水 HUD 双重渲染检查（二选一）
- [ ] 各分辨率下 HUD 布局回归

## 风险与开放问题

1. 与 Stylish Effects 药水 HUD 重叠，需裁决。

## 历史

- 2026-07-21: 作为第六批 HUD 体系模组添加
