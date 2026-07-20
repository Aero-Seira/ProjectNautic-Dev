# Krypton FNP

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/krypton_fnp-neoforge-1.21.1-0.2.28.1-1.21.1.jar`
- 标识与版本：mod_id = `krypton_fnp`；版本 `0.2.28.1-1.21.1`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Krypton FNP 是 Krypton 的 NeoForge 延续分支，优化 Minecraft 网络栈：Netty 管道调优、数据包 flush 合并、原生传输（embedded velocity-native）等（verified，mods.toml 描述："Provides powerful network optimization capabilities for all mainstream systems"）。整合包以长线服务器运营为首要支柱，网络栈优化直接降低联机延迟与 CPU 占用，归入 performance。

## 玩家可见行为

- 无 UI；预期表现为联机场景下延迟更稳定、高实体/区块同步时掉帧减少。

## 集成关系

- **依赖**：NeoForge `[21,)`、Minecraft `[1.21.1,1.21.2]`（required, BOTH）。
- **嵌入库**：jarJar 携带 sewlia-config 与 velocity-native（原生网络传输）。
- **协作**：与 PacketFixer（数据包修复）同属网络维度但职责不同（优化 vs 容错），可并存。

## 配置意图

`config/krypton_fnp.yaml`（27 行）保持默认：未见包级自定义（全默认，一句带过）。设计意图是先接受作者预设的网络调优参数。

## 兼容性与性能

- BOTH 端；无存档影响；纯网络层优化，可随时移除。
- 原生传输在非常规系统上若加载失败应有 JNI 回退（inferred）。

## 验证

- [ ] 启动或加载测试
- [ ] 联机延迟与高负载同步场景对比
- [ ] 与服务端部署的兼容性核对

## 风险与开放问题

1. 分支版本（FNP 非 Krypton 官方）维护活跃度待观察。

## 历史

- 2026-07-21: 作为第六批 HUD/性能/机制模组添加（性能矩阵补齐网络栈维度）
