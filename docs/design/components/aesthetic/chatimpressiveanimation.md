# Chat Impressive Animation

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/[聊动魅影] ChatImpressiveAnimation-neoforge-1.6.0+mc1.21.5.jar`
- 标识与版本：chatimpressiveanimation 1.6.0+mc1.21.5
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Chat Impressive Animation 为 ProjectNautic 提供聊天栏发送与展开时的淡入淡出动画，直接服务于"UI 愉悦感"设计支柱。它让聊天消息的发送和聊天栏的展开不再生硬，而是带有一个短暂的透明度过渡，使社交界面更符合整合包整体的动效风格。

该模组与 Smooth GUI、Smooth Scrolling 共同构成 ProjectNautic 的界面动效层：Smooth GUI 负责屏幕整体动画，Smooth Scrolling 负责聊天/列表滚动，Chat Impressive Animation 负责聊天消息与聊天栏的专属动画。

## 玩家可见行为

- **可发现性**：玩家发送聊天消息时，消息会以淡入动画出现；打开聊天栏时，聊天输入框会以淡入动画展开；关闭时则淡出。
- **交互**：无需主动操作；玩家可在配置文件中调整动画开关与淡入时间。
- **进度/奖励**：无内置进度；动画作为聊天交互的视觉润色。
- **摩擦/失败**：动画时间过长可能让玩家感觉聊天响应变慢；在快速连续发送消息时可能产生动画堆叠。

## 集成关系

- **依赖**：Minecraft 1.21.1（required, CLIENT）。
- **冲突**：无已知严重冲突。
- **协作**：与 **Smooth GUI**、**Smooth Scrolling** 共同强化界面动效；与 **Chat Heads** 在聊天 UI 上共存，需验证渲染顺序。
- **配方/标签**：无直接参与。
- **任务**：无。
- **世界生成**：无。
- **UI**：影响聊天栏与聊天消息的渲染动画。
- **资源**：依赖内置动画逻辑，无需额外资源包。
- **脚本**：无直接 KubeJS 交互。
- **加载顺序假设**：标准 CLIENT 端加载。

## 配置意图

`config/chatimpressiveanimation.json` 当前配置启用了聊天动画并移除了消息指示器：

- **`enableChatSendingAnimation=true`**：启用发送聊天消息时的淡入动画。
- **`chatSendingAnimationFadeTime=170`**：消息淡入时间为 170 ms，提供柔和过渡但不至于让消息出现滞后。
- **`enableChatBarAnimation=true`**：启用聊天栏展开/关闭的淡入淡出动画。
- **`chatBarAnimationFadeTime=170`**：聊天栏动画时间同样为 170 ms，与消息动画保持一致的节奏。
- **`removeMessageIndicator=true`**：移除聊天消息左侧的"已发送/已接收"指示器，使聊天界面更简洁，符合整合包 UI 一致性目标。

整体配置意图是"让聊天界面动起来，但保持简洁"：通过一致的 170 ms 动画时间与移除指示器，减少聊天 UI 的视觉噪音。

## 兼容性与性能

- **客户端/服务端范围**：仅 **CLIENT**。
- **存档影响**：无。
- **已知不兼容**：无。
- **资源成本**：纯 CLIENT 端 UI 透明度过渡动画，开销极低。
- **缓解措施**：动画时间控制在 170 ms，避免影响聊天输入的即时反馈。

## 验证

- [ ] 启动或加载测试
- [ ] 功能测试
- [ ] 多人游戏测试（如适用）
- [ ] 性能或世界生成测试（如适用）
- [ ] 相关系统的回归检查

## 风险与开放问题

1. Jar 文件名为 `mc1.21.5` 而整合包目标为 Minecraft 1.21.1 / NeoForge 21.1.235，版本跨度较大，需重点验证加载兼容性与聊天渲染行为。
2. 与 Chat Heads 同时修改聊天消息渲染时，需验证头像、消息文本与淡入动画的层级关系是否正确。

## 历史

- 2026-07-16: 作为第三批 QoL 模组添加
