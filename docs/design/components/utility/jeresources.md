# Just Enough Resources

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/JustEnoughResources-NeoForge-1.21.1-1.6.0.17.jar`
- 标识与版本：mod_id = `jeresources`；版本 `1.6.0.17`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Just Enough Resources（JER）是 ProjectNautic 整合包的资源信息扩展模组，作为 JEI 的插件，为玩家提供世界生成（矿石、植物）、生物掉落、村民交易、地牢战利品等来源查询。它支撑整合包**QoL 工具**与**Vanilla+ 起步**的设计意图：在不破坏探索乐趣的前提下，帮助玩家理解资源获取路径，降低面对大量模组内容时的认知负担。

JER 主要作用于 CLIENT 侧，依赖 JEI 的配方查看界面展示信息。

## 玩家可见行为

- **可发现性**：玩家在 JEI 中悬停物品并按 `R`/`U` 时，会看到 JER 新增的世界生成、生物掉落、植物、村民交易等标签页。
- **交互**：
  - 查看矿石在世界中的分布高度与概率
  - 查看生物掉落物品与概率
  - 查看村民职业交易列表
  - 查看地牢/结构战利品表
- **进度/奖励**：无直接进度关联。它是玩家规划资源获取路线的重要工具。
- **摩擦**：无额外摩擦。信息集成在熟悉的 JEI 界面中。
- **失败状态**：`diyData=true` 模式下若未正确扫描世界，生成数据可能为空或不准确。

## 集成关系

- **依赖**：Minecraft `[1.21,1.21.1)`、NeoForge（required, BOTH）；JEI `>=16.0.0`（required, BOTH）
- **冲突**：当前批次中无已知硬冲突。
- **协作**：作为 JEI 插件存在；JER Integration 为其提供与 Create、Thermal、IE 等模组的扩展集成。
- **配方/标签**：无直接参与
- **任务**：无
- **世界生成**：不修改世界生成，仅读取并展示生成数据
- **UI**：在 JEI 配方查看器中新增多个信息标签页
- **资源**：无
- **脚本**：无 KubeJS 脚本交互需求
- **加载顺序**：需在 JEI 之后加载

## 配置意图

`config/jeresources-common.toml`：

```toml
itemsPerColumn = 4
itemsPerRow = 4
diyData = true
showDevData = false
enchantsBlacklist = ["flimflam", "soulBound"]
hiddenTabs = []
dimensionsBlacklist = [-11]
disableLootManagerReloading = false
```

- **`itemsPerColumn = 4` / `itemsPerRow = 4`**：JER 信息网格使用 4×4 布局，与 JEI 默认物品列表密度保持一致。
- **`diyData = true`**：启用 DIY 数据模式，允许从当前世界扫描生成数据。这意味着首次进入新世界时会进行扫描，确保数据与当前数据包/模组组合一致。
- **`showDevData = false`**：不显示开发者数据，保持界面简洁。
- **`enchantsBlacklist`**：默认黑名单隐藏 `flimflam` 与 `soulBound` 附魔信息（来自特定模组），当前这些模组未安装，属于前向兼容默认值。
- **`dimensionsBlacklist = [-11]`**：将维度 ID `-11`（通常对应某些自定义维度或未知维度）排除在生成数据显示之外，避免显示无意义数据。
- **`disableLootManagerReloading = false`**：保持战利品表重载行为，确保 `/reload` 后 JER 数据同步。

## 兼容性与性能

- **客户端/服务端范围**：标记为 BOTH，但主要功能在 CLIENT 侧。服务端安装可支持 DIY 数据扫描与部分服务端数据同步。
- **存档影响**：无直接存档影响。`diyData` 扫描结果可能存储在本地配置或世界配置中。
- **已知不兼容**：当前批次中无已知冲突。
- **资源成本**：DIY 数据扫描在首次进入世界或重载时可能产生短暂卡顿；扫描完成后无持续开销。
- **缓解措施**：若扫描时间过长，可考虑预生成 JER 数据文件或调整 `dimensionsBlacklist`。

## 验证

- [ ] 启动或加载测试
- [ ] 功能测试（JEI 中查看世界生成、生物掉落、村民交易）
- [ ] 多人游戏测试（DIY 数据扫描与服务端同步）
- [ ] 性能测试（扫描耗时）
- [ ] 与 JEI、JER Integration 的回归检查

## 风险与开放问题

1. **DIY 数据准确性**：`diyData=true` 依赖世界扫描，某些模组的自定义生成逻辑可能无法被正确识别。
2. **与未来内容模组的扩展**：JER Integration 已预留 Create、Thermal、IE 等模组的补丁；未来安装这些模组后需验证 JER 是否正确显示其资源信息。

## 历史

- 2026-07-16: 作为第二批基础模组与 QoL 扩展批次添加
