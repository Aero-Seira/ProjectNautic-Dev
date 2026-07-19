# Euphoria Patcher

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/EuphoriaPatcher-1.9.3-r5.8.1-neoforge.jar`
- 标识与版本：mod_id = `euphoria_patcher`；版本 `1.9.3-r5.8.1-neoforge`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Euphoria Patcher 在游戏内为 Complementary Shaders 应用 Euphoria Patches 扩展补丁（verified，来自 jar 内 mods.toml 描述）。它是整合包**视觉风格统一**的关键组件：配合随包分发的 `ComplementaryUnbound_r5.8.1 + EuphoriaPatches_1.9.3` 光影，为整合包确立官方默认视觉基调，同时保留玩家在光影设置菜单中微调的自由度。

## 玩家可见行为

- 玩家无需手动打补丁，启用 Iris 中的 EuphoriaPatches 衍生光影即可获得扩展效果（额外设置项、发光/氛围增强等，具体项以光影内设置为准）。
- 游戏内可能出现光影自身的更新/提示消息（`doDisplayShaderInGameMessage=true`）。

## 集成关系

- **依赖**：NeoForge `>=21.0`、Minecraft `>=1.21.0`（required, BOTH）；实际功能依赖 **Iris** 加载光影（inferred，模组描述提及 Iris/Oculus）。
- **配套**：`shaderpacks/ComplementaryUnbound_r5.8.1.zip`（基础光影）与 `shaderpacks/ComplementaryUnbound_r5.8.1 + EuphoriaPatches_1.9.3`（补丁衍生版）；`config/iris.properties` 已将 `shaderPack` 指向衍生版。
- **冲突**：与 Oculus 等其它光影加载器二选一（inferred）。

## 配置意图

- **`config/euphoria_patcher/settings.toml`**：更新检查设为 `important`（仅提示重要更新，避免打扰）；`doRenameOldShaderFiles=true` 自动重命名过期补丁目录便于识别，`doDeleteOldShaderFiles=false` 不自动删除，保守处理玩家文件。
- **`config/euphoria_patcher/.data.json`**：运行时记录（`shaderHash`、`EPVersion=1.9.3`、`styleUnbound=true`），反映当前补丁对应 Unbound 风格分支；属生成数据，非设计决策。

## 兼容性与性能

- 标记 BOTH，但功能实际作用于客户端渲染；服务端安装无害（displayTest=IGNORE_ALL_VERSION）。
- 性能开销取决于光影本身；低配设备可切换回无光影状态，不影响存档。

## 验证

- [ ] 启动测试（Iris 正确识别衍生光影）
- [ ] 功能测试（Euphoria Patches 设置项在光影菜单可见并生效）
- [ ] 低配设备帧率验证

## 风险与开放问题

1. 光影更新后 `shaderHash` 变化，补丁是否自动重新应用需实测。
2. 光影设置文件（`.txt`）随包分发，玩家本地修改会与更新冲突，需明确升级策略。

## 历史

- 2026-07-19: 作为第四批视觉与协作体验批次添加，配套引入 Complementary Unbound r5.8.1 + EuphoriaPatches 1.9.3 官方默认光影
