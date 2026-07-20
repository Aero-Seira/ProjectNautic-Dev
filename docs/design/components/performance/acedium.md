# Acedium Sodiumized

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/acedium-0.4.1+mc1.21.1.jar`
- 标识与版本：mod_id = `acedium`；版本 `0.4.1+mc1.21.1`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Acedium Sodiumized 是 Acedium（Nvidium 的 NeoForge 移植）的非官方延续，为 Sodium 0.8 / NeoForge 1.21.1 更新（verified，mods.toml 描述）。Nvidium 利用 NVIDIA GPU 的 mesh shader 管线大幅加速地形渲染，是 Sodium 之上的进阶渲染加速层，归入 performance。其收益依赖特定硬件（NVIDIA 16 系以上，inferred），对非 NVIDIA 玩家无作用。

## 玩家可见行为

- 无直接可见 UI；NVIDIA 显卡玩家预期在远视距下获得明显帧率提升；其余玩家无感知。

## 集成关系

- **依赖**：NeoForge `[21.1,)`、Minecraft `[1.21.1,1.22)`（required）；Sodium `[0.8.12-beta.1,0.9)`（required, CLIENT）——与本包 Sodium 0.8.12 精确匹配。
- **配置**：生成 `config/nvidium-config.json`（沿用 Nvidium 配置命名，默认 `automatic_memory_limit=true`、`max_temp_memory`=512MB 等）。mods.toml 注册 `sodium:config_api_user`，设置入口挂在 Sodium 视频设置内。
- **协作**：与 Sodium 硬绑定；与 Iris 光影同开时 Nvidium 类模组通常自动让位（inferred，需实机验证）。

## 配置意图

`config/nvidium-config.json` 全默认：自动显存限制、区域保持距离等未自定义。设计意图是先以默认行为验证硬件收益，再决定是否按目标机型调参。

## 兼容性与性能

- 仅 CLIENT；无存档影响。
- 已知约束：仅 NVIDIA 显卡受益（mesh shader 依赖，inferred）；非 NVIDIA 环境应确认不会崩溃或退化。

## 验证

- [ ] NVIDIA 环境启动 + 远视距帧率对比
- [ ] 非 NVIDIA（AMD/核显）环境启动回归
- [ ] 与 Iris 官方默认光影同开的兼容性

## 风险与开放问题

1. 非官方延续版本（0.4.1），维护稳定性与上游跟进速度待观察。
2. 若目标玩家硬件谱系中 NVIDIA 占比低，该模组收益面有限，可考虑标注为可选模组。

## 历史

- 2026-07-21: 作为第六批 HUD/性能/机制模组添加（渲染加速进阶层）
