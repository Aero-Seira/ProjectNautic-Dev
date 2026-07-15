# YetAnotherConfigLib

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/yet_another_config_lib_v3-3.8.2+1.21.1-neoforge.jar`
- 标识与版本：mod_id = `yet_another_config_lib_v3`；版本 `3.8.2+1.21.1-neoforge`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

YetAnotherConfigLib（YACL）是构建式配置库，为依赖它的模组提供声明式配置屏幕、字段验证与资源重载集成。它在包内属于 UI 基础设施层：开发者通过链式 API 描述配置项，YACL 负责生成美观且可交互的配置界面。

在 ProjectNautic 中，它支撑**UI 一致性**与**可配置性**支柱，使效果计时器增强（EffectTimerPlus）等模组能够以统一的视觉风格暴露设置。

## 玩家可见行为

普通玩家不会直接看到 YACL 的名称。其功能通过依赖它的模组间接体现，主要体现在配置界面的颜色选择器提示、图像预览与流畅的滚动体验上。

## 集成关系

- **依赖**：NeoForge（required, BOTH）、Minecraft `[1.21,1.21.1]`（required, BOTH）
- **被依赖**：
  - **effecttimerplus**：需要 YACL `>=3.5.0`（required, CLIENT）
- **冲突**：当前批次中无已知硬冲突。
- **协作**：作为配置基础设施，与 Mod Menu 等配置导航方案协同工作（若未来安装）。
- **配方/标签**：无直接参与。
- **任务**：无。
- **世界生成**：无。
- **UI**：提供配置屏幕框架、颜色选择器与复杂图像格式预览。
- **资源**：无。
- **脚本**：无 KubeJS 脚本交互需求。
- **加载顺序假设**：标准。YACL 需在 effecttimerplus 之前完成加载。

## 配置意图

`config/yacl.json5`：

```json5
{
  "showColorPickerIndicator": true,
  "preloadComplexImageFormats": false
}
```

- **`showColorPickerIndicator: true`**：首次使用颜色选择器时显示闪烁提示，帮助玩家发现颜色选取功能。该提示会在首次使用后自动关闭。
- **`preloadComplexImageFormats: false`**：不在资源重载阶段预加载 `.webp` 与 `.gif` 格式图像，改为按需加载。保持关闭以减少启动时间，符合包的性能优先原则。

YACL 本身的核心配置由依赖它的模组各自持有（如 effecttimerplus 的配置文件）。

## 兼容性与性能

- **客户端/服务端范围**：主要为 CLIENT。虽然 manifest 声明为 BOTH，但配置屏幕与颜色选择器仅在客户端渲染。
- **存档影响**：无直接存档影响。
- **已知不兼容**：当前批次中无已知冲突。
- **资源成本**：配置屏幕仅在打开时占用 GPU/CPU 资源；关闭预加载可减少启动内存峰值。
- **缓解措施**：保持与 effecttimerplus 的版本同步；避免手动删除或替换。

## 验证

- [ ] 启动或加载测试
- [ ] effecttimerplus 配置屏幕可正常打开
- [ ] 多人游戏测试（如适用）
- [ ] 颜色选择器提示与图像加载行为验证

## 风险与开放问题

1. **版本绑定**：YACL 的更新与 effecttimerplus 紧密耦合。
2. **配置库风格碎片化**：包内已同时存在 Cloth Config、Fzzy Config、MidnightLib、YACL 等多个配置库，未来新增模组时应优先复用现有库，避免玩家面对多套不同的设置界面。

## 历史

- 2026-07-16: 作为第三批 QoL 模组添加
