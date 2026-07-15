# Mouse Tweaks 鼠标手势

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/[鼠标手势] MouseTweaks-neoforge-mc1.21-2.26.1.jar`
- 标识与版本：mod_id = `mousetweaks`；版本 `2.26.1`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Mouse Tweaks 是 ProjectNautic 整合包的库存管理 QoL 模组，通过扩展鼠标右键、左键与滚轮在物品栏中的行为，显著减少整理物品、合成、搬运时的重复操作。它支撑整合包**Vanilla+ 起步**与**QoL 工具**的设计意图：在不改变原版游戏机制的前提下，提升日常交互效率，让玩家将更多注意力集中在探索与建造上。

Mouse Tweaks 为 CLIENT ONLY，不影响服务端逻辑或反作弊检测（仅改变客户端物品栏交互方式）。

## 玩家可见行为

- **可发现性**：玩家打开背包或容器时会自然体验到更流畅的物品操作：
  - 按住右键拖动可快速分配一组物品到多个格子
  - 按住左键拖动可快速拾取相同物品
  - 鼠标滚轮在物品上滚动可快速移动单个物品
- **交互**：无新增 GUI，所有操作通过鼠标手势在原版物品栏中完成。
- **进度/奖励**：无直接进度关联。
- **摩擦**：无学习成本，操作逻辑符合玩家对"拖拽"与"滚轮"的直觉。
- **失败状态**：与服务端反作弊插件（如某些自定义背包验证）冲突时，可能导致物品移动被服务端拒绝。

## 集成关系

- **依赖**：NeoForge（required, CLIENT）
- **冲突**：当前批次中无已知硬冲突。与 Inventory Profiles Next 等同类模组功能重叠，不建议同时安装。
- **协作**：与 JEI 的配方自动填充协同：玩家可快速整理材料后一键填充合成网格。
- **配方/标签**：无直接参与
- **任务**：无
- **世界生成**：无
- **UI**：扩展原版物品栏与容器的鼠标交互
- **资源**：无
- **脚本**：无 KubeJS 脚本交互需求
- **加载顺序**：标准模组加载顺序即可

## 配置意图

`config/MouseTweaks.cfg`：

```cfg
RMBTweak=1
LMBTweakWithItem=1
LMBTweakWithoutItem=1
WheelTweak=1
WheelSearchOrder=1
WheelScrollDirection=0
ScrollItemScaling=0
Debug=0
```

- **`RMBTweak=1`**：启用右键手势，按住右键拖动可在多个格子间分配物品。
- **`LMBTweakWithItem=1`**：启用持物时左键手势，快速拾取同类物品。
- **`LMBTweakWithoutItem=1`**：启用空手时左键手势，快速从容器中提取物品。
- **`WheelTweak=1`**：启用滚轮手势，滚轮向上/向下移动单个物品。
- **`WheelSearchOrder=1`** / `WheelScrollDirection=0` / `ScrollItemScaling=0`：使用默认滚轮搜索顺序与滚动方向。
- **`Debug=0`**：关闭调试输出。

所有核心功能均启用，体现"最大化日常操作效率"的配置意图，同时保持默认参数以确保与其他模组的兼容性。

## 兼容性与性能

- **客户端/服务端范围**：CLIENT ONLY。服务端无需安装。
- **存档影响**：无。Mouse Tweaks 不修改存档数据。
- **已知不兼容**：与 Inventory Profiles Next、Item Scroller 等同类模组功能重叠，不建议同时安装。
- **资源成本**：几乎无运行时开销。
- **缓解措施**：若服务器使用严格反作弊插件，可临时禁用 WheelTweak 或 RMBTweak。

## 验证

- [ ] 启动或加载测试
- [ ] 功能测试（右键分配、左键拾取、滚轮移动）
- [ ] 多人游戏测试（与服务端物品同步）
- [ ] 相关系统（JEI 自动填充）回归检查

## 风险与开放问题

1. **反作弊兼容性**：部分服务器插件可能将快速物品移动视为异常行为，需在服务器部署文档中说明。
2. **与未来同类模组的冲突**：若未来考虑 Inventory Profiles Next 等更强大的库存管理模组，需评估是否替换 Mouse Tweaks。

## 历史

- 2026-07-16: 作为第二批基础模组与 QoL 扩展批次添加
