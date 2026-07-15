# Smooth GUI

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/smoothgui-neoforge-2.0.1+mc1.21.jar`
- 标识与版本：smoothgui 2.0.1
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Smooth GUI 为 ProjectNautic 提供 GUI 打开/关闭时的缩放动画，直接服务于"UI 愉悦感"设计支柱。它用平滑的缩放与淡入淡出替代原版瞬间弹出的屏幕切换，使背包、菜单、容器等界面的出现和消失更加柔和。

该模组与 Smooth Scrolling 共同构成 ProjectNautic 的界面动效层：Smooth GUI 负责屏幕整体出现/消失动画，Smooth Scrolling 负责列表/快捷栏/聊天的滚动平滑。

## 玩家可见行为

- **可发现性**：玩家打开背包、暂停菜单、容器界面或关闭界面时，整个 GUI 会以缩放动画形式出现或消失。
- **交互**：无需主动操作；玩家可在配置文件中调整动画时间、缩放、方向、背景模糊等。
- **进度/奖励**：无内置进度；动画作为界面切换的视觉润色。
- **摩擦/失败**：动画时间过长可能让玩家感觉操作响应变慢；某些模组的自定义 GUI 可能不受 Smooth GUI 影响，导致部分界面动画不一致。

## 集成关系

- **依赖**：Minecraft 1.21.1（required, CLIENT）。
- **冲突**：无已知严重冲突。
- **协作**：与 **Smooth Scrolling** 共同强化界面动效；与 **Modern UI** 的现代化屏幕框架在视觉风格上互补。
- **配方/标签**：无直接参与。
- **任务**：无。
- **世界生成**：无。
- **UI**：全局影响所有支持的 GUI 屏幕。
- **资源**：依赖内置动画逻辑，无需额外资源包。
- **脚本**：无直接 KubeJS 交互。
- **加载顺序假设**：标准 CLIENT 端加载。

## 配置意图

`config/smoothgui.json` 当前配置启用了动画并做了风格统一：

- **`enableAnimation=true`**：启用 GUI 动画。
- **`animationTime=220`**：动画时长 220 ms，提供可见的平滑过渡，但不至于让玩家等待。
- **`animationScale=1.0`**：默认缩放幅度，保持 1 倍自然缩放。
- **`animationStyle="BACK"`**：使用 BACK 缓动曲线，使动画在结束时有轻微回弹，增强视觉弹性。
- **`animationDirection="DOWN"`**：动画方向向下，保持统一的出现方向。
- **`modifyBackground=true`**：启用背景修改。
- **`backgroundOpacity=0.65`**：背景不透明度 0.65，保留半透明遮罩但不过度遮挡游戏世界。
- **`backgroundFadeTime=150`**：背景淡入淡出时间 150 ms，与 GUI 动画同步。
- **`alwaysBlurBackground=false`**：不强制模糊背景，避免低端设备性能压力。
- **`repeatSameScreen=false`**：相同屏幕重复打开时不重复播放动画，减少操作疲劳。
- **`screensForceEnabled=[]` 与 `screensForceDisabled=[]`**：未强制启用或禁用任何屏幕，使用默认检测逻辑。

## 兼容性与性能

- **客户端/服务端范围**：仅 **CLIENT**。
- **存档影响**：无。
- **已知不兼容**：无。
- **资源成本**：GUI 缩放与背景淡化为纯 CLIENT 端开销；开启背景模糊会显著增加 GPU 负载，当前已关闭。
- **缓解措施**：`alwaysBlurBackground=false` 关闭模糊；`animationTime=220` 控制动画时长；`backgroundOpacity=0.65` 保持半透明而非全黑。

## 验证

- [ ] 启动或加载测试
- [ ] 功能测试
- [ ] 多人游戏测试（如适用）
- [ ] 性能或世界生成测试（如适用）
- [ ] 相关系统的回归检查

## 风险与开放问题

1. 与 Modern UI 同时存在时，两者对 GUI 背景与动画的处理可能有渲染顺序冲突，需实机验证。
2. 某些模组的自定义屏幕可能无法被 Smooth GUI 识别，导致部分界面有动画而部分没有，产生视觉不一致。

## 历史

- 2026-07-16: 作为第三批 QoL 模组添加
