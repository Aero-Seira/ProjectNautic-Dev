# Progress Peek

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/progresspeek-1.0.2+1.21.1-neoforge.jar`
- 标识与版本：mod_id = `progresspeek`；版本 `1.0.2+1.21.1-neoforge`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Progress Peek 在**任务栏图标上显示游戏加载进度**（verified，mods.toml 描述）。整合包模组数已达约 90 个，启动耗时较长，玩家在切出窗口等待时可通过任务栏进度判断加载状态，属启动体验 QoL，归入 utility。

## 玩家可见行为

- 启动游戏时 Windows 任务栏图标显示进度条（Linux 亦有支持）。

## 集成关系

- **依赖**：NeoForge、Minecraft `1.21.1`（required）。
- **协作**：与 ModernFix（缩短启动时间）互补——一个减少等待，一个让等待可见。

## 配置意图

- **`config/progresspeek.toml`**：`enabled=true`，Windows 与 Linux 均启用；Linux 桌面文件相关项留空（目标平台为 Windows，保留默认）。

## 兼容性与性能

- 纯客户端，无存档影响；开销可忽略。

## 验证

- [ ] 启动测试（任务栏进度显示）

## 风险与开放问题

- 无显著风险。

## 历史

- 2026-07-20: 作为第五批性能与修复类更新添加
