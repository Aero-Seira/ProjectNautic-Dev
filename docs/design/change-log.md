# 设计变更记录

## 2026-07-16 - 加载第三批 QoL、本地化与视听反馈模组

- 新增模组 **约 41 个**（第三批），按设计角色分类：
  - **性能优化**：BadOptimizations 2.4.1、FastSuite 6.0.7、Flerovium 1.1.2、Cupboard 3.8
  - **视觉/粒子/音频/动画**：Ambiance 1.1.0、AmbientSounds 6.3.8、Visuality: Reforged 2.1.0、Particular 1.5.5、Effectual 1.4.0、Particle Effects 1.5.0、Inventory Particles 2.6.0、Extra Sounds 1.5.2、Tiny Item Animations 1.2.1、Smooth Swapping 0.9.3.2、SmoothGui 2.0.1、Smooth Scrolling 1.0.1、Chat Impressive Animation 1.6.0
  - **聊天/社交**：No Chat Reports 2.9.1、Chat Heads 0.15.2、Beautified Chat Server 3.2、ChatNotify 2.6.9
  - **信息/便利**：Better Advancements 0.4.3.21、Clickable Advancements 3.8、Effect Descriptions 21.1.1、EffectTimerPlus 2.2.7、Screenshot to Clipboard 1.0.10、I18nUpdateMod 3.7.0、Just Enough Characters 4.5.26
  - **依赖库**：Architectury 13.0.8、Cloth Config v15 15.0.140、Collective 8.39、Configuration 3.1.1、Fzzy Config 0.7.6、Kotlin for Forge 5.12.0、MossyLib 1.5.0、Placebo 9.9.1、Puzzles Lib 21.1.52、TLib 1.5.0、YetAnotherConfigLib 3.8.2
  - **集成/其他**：AsyncParticles 21.1.2.0
- 新增资源包：**Minecraft Mod Language Modpack Converted 1.21.1**（社区简体中文翻译包）
- 新增配置文件：所有新增模组生成的默认/初始配置，包括 NoChatReports 系列、Visuality 粒子发射器、AmbientSounds 声景、Particular 环境粒子、FastSuite 并行配方、Flerovium 剔除选项等
- 设计文档更新：
  - 更新 `README.md` 产品阶段、系统地图（新增 41+ 组件条目）、性能预算、跨系统约束
  - 创建 41 个组件文档（按类别分目录存放），覆盖新增模组与资源包
- 设计影响：
  - 整合包从"轻量包"进入"中量包"区间，模组总数约 70
  - 中文玩家体验基线建立（自动汉化更新 + 社区汉化包 + JEI 拼音搜索）
  - 聊天/社交体验完整（禁用举报、头像、服务器格式、关键词通知）
  - 信息查询与进度体验增强（Better Advancements、Clickable Advancements、效果说明/计时器）
  - 环境视听反馈层完整，但多个粒子/音效模组叠加带来低配设备性能与感知噪音风险
  - 底层性能优化进一步扩展（配方并行、剔除、微优化），与现有 Sodium/Lithium/ModernFix 形成互补
- 验证状态：已扫描整合包，所有新增模组元数据、配置文件与资源包已记录；实际启动测试、低配性能测试、多人聊天/社交行为验证尚待完成。
- 剩余问题：
  - 多个视听模组在低配硬件上的叠加性能影响需实测并制定关闭建议
  - I18nUpdateMod 与 Minecraft Mod Language Modpack 的加载顺序/覆盖关系需向玩家明确
  - 部分粒子模组（Effectual / Particular / Visuality / Ambiance）在火源、灵魂沙、洞穴尘埃等场景存在效果重叠风险
  - FastSuite 对 KubeJS 动态配方与自定义配方线程安全性的影响需回归验证

## 2026-07-16 - 加载第二批基础模组与 QoL 扩展

