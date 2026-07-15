# Cupboard

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/cupboard-1.21.1-3.8.jar`
- 标识与版本：`cupboard` 3.8
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Cupboard 是一个底层工具库，提供崩溃修复、诊断日志与配置辅助。它主要作用是 silently 修复其他模组在非主线程添加实体导致的崩溃，并通过可选日志与堆转储帮助排查内存问题。作为“基础设施”类库，它不直接提升帧率，但支撑整合包的稳定性与可维护性，间接服务于性能支柱。

## 玩家可见行为

Cupboard 没有独立玩家交互界面。普通玩家几乎感受不到它的存在，仅在配置文件热重载时可能看到一条聊天提示；若其他模组违反线程规则添加实体，它会在后台修复并记录日志。OOM 时是否自动生成堆转储默认关闭，以避免占用大量磁盘空间。

## 集成关系

- **依赖**：Minecraft [1.21, 1.30)（BOTH）
- **冲突**：`debugChunkloadAttempts` 选项与 Lithium 及其分支不兼容；当前配置已关闭该选项
- **协作**：作为 ClickAdv（可点击的进度）等模组的依赖库提供底层工具；与 Lithium 共存时需注意关闭 chunkload 调试
- **配方/标签**：无直接关联
- **任务**：无
- **世界生成**：不干预世界生成
- **UI**：无 UI 注入
- **资源**：无额外资源需求
- **脚本**：无 KubeJS 脚本交互
- **加载顺序假设**：标准加载顺序即可

## 配置意图

`config/cupboard.json` 以“默认启用安全修复、诊断保持轻量”为原则：

- `showCommandExecutionErrors: true` — 在命令执行出错时显示错误信息，便于调试。
- `debugChunkloadAttempts: false` — 关闭强制加载 chunk 的调试日志（与 Lithium 不兼容，必须保持关闭）。
- `skipErrorOnEntityLoad: false` — 不在实体加载错误时静默跳过，保持默认崩溃行为以便暴露真实问题。
- `logOffthreadEntityAdd: true` — 记录非主线程添加实体的违规行为，同时由 Cupboard 自动修复崩溃。
- `forceHeapDumpOnOOM: false` — 不在内存溢出时自动生成堆转储，避免磁盘被大文件占满；仅在主动排查时临时开启。
- `displayConfigReload: true` — 客户端配置文件热重载后在聊天显示提示。

## 兼容性与性能

- **客户端/服务端范围**：BOTH
- **存档影响**：无。不修改存档格式或游戏数据
- **已知不兼容**：Lithium（仅在 `debugChunkloadAttempts` 开启时冲突；当前已关闭）
- **资源成本**：修复逻辑带来的运行时开销极低；`forceHeapDumpOnOOM` 关闭时无额外磁盘占用
- **缓解措施**：保持 `debugChunkloadAttempts: false` 以兼容 Lithium；`logOffthreadEntityAdd: true` 用于在问题发生时提供日志证据

## 验证

- [ ] 启动或加载测试
- [ ] 功能测试
- [ ] 多人游戏测试（如适用）
- [ ] 性能或世界生成测试（如适用）
- [ ] 相关系统的回归检查

## 风险与开放问题

1. **静默修复可能掩盖模组 Bug**：`logOffthreadEntityAdd` 虽记录但自动修复，可能让开发者错过需要上游修复的线程安全问题。
2. **诊断选项的误开启风险**：`debugChunkloadAttempts` 与 `forceHeapDumpOnOOM` 若被玩家手动开启，可能分别导致与 Lithium 冲突或产生超大堆转储文件。

## 历史

- 2026-07-16: 作为第三批性能模组添加
