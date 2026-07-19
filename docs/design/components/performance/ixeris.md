# Ixeris

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/Ixeris-4.5.2+1.21.1-neoforge.jar`
- 标识与版本：mod_id = `ixeris_dummy`（jar 内 mods.toml 声明，属展示用元数据）；版本 `4.5.2+1.21.1-neoforge`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）；`clientSideOnly=true`

## 设计作用

Ixeris 提供**缓冲原始输入与线程化事件轮询**（verified，mods.toml 描述："Buffered raw input and threaded event polling"），将 GLFW 输入事件处理移出主渲染线程，降低输入处理对帧率的影响。它是对现有性能矩阵（渲染/逻辑/内存）在**输入管线**维度上的补充，归入 performance。

## 玩家可见行为

- 无直接可见 UI；预期表现为高负载场景下鼠标/键盘输入更跟手、帧时间更平稳。

## 集成关系

- **依赖**：NeoForge、Minecraft `[1.21,1.21.1]`（required）。
- **冲突**：无声明；但输入管线属于敏感底层，与任何修改输入处理的模组叠加时需回归（unknown，目前包内无同类）。

## 配置意图

- **`config/ixeris.toml`**：`flexibleThreading=true`（更灵活的线程模型，在遵守 OS 线程要求的前提下提升性能）；`aggressiveCaching=false`（实验性 GLFW 状态缓存暂不启用，保守优先）；`fullyBlockingMode=false`（允许延迟 GLFW 调用以换取性能）；macOS 平台禁用（与目标平台无关，保留默认）。

## 兼容性与性能

- 纯客户端（clientSideOnly），服务端无需安装；无存档影响。

## 验证

- [ ] 启动测试
- [ ] 高负载场景输入响应主观验证
- [ ] 与 Shoulder Surfing（相机输入）、NewVisualKeybing（键位）回归

## 风险与开放问题

1. 线程化输入与第三人称视角模组的交互需实测。

## 历史

- 2026-07-20: 作为第五批性能与修复类更新添加
