# ProjectNautic 设计文档

## 产品定义

- Minecraft 版本：1.21.1
- 模组加载器：NeoForge 21.1.235（FML 4.0.42）
- Java 版本：21
- 目标玩家：待定义
- 核心体验：待定义
- 当前阶段：基础性能模组与魔改框架已加载，首批 QoL 与脚本基础设施就位

## 设计支柱

1. **长线运营优先**：整合包以服务器长期运营为首要目标，所有设计决策须考虑版本迭代兼容性、存档连续性、玩家进度迁移成本。
2. ** Vanilla+ 起步，阶段式解锁**：第一版贴近原版生存体验，但通过 Chapters + KubeJS 预留完整的阶段解锁框架。后续大版本更新以"解锁新内容章节"形式投放，而非重置世界。
3. **魔改即基础设施**：KubeJS 7.0 是核心魔改框架，配方、事件、自定义物品、阶段控制全部脚本化。魔改脚本须具备版本间可迁移性。
4. **服务端与客户端同源**：所有模组选择必须同时支持服务端与客户端部署，配置与脚本通过版本控制统一管理，支持热更新。

## 系统地图

| 组件 | 作用 | 状态 | 关键依赖 |
| --- | --- | --- | --- |
| [运行平台与基础配置](components/platform/platform-runtime.md) | 固定 Minecraft、NeoForge 与 Java 运行基线 | active | Minecraft 1.21.1、NeoForge 21.1.235、Java 21 |
| [调研报告：长线运营方案](research-report-v1.md) | 1.21.1 NeoForge 生态调研、魔改框架选型、版本更新机制设计 | completed | KubeJS 7.0、Chapters、FTB 生态 |
| [Sodium](components/performance/sodium.md) | 渲染引擎优化，提升帧率与减少卡顿 | active | CLIENT；与 Embeddium 不兼容 |
| [Lithium](components/performance/lithium.md) | 游戏逻辑与物理模拟优化 | active | BOTH |
| [ImmediatelyFast](components/performance/immediatelyfast.md) | 立即模式渲染批处理优化 | active | CLIENT |
| [Iris](components/aesthetic/iris.md) | 着色器模组，兼容 OptiFine 着色器包 | active | CLIENT；与 Embeddium 不兼容 |
| [ModernUI](components/aesthetic/modernui.md) | 现代化 UI 框架与字体渲染 | active | BOTH |
| [JEI](components/utility/jei.md) | 物品与配方查看 | active | CLIENT；KubeJS 可选集成 |
| [Jade](components/utility/jade.md) | 方块与实体信息提示（Hwyla 分支） | active | BOTH；KubeJS 可选集成 |
| [KubeJS](components/content/kubejs.md) | 脚本系统，自定义配方、事件、内容 | active | BOTH；依赖 Rhino |
| [KubeJS Additions](components/integration/kubejsadditions.md) | KubeJS 扩展功能，补充事件与 API | active | BOTH；依赖 KubeJS |
| [Rhino](components/library/rhino.md) | Mozilla Rhino JS 运行时，KubeJS 核心依赖 | active | BOTH |

## 跨系统约束

- 兼容性：新增模组应明确支持 Minecraft 1.21.1 与 NeoForge 21.1.x。
- 客户端/服务端范围：当前目录是客户端实例；加入联机或服务端内容时需要单独核对服务端依赖与配置同步方式。所有魔改脚本（KubeJS）须兼容服务端热重载。
- 存档兼容性：尚无玩法组件，后续加入世界生成、注册表内容或数据包时必须记录移除风险。新模组加入应尽量不影响已生成区块。
- 性能预算：当前 10 个模组，其中 3 个渲染优化（Sodium、ImmediatelyFast、Iris）、1 个逻辑优化（Lithium），构成性能基线。后续引入大型内容模组时需补充启动耗时与内存影响。参考基准：轻量包 <50 模组、中量包 50-200 模组、服务端内存预算 6-8GB。
- 阶段锁定：所有新增内容模组必须在 Chapters 阶段系统中定义默认解锁状态，确保未来可通过阶段进行内容开关。
- 着色器兼容性：Iris 已加载，后续渲染相关模组须验证与 Sodium + Iris 的兼容性，避免引入与 Embeddium 相关的冲突模组。
- 脚本依赖链：KubeJS → Rhino 为硬依赖；KubeJS Additions → KubeJS 为硬依赖。脚本系统面向未来所有内容魔改。

## 开放问题

- 整合包的核心主题、目标玩家与预期游戏时长是什么？
- 是否需要可独立部署的服务端版本？
- 性能下限与目标硬件配置是什么？
- 第一版（V1.0）模组清单是否确认？需进行实际兼容性测试。
- Alex's Mobs、Thermal Expansion、Mystical Agriculture 等模组的 1.21.1 NeoForge 可用性待最终确认。

## 自动生成资料

- [当前内容清单](_generated/current-inventory.md)
- [运行环境识别结果](_generated/pack-profile.md)
- [待整理变更](_generated/pending-changes.md)

