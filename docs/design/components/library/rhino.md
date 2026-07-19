# Rhino

## 记录

- 类型：library
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/[犀牛] rhino-2101.2.8-build.91.jar`
- 标识与版本：mod_id = `rhino`；版本 `2101.2.8-build.91`
- 加载器或包格式：扫描器报告为 fabric loader，但实际作为跨加载器库在 NeoForge 环境运行

## 设计作用

Rhino 是 **KubeJS 的硬依赖**，为 KubeJS 提供 JavaScript 运行时环境。它是整合包技术栈的最底层脚本引擎，对玩家完全不可见。

Rhino 的定位：
- **纯基础设施**：没有 Rhino，KubeJS 无法执行任何脚本，整合包的魔改体系将完全失效。
- **透明层**：玩家、管理员甚至大部分开发者都不需要直接与 Rhino 交互。所有 JavaScript 执行细节由 KubeJS 封装。

## 玩家可见行为

- **无**。Rhino 是纯粹的底层库，不暴露任何玩家可见的功能、UI 或游戏机制。

## 集成关系

### 依赖
- 无额外依赖。Rhino 是独立运行的 JavaScript 引擎。

### 被依赖
- **KubeJS**（required, AFTER, BOTH）—— KubeJS 是 Rhino 在整合包中的唯一消费者。

### 冲突
- 与其他 JavaScript 运行时库（如 Nashorn、GraalJS）理论上不能共存于同一 ClassLoader，但 Minecraft 模组环境中通常不会同时出现多个 JS 引擎。

### 加载顺序
- Rhino 必须在 KubeJS 之前完成加载。KubeJS 的 `AFTER` 依赖声明确保了这一点。

## 配置意图

Rhino 本身不暴露 Minecraft 级别的配置文件。其行为由以下因素决定：

- **版本锁定**：KubeJS `2101.7.2-build.368` 要求 Rhino `>=2101.2.7-build.81`。当前安装的 `2101.2.7-build.85` 满足此约束。Rhino 版本不能独立更新——必须首先确认 KubeJS 的兼容性声明。
- **加载器兼容性**：扫描器报告 Rhino 使用 fabric loader，但实际文件放置于 NeoForge 的 `mods` 目录且正常运行。这是跨加载器库的已知现象，无需干预。

## 兼容性与性能

### 客户端/服务端范围
- 标记为全平台（`*`）。Rhino 在客户端和服务端都需要存在，因为 KubeJS 脚本在两侧都会执行。

### 存档影响
- 无。Rhino 不读写存档数据。

### 已知不兼容
- Rhino 的 JavaScript 语法支持基于 ES5 并包含部分 ES6 特性，但不支持现代 JavaScript 的全部特性（如 async/await、class 字段初始化器）。KubeJS 脚本必须使用 Rhino 支持的语法子集。

### 资源成本
- Rhino 在启动时初始化 JavaScript 上下文，占用少量内存。脚本执行时的 CPU 开销取决于脚本复杂度，与 Rhino 本身关系不大。

## 验证

- [ ] 启动或加载测试（通过 KubeJS 启动成功即视为 Rhino 正常）
- [ ] 功能测试

## 风险与开放问题

1. **版本绑定**：Rhino 的更新与 KubeJS 紧密耦合。不能独立更新 Rhino 以获得新特性或修复。
2. **语法限制**：开发者可能习惯性地使用现代 JavaScript 语法（如 `??=`、`#privateField`），这些在 Rhino 中不支持。需要在开发规范中明确记录 Rhino 支持的语法范围。
3. **加载器标识异常**：扫描器报告 fabric loader 但不影响实际运行。若未来 NeoForge 版本更新后对此类跨加载器库的检查更严格，可能需要关注。

## 历史

- 2026-07-20: 升级 2101.2.7-build.85 → 2101.2.8-build.91（例行小版本升级，替换旧 jar；KubeJS 硬依赖，升级后需回归脚本系统）
- 2026-07-15: 作为基础 QoL 与魔改框架批次添加