- 新增模组 **20 个**：
  - **性能优化**：C2ME 0.4.0-alpha.0.115、ModernFix 5.27.15、Ferrite Core 7.0.3、CullLeaves 4.1.1、Sodium Extra 0.9.3、ScalableLux 0.3.0-alpha.0.6
  - **视觉/UI**：NewVisualKeybing 0.6.16
  - **QoL 工具**：Mouse Tweaks 2.26.1、AppleSkin 3.0.9、Just Enough Resources 1.6.0.17、Just Enough Effects Descriptions 2.3.2、Xaero's Minimap 26.3.0、Xaero's World Map 1.43.0、Chunky 1.4.23、spark 1.10.124
  - **集成扩展**：Jade Addons 6.1.0、JER Integration 6.5.0、KubeJS Data Component 1.0.1、LootJS 3.7.0
  - **依赖库**：MidnightLib 1.9.3
- 新增配置文件：C2ME、ModernFix、Ferrite Core、CullLeaves、Sodium Extra、NewVisualKeybing、Mouse Tweaks、AppleSkin、JER、JEED、JER Integration、Xaero 系列、Chunky、spark、MidnightLib 等模组配置
- 设计文档更新：
  - 更新 `README.md` 系统地图，添加 20 个新组件条目
  - 更新性能预算说明，完整覆盖渲染/HUD/逻辑/区块/光照/内存/树叶剔除维度
  - 更新脚本依赖链，纳入 KubeJS Data Component 与 LootJS
  - 创建 20 个组件文档：`c2me.md`、`modernfix.md`、`ferritecore.md`、`cullleaves.md`、`sodium-extra.md`、`scalablelux.md`、`newvisualkeybing.md`、`mousetweaks.md`、`appleskin.md`、`jeresources.md`、`jeed.md`、`xaerominimap.md`、`xaeroworldmap.md`、`chunky.md`、`spark.md`、`jadeaddons.md`、`jerintegration.md`、`kubejs-datacomponent.md`、`lootjs.md`、`midnightlib.md`
- 设计影响：
  - 性能优化矩阵已完整，覆盖渲染、逻辑、区块、光照、内存与客户端剔除
  - QoL 信息查询体系建立（JEI + JER + JEED + Jade），为后续内容模组提供信息展示基础
  - 导航与运维工具就位（Xaero 地图、Chunky、spark），支撑长期服务器运营
  - KubeJS 魔改生态扩展至 DataComponent 与战利品表，为阶段化内容与平衡调整提供基础设施
  - 多个集成模组（Jade Addons、JER Integration）作为前向兼容钩子，为未来的 Create、Thermal、Tinkers' 等内容模组预留集成点
- 验证状态：已扫描整合包，所有新增模组元数据与配置文件已记录；实际兼容性、性能测试与多人验证尚待完成。
- 剩余问题：
  - C2ME 与 ScalableLux 为 alpha 版本，需密切关注稳定性更新
  - Xaero 地图缓存与 HUD 布局需在实际多人环境中验证
  - KubeJS Data Component 与 LootJS 的脚本 API 覆盖度需在后续魔改开发中验证
  - 前向兼容的集成模组（Jade Addons、JER Integration）在目标内容模组未安装时处于空窗期，需向玩家/运营者说明

## 2026-07-15 - 加载首批基础模组与魔改框架

- 新增模组 **10 个**：
  - **性能渲染**：Sodium 0.8.12、Lithium 0.15.4、ImmediatelyFast 1.6.11
  - **着色器与 UI**：Iris 1.8.14-beta.1、ModernUI 3.13.0.1
  - **QoL 工具**：JEI 19.38.0.366、Jade 15.10.5
  - **魔改框架**：KubeJS 2101.7.2-build.368、KubeJS Additions 1.21.1-6.0.0、Rhino 2101.2.7-build.85
