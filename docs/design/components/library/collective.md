# Collective

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/collective-1.21.1-8.39.jar`
- 标识与版本：mod_id = `collective`；版本 `8.39`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Collective 是 Serilum 系列模组的共享工具库，提供更新检查、翻译下载、实体掉落收集、方块状态查询等通用能力。它在包内属于基础设施层：依赖它的 Serilum 模组无需重复实现这些跨模组的通用逻辑。

在 ProjectNautic 中，它支撑**可维护性**与**本地化**支柱，使聊天美化等 Serilum 模组能一致地处理多语言文本与可选更新提示。

## 玩家可见行为

普通玩家不会直接看到 Collective 的名称。其功能通过依赖它的模组间接体现，例如 Serilum 模组的汉化文本、可选的更新提示消息等。

## 集成关系

- **依赖**：Minecraft `[1.21,1.21.1]`（required, BOTH）
- **被依赖**：
  - **beautifiedchatserver**：需要 Collective `>=8.25`（required, BOTH）
- **冲突**：当前批次中无已知硬冲突。
- **协作**：作为 Serilum 生态的基础设施，与未来可能添加的其他 Serilum 模组共享配置与工具方法。
- **配方/标签**：无直接参与。
- **任务**：无。
- **世界生成**：无。
- **UI**：无直接 UI。
- **资源**：可在服务器端向未安装 Collective 的客户端推送翻译资源包（当前未启用）。
- **脚本**：无 KubeJS 脚本交互需求。
- **加载顺序假设**：标准。Collective 需在 beautifiedchatserver 之前完成加载。

## 配置意图

`config/collective.json5`：

```json5
{
  "enableUpdateChecker": true,
  "transferItemsBetweenReplacedEntities": true,
  "loopsAmountUsedToGetAllEntityDrops": 100,
  "findABlockCheckAroundEntitiesDelayMs": 30000,
  "enablePatronPets": true,
  "downloadNonEnglishTranslations": true,
  "pushTranslationResourcePack": false,
  "requireTranslationResourcePack": true,
  "serverLanguage": "en_us",
  "itemNameTranslationMode": "auto",
  "updateMinecraftWindowTitleInDevMode": false
}
```

- **`enableUpdateChecker: true`**：当依赖 Collective 的 Serilum 模组有更新时，在控制台输出提示。该检查是异步且可选的，不影响游戏性能。
- **`downloadNonEnglishTranslations: true`**：当客户端语言不是英文时，尝试从 Serilum 官方仓库下载翻译文件，改善中文等非英文玩家的体验。
- **`pushTranslationResourcePack: false`**：服务器端不向未安装 Collective 的客户端强制推送翻译资源包。保持关闭以避免玩家加入服务器时被资源包弹窗打断。
- **`requireTranslationResourcePack: true`**：若未来启用推送，资源包将被标记为必需，防止玩家误拒绝后长期看到英文键名。
- **`serverLanguage: "en_us"`**：未接收资源包的玩家默认看到英文服务端文本。
- **`itemNameTranslationMode: "auto"`**：自定义物品名称的翻译模式跟随首个加入的玩家语言自动决定。
- **`loopsAmountUsedToGetAllEntityDrops: 100`**：实体掉落预计算循环次数，默认值兼顾加载速度与准确性。
- **`findABlockCheckAroundEntitiesDelayMs: 30000`**：方块状态查询延迟 30 秒，用于依赖该检查的模组，默认即可平衡 TPS 与响应性。

## 兼容性与性能

- **客户端/服务端范围**：BOTH。作为库模组，两侧均需安装。
- **存档影响**：无直接存档影响。
- **已知不兼容**：当前批次中无已知冲突。
- **资源成本**：更新检查会产生少量出站网络请求；翻译下载仅在非英文语言启动时触发一次；实体掉落预计算在世界加载时占用短暂 CPU 时间。
- **缓解措施**：已关闭服务端资源包推送，避免网络与用户体验风险；更新检查为异步，不会阻塞启动。

## 验证

- [ ] 启动或加载测试
- [ ] beautifiedchatserver 功能测试
- [ ] 多人游戏测试（如适用）
- [ ] 翻译下载与更新检查行为验证

## 风险与开放问题

1. **出站网络请求**：`enableUpdateChecker` 与 `downloadNonEnglishTranslations` 会在启动时访问外部服务，离线环境或严格网络策略下可能超时或报错。
2. **翻译资源包策略**：当前未向客户端推送翻译资源包，若未来 beautifiedchatserver 新增大量自定义文本，需重新评估是否启用推送。

## 历史

- 2026-07-16: 作为第三批 QoL 模组添加
