# PacketFixer

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/packetfixer-3.3.1-1.20.5-1.21.X-merged.jar`
- 标识与版本：mod_id = `packetfixer`；版本 `3.3.1`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

PacketFixer 修复各类数据包问题：提高 NBT/数据包大小限制、修复因超大 packet 导致的踢出与连接失败（verified，mods.toml 描述："A simple mod to solve various problems with packets"）。模组数破百后 NBT 负载（物品、区块实体、登录同步包）增大，本模组是联机稳定性的容错保险，归入 performance（稳定性维度）。与 Krypton FNP 职责互补：一个优化网络栈，一个兜底数据包超限。

## 玩家可见行为

- 无 UI；价值体现在"不再出现"的断连/踢出（如 DecoderException、badly compressed packet）。

## 集成关系

- **依赖**：NeoForge、Minecraft `[1.20.5,1.22)`（required, BOTH）。
- **协作**：与 Krypton FNP 并存；两者均 hook 网络层，叠加需联机回归。

## 配置意图

`config/packetfixer.properties`（15 行）保持默认，各大小限制未自定义。设计意图是接受作者预设的宽松上限，遇具体断连案例再针对性收紧/放宽。

## 兼容性与性能

- BOTH 端；无存档影响；开销可忽略。

## 验证

- [ ] 启动或加载测试
- [ ] 联机登录与大数据量场景（满背包、复杂区块）回归
- [ ] 与 Krypton FNP 叠加的联机回归

## 风险与开放问题

1. 放宽数据包上限可能掩盖真正的异常包问题，排障时需知悉。

## 历史

- 2026-07-21: 作为第六批 HUD/性能/机制模组添加（联机稳定性兜底）
