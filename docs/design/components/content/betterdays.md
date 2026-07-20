# BetterDays

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/betterdays-1.21.1-3.3.6.3-NEOFORGE.jar`
- 标识与版本：mod_id = `betterdays`；版本 `3.3.6.3`；作者 wendall911
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

BetterDays 提供时间流速控制（自定义昼夜长度）与睡眠机制改造（睡觉时时间加速流逝而非瞬间跳过）（verified，mods.toml 描述）。这是整合包首个**机制类**模组：改变时间规则而非内容。当前配置保持原版节奏（`daySpeed=nightSpeed=1.0`），实际落地的行为变化只有睡眠加速（`enableSleepFeature=true`）与多人睡眠投票（`ratioPlayersForSleep=0.0`，即任意比例玩家睡觉即可加速到早晨，inferred 语义需实测）。

## 玩家可见行为

- 睡觉时时间加速流过（单人 `sleepSpeedMax=110`，接近原版睡眠时长），早晨天气按原版转晴（`clearWeatherOnWake=true`）。
- 睡眠界面显示时钟（`displayBedClock=true`）；进入/离开床铺与早晨有聊天/动作栏提示消息（英文默认文本，待本地化）。
- 昼夜长短不变（RATIO 1.0）。

## 集成关系

- **依赖**：NeoForge `[4,)`、Minecraft `[1.21.1,1.21.2)`、`whitenoise [2.2.0,3.0.0)`（required）——whitenoise 2.2.0 已以 jarJar 形式内嵌于 betterdays jar 内（verified），`config/whitenoise-client.toml` 即该内嵌库生成。
- **冲突面**：睡眠/时间机制模组唯一，无重叠；与其他睡眠类模组未来互斥需注意。

## 配置意图

`config/betterdays-common.toml`：

- **昼夜速度 1.0**：有意维持原版时间节奏，仅引入睡眠加速——机制改动克制，符合类原版定位。
- **`ratioPlayersForSleep=0.0`**：睡眠加速不卡人数比例，单人睡觉即可推进——降低多人服务器"等人睡觉"的摩擦（inferred 设计意图）。
- **`weatherEffect="SLEEPING"`**、其余 time.effects 全 `NEVER`：仅睡觉时同步天气流逝，不干预随机刻/药水/饥饿/方块实体——规避性能与平衡风险。
- `config/betterdays-client.toml`（19 行）保持默认。

## 兼容性与性能

- BOTH 端；修改睡眠与时间机制，无存档破坏风险；time.effects 全关使性能影响可忽略。
- 提示消息为英文默认文本，需纳入本地化清单。

## 验证

- [ ] 启动或加载测试
- [ ] 单人/多人睡眠加速行为测试
- [ ] 与游戏规则 doDaylightCycle、其他时间模组的交互回归

## 风险与开放问题

1. 睡眠提示消息未本地化。
2. `ratioPlayersForSleep=0.0` 在多人下的实际语义（0 是否等于"任意比例"）需实测确认。

## 历史

- 2026-07-21: 作为第六批机制类模组添加（首个时间/睡眠机制改动）
