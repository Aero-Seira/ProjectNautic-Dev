# Async Logger

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/asynclogger-2.1.2+1.21.1-neoforge.jar`
- 标识与版本：mod_id = `asynclogger`；版本 `2.1.2+1.21.1-neoforge`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Async Logger 将 log4j2 日志写入**异步化并提供高效过滤**（verified，mods.toml 描述），消除同步日志 I/O 对主线程的阻塞，同时可过滤噪音日志。对长线运营的服务器与客户端日志可维护性均有价值，归入 performance。

## 玩家可见行为

- 无直接可见 UI；日志文件保持原格式，模组数量多时的卡顿尖峰预期减少。

## 集成关系

- **依赖**：NeoForge、Minecraft `[1.21,1.21.8]`（required）。
- **协作**：与 spark（性能剖析时日志开销降低）、ModernFix（启动优化）互补。

## 配置意图

- **`config/asynclogger.toml`**：`enabled=true`，其余 log4j2 高级参数（ring buffer、wait strategy 等）全部留空使用模组默认——不做过早调优；`wrapSysOutSysErr=false` 不重定向标准输出（保守，避免干扰启动器日志捕获）。
- **`config/asynclogger/default.toml`**：空过滤规则文件（`levels/loggers/strings/regexes` 均为空），预留用于后续过滤已知的刷屏噪音日志。

## 兼容性与性能

- BOTH，服务端亦可安装；无存档影响。
- 异步日志在崩溃瞬间可能丢失末尾数条日志（inferred，异步 I/O 通病），排障时需知悉。

## 验证

- [ ] 启动测试
- [ ] 崩溃场景日志完整性抽查

## 风险与开放问题

1. 崩溃时日志尾部丢失风险与排障需求的权衡。

## 历史

- 2026-07-20: 作为第五批性能与修复类更新添加
