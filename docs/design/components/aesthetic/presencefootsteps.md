# Presence Footsteps [FORGE]

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/[脚步声Forge版] Presence Footsteps [NEOFORGE]+1.21-1.1.0.jar`
- 标识与版本：mod_id = `presencefootsteps`；版本 `1.0.0`（非官方 NeoForge 移植，作者 1Influence）
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Presence Footsteps 为行走/奔跑提供**材质感知的程序化脚步声**（非官方 NeoForge 移植版，verified，mods.toml 描述）。它是环境声景层（AmbientSounds、Extra Sounds）的地面交互补充，强化沉浸感这一视听支柱。

## 玩家可见行为

- 不同方块材质产生不同脚步声；赤足/穿鞋、湿润、奔跑均有差异音效。
- 多人下可听到其他玩家与生物的脚步声（`multiplayer=true`、`targetEntities=ALL`）。

## 集成关系

- **依赖**：NeoForge `>=21`、Minecraft `>=1.21`（required, BOTH）。
- **协作**：与 AmbientSounds（环境声）、Extra Sounds（UI 音）分层叠加，不冲突；与光影/ETF 无交互。

## 配置意图

- **`config/presencefootsteps/userconfig.json`**：总音量 70（低于默认满值，避免脚步声盖过环境声景——视听叠加降噪）；`wetSoundsVolume=50` 湿润音效减半；其余实体类别音量保留默认。
- **`config/presencefootsteps/updater.json`**：`enabled=false` 关闭内置更新检查（统一由整合包版本管理，避免玩家侧更新提示）。

## 兼容性与性能

- 纯客户端表现，服务端无需安装；无存档影响。
- 非官方移植版，稳定性需实测（版本号 1.0.0 与 jar 文件名 1.1.0 不一致，以 mods.toml 为准）。

## 验证

- [ ] 启动测试
- [ ] 功能测试（多材质地面脚步声、多人脚步可闻）
- [ ] 与 AmbientSounds 叠加的主观听感验证

## 风险与开放问题

1. 非官方移植的长期维护性存疑，升级 NeoForge 版本时需优先验证。
2. 脚步声与其他音效模组叠加音量平衡需玩家反馈迭代。

## 历史

- 2026-07-19: 作为第四批视觉与协作体验批次添加
