# Complementary Unbound r5.8.1 + EuphoriaPatches 1.9.3（官方默认光影）

## 记录

- 类型：shader-pack
- 状态：active
- 证据可信度：verified
- 来源路径：`shaderpacks/ComplementaryUnbound_r5.8.1.zip`、`shaderpacks/ComplementaryUnbound_r5.8.1 + EuphoriaPatches_1.9.3/`（已打补丁衍生目录）、`shaderpacks/ComplementaryUnbound_r5.8.1 + EuphoriaPatches_1.9.3.txt`（光影设置文件）
- 标识与版本：Complementary Shaders Unbound 风格 r5.8.1 + EuphoriaPatches 1.9.3
- 加载器或包格式：Iris（OptiFine 光影包格式）

## 设计作用

整合包首次确立**官方默认光影**。选择 Complementary（Unbound 风格分支）+ EuphoriaPatches 扩展，原因：

- Complementary 是 1.21.1 生态中兼容性与性能口碑最好的光影之一（inferred，基于社区共识）。
- EuphoriaPatches 在 Complementary 基础上提供大量额外氛围选项，配合 Euphoria Patcher 模组可在游戏内直接维护补丁。
- 支撑"视觉风格统一"支柱：为截图、宣传、服务器统一观感提供基线。

## 玩家可见行为

- `config/iris.properties` 已预置 `shaderPack=ComplementaryUnbound_r5.8.1 + EuphoriaPatches_1.9.3`，首次启动即启用光影。
- 玩家可在 Iris 光影设置菜单中调整 EuphoriaPatches 扩展项（设置持久化于同名 `.txt`）。
- 低配玩家可切换回基础版 `ComplementaryUnbound_r5.8.1.zip` 或关闭光影。

## 集成关系

- **依赖**：Iris（加载器）、Euphoria Patcher（应用补丁，见其组件页）。
- **许可**：目录内含 `ComplementaryLicense.txt`、`EuphoriaPatchesLicense.txt`，随包分发需遵守两份许可（verified，文件存在）。

## 配置意图

- 衍生目录为 Euphoria Patcher 对基础 zip 打补丁后的产物；基础 zip 同时保留，便于补丁失败时回退或重新生成。
- `.txt` 设置文件随包分发，作为官方默认视觉参数基线。

## 兼容性与性能

- 纯客户端，无存档影响。
- 光影显著增加 GPU 负载；与"视听叠加风险"约束并列，需在中低端目标硬件上实测帧率。

## 验证

- [ ] 启动后 Iris 自动加载衍生光影
- [ ] 补丁设置项生效抽查
- [ ] 目标硬件帧率测试

## 风险与开放问题

1. 玩家修改 `.txt` 后整合包更新会覆盖，需文档告知或改为分发默认值。
2. 光影包体积较大，对分发带宽的影响待评估。

## 历史

- 2026-07-20: 设置 `.txt` 重新写入（仅时间戳变化，无参数 delta）
- 2026-07-19: 作为官方默认光影随第四批添加
