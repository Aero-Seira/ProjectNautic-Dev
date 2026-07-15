# ChatNotify

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/chatnotify-neoforge-2.6.9+1.21.1.jar`
- 标识与版本：chatnotify 2.6.9+1.21.1
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

ChatNotify 是整合包的聊天关键词通知组件，当玩家收到包含特定关键词或自己名字的聊天消息时，以声音、高亮文字等方式提醒玩家。它解决了多人游戏中信息过载的问题：玩家不必持续盯着聊天栏，也能在有人呼叫自己或触发重要事件时立即获得反馈。

该模组属于**实用型 QoL 组件**，支撑 ProjectNautic"社交响应性"与"低摩擦体验"的设计方向：协作、交易、救援等场景都依赖玩家及时看到聊天信息，ChatNotify 让关键信息不再被战斗、建筑或探索中的视觉焦点淹没。

## 玩家可见行为

- **可发现性**：当收到包含自己名字或触发词的消息时，玩家会听到提示音（默认音符盒钟声），对应关键词在聊天中会被染成醒目的金色（`#FFD700`）。
- **交互**：玩家可通过模组配置界面或配置文件添加、删除、启用/禁用触发词；默认配置已包含"Profile name"和"Display name"两个触发器，对应玩家自身标识。
- **进度/奖励**：无。
- **摩擦/失败**：若触发词设置过于宽泛（如常见短词），会导致频繁提示造成噪音；若 Beautified Chat Server 的格式变更导致关键词被分隔，可能触发失败。

## 集成关系

- **依赖**：NeoForge（required, CLIENT）。
- **冲突**：与同样重写聊天通知或声音事件的模组可能存在冲突；无已知严重冲突。
- **协作**：与 No Chat Reports、Beautified Chat Server、Chat Heads 共同构成聊天体验栈；通知检测基于服务端格式化后的聊天文本，因此触发词需与 Beautified Chat Server 的 `chatMessageFormat` 输出兼容。
- **配方/标签**：无。
- **任务**：无。
- **世界生成**：无。
- **UI**：无独立常驻 UI，通过配置界面管理触发规则；收到通知时聊天文字会被重新着色。
- **资源**：无特殊资源需求；声音事件使用 Minecraft 原声音频。
- **脚本**：无 KubeJS 交互。
- **加载顺序假设**：标准 CLIENT 端模组；应在 Beautified Chat Server 完成服务端格式化后的聊天包到达客户端之后处理。

## 配置意图

### `config/chatnotify.json`

- `notifMode: "ALL_SINGLE_SOUND"`：每条匹配消息只播放一次提示音，避免连续触发造成听觉 spam。
- `restyleMode: "ALL_INSTANCES"`：聊天中触发词的所有出现位置都会高亮，确保玩家在滚动历史记录时也能发现。
- `sendMode: "PACKET"`：通过数据包发送通知，兼容 NeoForge 网络环境。
- `soundSource: "PLAYERS"`：提示音归类为玩家音效源，受客户端玩家音效音量滑块控制。
- `defaultColor: 16761856`（金色 `#FFD700`）：触发词默认使用高对比度的金色，在明暗聊天背景下均醒目。
- `defaultSound`：默认启用音符盒钟声（`block.note_block.bell`），音量与音高均为 `1.0`，温和不刺耳。
- `detectionMode: "HUD_KNOWN_TAGS"`：优先检测 HUD 聊天区域中已知标签的文本，兼顾准确性与兼容性。
- `actionBarDetectionMode: "NONE"`、`titleDetectionMode: "NONE"`、`subtitleDetectionMode: "NONE"`：关闭对 action bar、标题、字幕的检测，专注于聊天消息，避免非聊天系统消息触发误报。
- `senderDetectionMode: "COMBINED"`：结合多种信息源判断发送者身份。
- `checkOwnMessages: true`：检测自己发送的消息；在某些情况下玩家可能通过脚本或命令触发自己的关键词，保留检测以便排查。
- `prefixes`：列出 `/shout`、`/say`、`/me`、`!` 等命令前缀，允许这些命令形式的广播消息也能触发通知。
- 默认通知规则：
  - 启用一条通知，包含触发器 `"Profile name"` 和 `"Display name"`，即当聊天中出现玩家档案名或显示名时触发。
  - 当前未启用 `replacementMsg`、`statusBarMsg`、`titleMsg`、`subtitleMsg`、`toastMsg`、`typedMsg`、`clipboardMsg` 等额外输出，保持最低侵入性——仅声音 + 文字高亮。
  - `inclusionTriggers` 和 `exclusionTriggers` 为空，未做进一步过滤；`responses` 为空，未配置自动回复。

## 兼容性与性能

- **客户端/服务端范围**：CLIENT。仅影响客户端聊天处理，无需服务端安装；但检测对象是经过服务端格式化后的聊天文本。
- **存档影响**：无。
- **已知不兼容**：与隐藏或替换原版聊天 HUD 的模组可能冲突；触发词检测对颜色代码敏感，若 Beautified Chat Server 改变文本格式，可能需要同步调整触发词。
- **资源成本**：
  - CPU：很低，仅在聊天消息到达时进行字符串匹配。
  - 内存：触发规则列表占用可忽略。
  - 声音：使用原版音效，无额外音频资源。
- **缓解措施**：通过 `ALL_SINGLE_SOUND` 限制声音频率；通过精确的默认触发器（玩家名字）避免泛词误触发；关闭 title/action bar/subtitle 检测减少非目标噪音。

## 验证

- [ ] 启动或加载测试
- [ ] 功能测试
- [ ] 多人游戏测试（如适用）
- [ ] 性能或世界生成测试（如适用）
- [ ] 相关系统的回归检查

## 风险与开放问题

1. **触发词与格式化文本的耦合**：Beautified Chat Server 当前在名称前加了时间戳和分隔符，ChatNotify 的默认检测基于 `"Profile name"` 和 `"Display name"`，若服务器昵称插件改变显示名，或格式化字符串调整，可能导致自我呼叫通知失效。
2. **默认配置过于个人化**：当前仅检测玩家自身名字，未包含通用紧急关键词（如"help"、"交易"、"TPA"），若整合包强调强协作玩法，未来可能需要提供一组社区通用触发词模板。

## 历史

- 2026-07-16: 作为第三批 QoL 模组添加
