# No Chat Reports

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/[禁用聊天举报] NoChatReports-NEOFORGE-1.21.1-v2.9.1.jar`
- 标识与版本：nochatreports 1.21.1-v2.9.1
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

No Chat Reports 是整合包社交体验的隐私基础设施，用于关闭 Microsoft/ Mojang 的聊天举报系统对玩家的签名追踪。它让玩家在多人游戏中无需将自己的聊天消息与微软服务账户绑定，从而保护服务器社区的表达自由度，同时减少原版聊天安全提示带来的视觉噪音。

该模组属于**实用型安全组件**，直接服务于 ProjectNautic"玩家自主权"与"低摩擦社交"的设计方向：聊天是玩家协作与服务器文化的核心，我们希望消除不必要的审核焦虑和安全警告，让玩家把注意力集中在交流本身。

## 玩家可见行为

- **可发现性**：玩家进入多人服务器时，原版的安全聊天警告、举报按钮和消息状态指示器会被隐藏或改写；客户端标题画面和暂停菜单中会出现 No Chat Reports 的切换按钮。
- **交互**：玩家可在客户端配置中调整签名模式（默认 `PROMPT`，即每次登录时提示是否签名）；服务器管理员可通过服务端配置要求客户端安装此模组。
- **进度/奖励**：无。
- **摩擦/失败**：若服务器要求安装而客户端未安装，会被拒绝进入；若玩家主动选择对消息签名，则举报系统会重新生效，模组无法撤销已签名消息。

## 集成关系

- **依赖**：NeoForge（required, BOTH）。
- **冲突**：与依赖原版聊天举报系统的服务器/插件机制可能存在理念冲突；无已知客户端渲染冲突。
- **协作**：与 Beautified Chat Server、Chat Heads、ChatNotify 共同构成整合包的聊天体验栈；所有聊天增强模组均基于 No Chat Reports 处理后的聊天通道工作。
- **配方/标签**：无。
- **任务**：无。
- **世界生成**：无。
- **UI**：在聊天屏幕、暂停菜单和社交屏幕添加了 No Chat Reports 的状态/切换按钮；隐藏原版举报按钮与安全提示。
- **资源**：无特殊资源需求。
- **脚本**：无 KubeJS 交互。
- **加载顺序假设**：标准。

## 配置意图

以下配置反映了整合包在隐私、兼容性和视觉一致性之间的取舍，未列出的项保持默认值。

### `config/NoChatReports/NCR-Client.json`

- `defaultSigningMode: "PROMPT"`：默认每次连接服务器时询问是否签名，让玩家对是否暴露聊天身份保持掌控。
- `enableMod: true`：启用模组功能。
- `showNCRButton: true`、`showReloadButton: true`：保留标题画面的切换按钮和配置重载按钮，便于玩家临时调整。
- `verifiedIconEnabled: true`、`showServerSafety: true`：在相关 UI 中显示服务器安全状态图标，帮助玩家判断当前服务器是否启用了聊天签名。
- `hideInsecureMessageIndicators: true`、`hideModifiedMessageIndicators: true`、`hideSystemMessageIndicators: true`、`hideWarningToast: true`：隐藏原版各类聊天安全/修改提示和警告弹窗，减少视觉噪音。
- `disableTelemetry: true`、`removeTelemetryButton: true`：关闭遥测并移除对应按钮，与整合包隐私优先方向一致。
- `demandOnServer: false`：客户端配置不要求服务器必须安装该模组，保持对非 NCR 服务器的兼容。

### `config/NoChatReports/NCR-Common.json`

- `demandOnClient: false`：服务端不强制要求客户端安装 No Chat Reports，降低公共服务器玩家的进入门槛。
- `convertToGameMessage: true`：将玩家消息转换为游戏消息类型，避免被系统标记为需签名的聊天消息。
- `addQueryData: true`：向服务器查询数据中添加 NCR 相关信息，便于服务器识别客户端能力。
- `enableDebugLog: false`：关闭调试日志，避免生产环境日志膨胀。

### `config/NoChatReports/NCR-Encryption.json`

- `enableEncryption: false`：当前不启用端到端聊天加密。加密功能会显著改变聊天系统行为，并可能与 Beautified Chat Server 等格式化模组产生不可预期的交互，因此保持关闭，仅保留未来手动开启的接口。
- `showEncryptionButton: true`、`showEncryptionIndicators: true`：仍显示加密相关按钮和指示器，方便玩家在看到需求时自行决定是否启用。
- `encryptableCommands`：列出可加密的消息命令（私聊、回复等），当前作为默认清单保留。

### `config/NoChatReports/NCR-ServerPreferences.json`

- `signingModes: {}`：当前未针对特定服务器保存签名偏好，未来若玩家对常用服务器有稳定偏好，可在此记录。

## 兼容性与性能

- **客户端/服务端范围**：BOTH。客户端负责隐藏 UI 和签名控制，服务端负责消息类型转换和是否要求客户端安装。
- **存档影响**：无。
- **已知不兼容**：与强制启用原版聊天举报的服务器机制冲突；某些依赖聊天签名的反作弊/审核插件可能误判消息来源。
- **资源成本**：极低，仅在玩家发送/接收聊天消息时进行少量协议转换，无持续 CPU/GPU 开销。
- **缓解措施**：通过 `demandOnServer: false` 和 `demandOnClient: false` 保持双向兼容；关闭加密避免复杂交互风险。

## 验证

- [ ] 启动或加载测试
- [ ] 功能测试
- [ ] 多人游戏测试（如适用）
- [ ] 性能或世界生成测试（如适用）
- [ ] 相关系统的回归检查

## 风险与开放问题

1. **隐私与服务器政策冲突**：部分公共服务器可能依赖原版举报系统维持秩序，若未来在服务器列表中推荐 No Chat Reports，需明确告知玩家该模组会改变聊天问责模型。
2. **加密功能未启用**：`NCR-Encryption.json` 保持关闭意味着私聊内容仍按原样传输，若有强隐私需求场景，需重新评估是否启用并测试与聊天格式化模组的兼容性。

## 历史

- 2026-07-16: 作为第三批 QoL 模组添加
