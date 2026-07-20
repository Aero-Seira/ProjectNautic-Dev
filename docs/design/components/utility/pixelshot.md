# Pixelshot

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/Pixelshot-v21.1.1-1.21.1-NeoForge.jar`
- 标识与版本：mod_id = `pixelshot`；版本 `21.1.1`；作者 BarracudaATA, Fuzs, Pascal Roeleven
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Pixelshot 是 Mineshot 的延续：高分辨率截图与正交（等轴）相机（verified，mods.toml 描述："High-resolution screenshots, and an orthographic camera all in one. A continuation of Mineshot by BarracudaATA."）。服务于建筑展示、宣传素材与服务器运营内容产出，属创作工具型 QoL。

## 玩家可见行为

- 通过键位/界面截取远超屏幕分辨率的截图，或切换正交视角拍摄等轴投影图。

## 集成关系

- **依赖**：Minecraft 1.21.1、NeoForge `[21.1.21,)`、PuzzlesLib `[21.1.7,)`（required）。
- **协作**：与 Screenshot to Clipboard（截图到剪贴板）职责互补（高清 vs 便捷）；与官方光影叠加出图为主要使用场景。

## 配置意图

`config/pixelshot-client.toml`（39 行）保持默认，未做包级自定义。

## 兼容性与性能

- 仅客户端；高分辨率截取瞬间有明显资源峰值（预期行为）；无存档影响。

## 验证

- [ ] 启动或加载测试
- [ ] 高分辨率截图与正交相机功能测试（含光影开启场景）

## 风险与开放问题

- 无已知重大风险。

## 历史

- 2026-07-21: 作为第六批 Fuzs 系模组添加
