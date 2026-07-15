# CreativeCore

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/CreativeCore_NEOFORGE_v2.13.41_mc1.21.1.jar`
- 标识与版本：creativecore 2.13.41
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

CreativeCore 是 CreativeMD 系列模组的共享库，为 ProjectNautic 中的 **AmbientSounds** 等模组提供 GUI 缩放、配置同步、网络协议与通用工具方法。它本身不直接产生玩家可见内容，但作为多个美学/音频模组的前置，是整合包环境体验层的基础设施。

在 ProjectNautic 中，CreativeCore 的定位是"库 / 集成支撑"：它确保依赖它的模组在 CLIENT 端能够正确渲染配置界面、同步设置，并在跨平台环境下保持一致的 GUI 缩放行为。

## 玩家可见行为

CreativeCore 作为库模组，玩家不会直接与其交互。它的影响间接体现在 AmbientSounds 等模组的配置界面可正常打开、GUI 缩放范围合理、设置能够正确保存与同步。

## 集成关系

- **依赖**：Minecraft 1.21.1（required, CLIENT/BOTH，取决于依赖它的模组）。
- **冲突**：无已知严重冲突。
- **协作**：
  - 为 **AmbientSounds** 提供配置 UI 与网络同步支持。
  - 为 CreativeMD 生态的其他可能模组提供通用基础设施。
- **配方/标签**：无直接参与。
- **任务**：无。
- **世界生成**：无。
- **UI**：自身不新增 UI，但为依赖模组提供 GUI 缩放等底层能力。
- **资源**：无额外资源需求。
- **脚本**：无直接 KubeJS 交互。
- **加载顺序假设**：作为库模组，需在依赖它的模组之前加载；标准 NeoForge 加载顺序自动处理。

## 配置意图

CreativeCore 的配置文件当前保持极简：

### `config/creativecore.json`

- 仅包含 `usergroups` 配置，按游戏模式（`creative`、`survival`、`adventure`）定义过滤器，用于 CreativeCore 的权限/用户组系统。当前未做自定义，保留默认分组作为占位。

### `config/creativecore-client.json`

- 仅包含 `maxGuiScale=10`，限制客户端 GUI 缩放的最大倍数为 10。该设置防止玩家将 GUI 缩放到过大尺寸导致布局错乱，同时保留足够的缩放空间以适应高分辨率屏幕。

整体配置意图是"启用基础 CLIENT 端支持能力，不做过度自定义"：CreativeCore 的主要价值在于让上层模组正常工作，而非通过自身配置塑造体验。

## 兼容性与性能

- **客户端/服务端范围**：**BOTH**（作为库，具体范围由依赖它的模组决定；AmbientSounds 主要使用 CLIENT 端功能）。
- **存档影响**：无。
- **已知不兼容**：无。
- **资源成本**：库本身开销可忽略；实际开销由上层模组决定。
- **缓解措施**：通过 `maxGuiScale=10` 限制 GUI 缩放上限，避免极端缩放导致的渲染问题。

## 验证

- [ ] 启动或加载测试
- [ ] 功能测试
- [ ] 多人游戏测试（如适用）
- [ ] 性能或世界生成测试（如适用）
- [ ] 相关系统的回归检查

## 风险与开放问题

1. CreativeCore 的版本更新可能影响依赖它的模组（如 AmbientSounds）的兼容性，升级时需同步验证。
2. 当前仅 AmbientSounds 明确依赖 CreativeCore，若未来引入更多 CreativeMD 模组，需重新评估权限/用户组配置的适用性。

## 历史

- 2026-07-16: 作为第三批 QoL 模组添加
