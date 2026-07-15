# Fzzy Config

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/fzzy_config-0.7.6+1.21+neoforge.jar`
- 标识与版本：mod_id = `fzzy_config`；版本 `0.7.6+1.21+neoforge`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Fzzy Config 是配置库模组，提供声明式配置定义、配置 GUI、服务端/客户端同步以及内置的 GUI 快捷键体系。它在包内属于 UI 与配置基础设施层：依赖它的模组可以用注解或代码描述配置项，Fzzy Config 负责渲染界面、处理输入与网络同步。

在 ProjectNautic 中，它支撑**UI 一致性**与**可配置性**支柱，使复杂配置能够以统一的键盘导航、复制粘贴与搜索体验呈现给玩家。

## 玩家可见行为

普通玩家不会直接看到 Fzzy Config 的名称。其功能通过依赖它的模组间接体现，主要体现在配置界面的快捷键、复制粘贴与搜索行为上。

## 集成关系

- **依赖**：NeoForge `[21.0.0-beta,)`、Minecraft `[1.21,)`、Kotlin for Forge `[5.3.0,)`（required, BOTH）
- **被依赖**：当前批次中无其他模组在 manifest 中显式声明依赖 Fzzy Config。
- **冲突**：当前批次中无已知硬冲突。
- **协作**：作为配置基础设施，与 Kotlin for Forge 共同运行；若未来引入使用 Fzzy Config 的模组，可直接复用其配置屏幕。
- **配方/标签**：无直接参与。
- **任务**：无。
- **世界生成**：无。
- **UI**：提供配置屏幕框架与内置快捷键。
- **资源**：无。
- **脚本**：无 KubeJS 脚本交互需求。
- **加载顺序假设**：标准。Fzzy Config 需在 Kotlin for Forge 之后加载，依赖声明确保了这一点。

## 配置意图

`config/fzzy_config/keybinds.toml`：

```toml
version = 1

[pageUp]
key = "page.up"

[pageDown]
key = "page.down"

[home]
key = "home"

[end]
key = "end"

[copy]
ctrl = "true"
key = "c"

[paste]
ctrl = "true"
key = "v"

[cut]
ctrl = "true"
key = "x"

[save]
ctrl = "true"
key = "s"

[undo]
ctrl = "true"
key = "z"

[contextMouse]
type = "mouse"
key = "mouse.right"

[back]
key = "backspace"

[search]
ctrl = "true"
key = "e"

[info]
key = "f1"

[fullExit]
shift = "true"
key = "escape"

[[find]]
ctrl = "true"
key = "f"

[[find]]
key = "f3"

[[contextKeyboard]]
shift = "true"
key = "f10"

[[contextKeyboard]]
key = "menu"

[[act]]
key = "enter"

[[act]]
key = "keypad.enter"
```

- 当前配置为 Fzzy Config GUI 的默认快捷键方案，未做自定义覆盖。
- 常用操作与系统约定一致：`Ctrl+C/V/X` 复制/粘贴/剪切，`Ctrl+S` 保存，`Ctrl+Z` 撤销，`Ctrl+F` 与 `F3` 搜索，`Page Up/Down` 翻页。
- 这些快捷键仅在 Fzzy Config 配置 GUI 内生效，但 `Ctrl+E`（搜索）、`Ctrl+S`（保存）可能与游戏内或其他模组的快捷键冲突，需在实际测试中确认。

## 兼容性与性能

- **客户端/服务端范围**：BOTH。配置同步机制在两侧都需要存在，但 GUI 快捷键仅在客户端生效。
- **存档影响**：无直接存档影响。
- **已知不兼容**：当前批次中无已知冲突。
- **资源成本**：库模组开销极低，配置屏幕仅在打开时占用资源。
- **缓解措施**：保持与 Kotlin for Forge 的版本同步；若未来有模组依赖 Fzzy Config，需验证其版本约束。

## 验证

- [ ] 启动或加载测试
- [ ] Fzzy Config GUI 快捷键无冲突
- [ ] 多人游戏测试（如适用）
- [ ] 配置保存与同步测试

## 风险与开放问题

1. **快捷键冲突**：`Ctrl+S`、`Ctrl+E` 等组合可能与玩家习惯或其他模组键位重叠，需要在实际游玩中验证。
2. **当前无消费者**：批次中暂无模组在 manifest 中声明依赖 Fzzy Config，需确认是否作为未来模组的预置依赖保留。

## 历史

- 2026-07-16: 作为第三批 QoL 模组添加
