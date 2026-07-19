# NewVisualKeybing 可视化键位

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/New Visual Keybing-1.21.1-neoforge-0.6.16.jar`
- 标识与版本：mod_id = `newvisualkeybing`；版本 `0.6.16`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

NewVisualKeybing 是 ProjectNautic 整合包的键位可视化模组，用于在屏幕上显示当前按键状态与按键绑定，方便玩家录制视频、直播或学习复杂操作。它支撑整合包**UI 一致性**与**Vanilla+ 起步**的设计支柱：在不破坏原版视觉风格的前提下，提供可自定义的键位反馈界面，增强信息可读性与内容创作友好度。

该模组为 CLIENT ONLY，主要面向内容创作者与希望提升操作透明度的玩家。

## 玩家可见行为

- **可发现性**：玩家进入世界后会在屏幕看到当前按键的可视化面板，显示 WASD、空格、Shift、鼠标等按键状态。
- **交互**：
  - 玩家可通过配置切换皮肤风格、组合键显示模式、面板折叠状态
  - 支持显示多个按键同时按下的组合状态
- **进度/奖励**：无直接进度关联。
- **摩擦**：默认使用 VANILLA 皮肤，与 Minecraft 原生 UI 风格接近，不会给普通玩家造成突兀感。
- **失败状态**：配置冲突可能导致键位显示不完整或与某些全屏 HUD 模组重叠。

## 集成关系

- **依赖**：Minecraft `[1.21.1,1.22)`、NeoForge `>=21.1.80`（required, BOTH）
- **冲突**：当前批次中无已知硬冲突。与 Xaero's Minimap 等屏幕 HUD 模组可能存在空间竞争。
- **协作**：可与小地图、状态栏等 HUD 模组共同构成信息展示层；皮肤风格可通过资源包扩展。
- **配方/标签**：无直接参与
- **任务**：无
- **世界生成**：无
- **UI**：在游玩界面叠加键位可视化面板
- **资源**：`config/newvisualkeybing/ui_textures/` 下包含可自定义的纹理包配置
- **脚本**：无 KubeJS 脚本交互需求
- **加载顺序**：标准模组加载顺序即可

## 配置意图

### `config/newvisualkeybing/keybind_viewer.json`

```json
{
  "hideNonSelectedMod": false,
  "comboKeysNonConflicting": true,
  "mousePanelCollapsed": false,
  "detailPanelCollapsed": false,
  "uiSkin": "VANILLA"
}
```

- **`uiSkin: VANILLA`**：使用原版风格皮肤，与 Minecraft 默认 HUD 保持一致，避免破坏沉浸感。
- **`comboKeysNonConflicting: true`**：组合键显示采用非冲突模式，便于玩家理解多键组合而不与其他绑定混淆。
- **`hideNonSelectedMod: false`**：不隐藏非当前选中模组的按键绑定，保留完整信息。
- **`mousePanelCollapsed: false` / `detailPanelCollapsed: false`**：默认展开鼠标面板与详情面板，方便初次使用。

### `config/newvisualkeybing/ui_textures/`

包含 `pack.json` 与 `README.md`，说明如何制作自定义皮肤。当前未添加自定义资源，保留默认 VANILLA 皮肤。

整体配置体现"辅助但不喧宾夺主"的设计意图：键位可视化默认开启且风格贴近原版，玩家可按需调整或关闭。

## 兼容性与性能

- **客户端/服务端范围**：CLIENT ONLY。服务端无需安装。
- **存档影响**：无。NewVisualKeybing 不修改存档数据。
- **已知不兼容**：当前批次中无已知硬冲突。未来添加 HUD 模组时需注意屏幕空间分配。
- **资源成本**：键位面板渲染开销极低，对帧率影响可忽略。
- **缓解措施**：若玩家不需要键位显示，可通过配置或按键绑定关闭；未来若与小地图冲突，可调整面板位置。

## 验证

- [ ] 启动或加载测试
- [ ] 功能测试（键位显示、组合键、皮肤切换）
- [ ] 多人游戏测试（如适用，CLIENT 端独立生效）
- [ ] UI 空间测试（与 Xaero's Minimap 等 HUD 模组共存）
- [ ] 相关系统回归检查

## 风险与开放问题

1. **HUD 布局冲突**：默认键位面板位置可能与未来的小地图、状态栏模组重叠，需要统一规划 HUD 区域。
2. **内容创作与沉浸感的平衡**：键位可视化对创作者友好，但部分玩家可能认为破坏沉浸。需在默认设置中保持克制（VANILLA 皮肤、可关闭）。

## 历史

- 2026-07-20: 新增 `config/newvisualkeybing/keybind_profiles.json`（按键配置档案文件，当前 `profiles` 为空、仅记录 `key.modernui.zoom` 优先级——属生成默认，尚未做 deliberate 配置）
- 2026-07-16: 作为第二批基础模组与 QoL 扩展批次添加
