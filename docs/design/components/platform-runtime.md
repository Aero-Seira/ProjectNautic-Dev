# 运行平台与基础配置

## 记录

- 类型：运行环境与基础配置
- 状态：active
- 证据可信度：verified
- 来源路径：`ProjectNautic-Dev.json`、`config/fml.toml`、`config/neoforge-client.toml`、`config/neoforge-common.toml`
- 标识与版本：Minecraft 1.21.1、NeoForge 21.1.235、FML 4.0.42
- Java：21
- 启动目标：`forgeclient`

## 设计作用

该组件定义所有后续模组、脚本、配置和资源的兼容性基线。目前整合包没有已识别的玩法模组，因此它是唯一已激活的系统组件。

## 玩家可见行为

当前只提供 NeoForge 客户端的基础启动能力。没有证据表明已经加入额外玩法、界面、材质或世界内容。

## 集成关系

- 新增 JAR 必须兼容 Minecraft 1.21.1 和 NeoForge 21.1.x。
- `defaultConfigPath` 指向 `defaultconfigs`，未来需要预置服务端通用配置时应使用该目录。
- 当前 `mods`、`defaultconfigs` 和 `resourcepacks` 目录均未检测到内容。

## 配置意图

- `versionCheck = true`：保留 NeoForge 全局版本检查，便于开发阶段发现更新信息。
- `disableOptimizedDFU = true`：沿用当前客户端默认选择；加入大量数据转换或旧存档兼容需求后需重新评估。
- `showLoadWarnings = true`：开发阶段显示加载警告，便于发现兼容问题。
- `logUntranslatedConfigurationWarnings = true`：保留开发诊断信息。
- `logLegacyTagWarnings = "DEV_SHORT"`：以简短开发级别报告旧 `forge` 命名空间标签。
- `attributeAdvancedTooltipDebugInfo = true`：高级提示中显示属性调试信息，适合开发实例；发布版本可能需要关闭。
- 未配置依赖覆盖；除非有明确验证依据，不应使用 `dependencyOverrides` 绕过模组约束。

## 兼容性与性能

- `earlyWindowControl = true`，保留 NeoForge 对早期窗口和 GL 能力的控制。
- 早期初始化线程数为自动值 `-1`；出现启动期竞态或硬件兼容问题时再限制。
- 实验性 NeoForge 方块渲染管线未启用。
- 未启用合并的深度/模板附件。
- 当前没有足够内容建立实际内存、启动耗时或帧率基线。

## 验证

- [ ] 完成一次无报错客户端启动
- [ ] 核对主菜单显示的 Minecraft 与 NeoForge 版本
- [ ] 确认开发日志能够报告加载及翻译警告
- [ ] 添加首批模组后重新建立启动耗时和内存基线

## 风险与开放问题

- 当前尚未记录成功启动证据。
- 部分配置是开发诊断取向，发布构建前需要复核是否影响玩家界面或日志噪声。
- 整合包尚未定义是否支持服务端与多人游戏。

## 历史

- 2026-07-14：从启动配置与 NeoForge 默认配置建立初始运行平台记录。

