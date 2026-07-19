# Gnetum

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/gnetum-3.3.6.jar`
- 标识与版本：mod_id = `gnetum`；版本 `3.3.6`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Gnetum 将 **HUD 更新分摊到多个帧**以改善性能（verified，mods.toml 描述："distributes HUD updates over multiple frames to improve performance"）。整合包 HUD 元素众多（Xaero 小地图、AppleSkin、EffectTimerPlus、Jade 等），HUD 渲染是帧时间的重要组成，Gnetum 是对 ImmediatelyFast 在 HUD 维度上的直接补充，归入 performance。

## 玩家可见行为

- 无直接可见 UI；预期表现为 HUD 密集场景（多人、多效果图标）下帧率更平稳。

## 集成关系

- **依赖**：NeoForge `>=21.1.0`、Minecraft（required）；`displayTest=MATCH_VERSION`，联机时客户端与服务端版本需一致（若服务端安装）。
- **协作**：与 ImmediatelyFast（HUD 批处理）、Xaero 地图、AppleSkin、EffectTimerPlus 等 HUD 模组叠加；叠加正确性需回归。

## 配置意图

- 本批次未产生独立配置文件（或尚未生成，unknown），使用默认行为。

## 兼容性与性能

- 主要作用于客户端 HUD 渲染；无存档影响。

## 验证

- [ ] 启动测试
- [ ] HUD 密集场景帧时间对比
- [ ] 与 Xaero / AppleSkin / Jade / EffectTimerPlus 的 HUD 显示回归

## 风险与开放问题

1. HUD 更新分帧可能引入轻微显示延迟（如饥饿值、效果计时跳变），需主观验证可接受度。

## 历史

- 2026-07-20: 作为第五批性能与修复类更新添加
