# JER Integration JER集成

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/[JER集成] jerintegration-6.5.0.jar`
- 标识与版本：mod_id = `jerintegration`；版本 `6.5.0`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

JER Integration 是 Just Enough Resources（JER）的扩展集成模组，为 Create、Thermal、Immersive Engineering、Tinkers' Construct、Mekanism、Mining Master、Randomite 等科技/内容模组提供额外的资源信息补丁。它与 Jade Addons 类似，在 ProjectNautic 整合包中扮演**前向兼容钩子**的角色：当前这些目标模组大多尚未安装，但预先加入 JER Integration 可确保未来添加它们时，JER 能正确显示其独特的资源获取方式（如 Create 的矿石处理、Thermal 的矿脉等）。

该模组属于 `integration` 分类：它本身不添加独立游戏内容，而是增强 JER 与其他内容模组之间的信息协同。

## 玩家可见行为

- **可发现性**：当前阶段玩家不会看到 JER Integration 的明显效果，因为目标内容模组尚未安装。
- **交互**：无直接交互界面。效果通过 JER 在 JEI 中的信息页在未来内容模组安装后自动体现。
- **进度/奖励**：无直接进度关联。
- **摩擦**：无额外摩擦。未安装目标模组时，JER Integration 几乎无运行时开销。
- **失败状态**：若目标模组版本不兼容，可能导致 JER 信息页缺失或控制台警告。

## 集成关系

- **依赖**：Minecraft `[1.21.1,1.22)`、NeoForge `>=21.1.0`（required, BOTH）
- **冲突**：当前批次中无已知硬冲突。
- **协作**：
  - **JER / JEI**：主信息查询框架，JER Integration 扩展其资源数据来源
  - **Create / Thermal / IE / Tinkers' / Mekanism / Mining Master / Randomite**：未来可能添加的内容模组，JER Integration 将为它们提供专属资源信息补丁
- **配方/标签**：无直接参与
- **任务**：无
- **世界生成**：无
- **UI**：通过 JER 在 JEI 中新增信息标签页
- **资源**：无
- **脚本**：无 KubeJS 脚本交互需求
- **加载顺序**：需在 JER 与 JEI 之后加载

## 配置意图

`config/jerintegration-client.toml`：

```toml
[jer-integration]
  randomite = true
  create = true
  thermal = true
  immersiveengineering = true
  tconstruct = true
  mekanism = true
  miningmaster = true
```

所有补丁默认启用，包括：

- **Create**：机械动力的资源处理信息
- **Thermal**：热力膨胀的矿脉与材料信息
- **Immersive Engineering**：沉浸工程的矿石与多方块结构信息
- **Tinkers' Construct**：匠魂的冶炼与材料信息
- **Mekanism**：通用机械的资源处理信息
- **Mining Master**：采矿大师相关资源信息
- **Randomite Classic**：随机矿石信息

尽管当前这些目标模组均未安装，但保留全部启用可在未来直接生效，无需重新配置。这符合"魔改即基础设施"的设计支柱：提前布设集成点，降低未来内容投放的摩擦。

## 兼容性与性能

- **客户端/服务端范围**：标记为 CLIENT，但需与 JER/JEI 协同工作。
- **存档影响**：无直接存档影响。
- **已知不兼容**：当前目标模组均未安装，无已知冲突。未来安装目标模组时需验证版本匹配。
- **资源成本**：当前几乎无开销；目标模组安装后，JER 信息索引会略微增加初始化时间。
- **缓解措施**：保持与 JER 版本同步；未来内容模组安装后检查 JER 信息页是否准确。

## 验证

- [ ] 启动或加载测试
- [ ] 功能测试（确认 JER Integration 配置加载）
- [ ] 多人游戏测试（如适用）
- [ ] 与未来内容模组（Create、Thermal 等）的集成验证（待未来执行）
- [ ] 与 JER、JEI 的回归检查

## 风险与开放问题

1. **前向兼容的空窗期**：当前 JER Integration 已安装但目标模组缺失，玩家可能困惑于其存在的意义。需在文档中说明这是为未来科技线内容预留的扩展。
2. **版本同步风险**：JER Integration 与 JER 主版本存在兼容关系，更新 JER 时需同步验证。
3. **信息准确性**：目标模组的自定义生成/处理逻辑可能随版本变化，未来添加后需抽样验证 JER 显示数据是否准确。

## 历史

- 2026-07-16: 作为第二批基础模组与 QoL 扩展批次添加