- 新增脚本内容：KubeJS 默认目录结构（`startup_scripts/`、`server_scripts/`、`client_scripts/`、`assets/`、`config/`）
- 新增配置文件：各模组的默认配置文件共 30 项（详见 `current-inventory.md`）
- 设计文档更新：
  - 更新 `README.md` 系统地图，添加 10 个组件条目
  - 更新设计支柱为"性能优先、可扩展性、UI 一致性"
  - 更新跨系统约束（性能预算、着色器兼容性、脚本依赖链）
  - 创建 10 个组件文档：`sodium.md`、`lithium.md`、`immediatelyfast.md`、`iris.md`、`modernui.md`、`jei.md`、`jade.md`、`kubejs.md`、`kubejsadditions.md`、`rhino.md`
- 设计影响：
  - 性能基线已建立（Sodium + Lithium + ImmediatelyFast），后续内容模组需在此基线上评估启动与内存影响
  - 魔改基础设施就位（KubeJS + Rhino + Additions），后续所有内容魔改均通过脚本系统实现
  - 着色器支持已启用（Iris），视觉风格可通过着色器包统一调整
- 验证状态：已扫描整合包，所有模组元数据与配置文件已记录；客户端成功启动及实际性能测试尚待验证。
- 剩余问题：
  - 需确认首批模组在目标硬件上的启动耗时与内存占用
  - Iris beta 版本稳定性待观察
  - 着色器包选择尚未确定
  - KubeJS 脚本尚未编写，魔改能力仅停留在框架层面

## 2026-07-15 - 完成长线运营整合包方案调研

- 新增：`docs/design/research-report-v1.md`，涵盖 1.21.1 NeoForge 生态全面调研。
- 魔改框架选型：**KubeJS 7.0** 作为主力魔改引擎，**LootJS**、**KubeJS Additions**、**KubeJS Data Component** 作为核心 addon。
- 阶段系统选型：**Chapters** 作为 GameStages 的现代继任者，支持 item/fluid/chemical/recipe/mod 级别锁定，原生支持 KubeJS、FTB Teams/Quests、JEI。
- FTB 生态确认：FTB Library、FTB Teams、FTB Quests、FTB Chunks、FTB Quests Optimizer 均支持 1.21.1 NeoForge。
- 版本管理：确认 **VersionerReborn** 可用于整合包版本检查与更新通知。
- 内容模组生态：梳理了科技线（Create/Mekanism/AE2 等）、魔法线（Ars Nouveau/Iron's Spells/Occultism 等）、冒险线（Ice and Fire/Cataclysm/Aether 等）的可用性与投放建议。
- 设计支柱更新：确立了"长线运营优先、Vanilla+ 起步阶段式解锁、魔改即基础设施、服务端客户端同源"四大支柱。
- 开放问题更新：补充了第一版模组清单确认、特定模组可用性待核实等项。
- 验证状态：调研基于 CurseForge/Modrinth/KubeJS Wiki/MC百科 等公开信息，实际模组兼容性待本地测试。

## 2026-07-15 - 添加 Git 仓库管理规则

## 2026-07-15 - 添加 Git 仓库管理规则

- 新增：`.gitignore` 文件，定义整合包仓库的忽略规则。
- 忽略范围：运行时日志、下载缓存、玩家存档、本地库文件（`.dll`）、PCL 启动器个人配置、游戏主 Jar、系统临时文件等。
- 保留范围：`config/`、`defaultconfigs/`、`mods/`、`resourcepacks/`、`docs/`、`ProjectNautic-Dev.json` 及设计文档。
- 设计影响：统一团队与 CI 的仓库边界，避免个人运行时数据混入版本控制。
- 验证状态：已扫描现有目录结构，规则与当前文件布局匹配。

## 2026-07-14 - 建立设计文档与运行基线

- 新增：整合包设计文档、组件文档和自动生成的内容清单。
- 已识别：Minecraft 1.21.1、NeoForge 21.1.235、FML 4.0.42、Java 21。
- 设计影响：后续内容必须以该运行平台为兼容性基线；当前尚无玩法模组或资源内容。
- 验证状态：本地元数据与配置文件已检查，客户端成功启动尚待验证。
- 剩余问题：核心主题、目标玩家、服务端范围和性能目标尚未定义。

