# ProjectNautic 设计文档

## 产品定义

- Minecraft 版本：1.21.1
- 模组加载器：NeoForge 21.1.235（FML 4.0.42）
- Java 版本：21
- 目标玩家：待定义
- 核心体验：待定义
- 当前阶段：基础性能模组、魔改框架、扩展 QoL 工具与集成插件已加载；第三批以中文本地化、聊天/社交、信息展示、环境视听与底层性能优化为主的 QoL 模组已就位；第四批确立官方默认光影（Complementary + EuphoriaPatches）并引入多人协作标记、越肩视角、实体纹理与 tooltip 视觉增强；第五批补充输入/日志维度性能优化（Ixeris、Async Logger）与启动体验 QoL（Progress Peek，Gnetum 已于第六批撤出）；第六批扩容 HUD/血条体系（Stylish Effects、Inventory HUD+、Enhanced Boss Bars、Overflowing Bars、Leave My Bars Alone、Pick Up Notifier、Configured、Armor Statues、Pixelshot、Distinguished Potions、MEED、GUI Tween）、补齐性能矩阵网络/红石/世界生成/实体剔除维度（Krypton FNP、PacketFixer、Alternate Current、Noisium、EntityCulling、Acedium）并首次落地机制类模组（BetterDays 时间/睡眠、Brutal Respawn 死亡惩罚）

## 设计支柱

1. **长线运营优先**：整合包以服务器长期运营为首要目标，所有设计决策须考虑版本迭代兼容性、存档连续性、玩家进度迁移成本。
2. **类原版起步，阶段式解锁**：第一版为"类原版"体验——不新增任何模组物品、方块、实体等注册表内容，但允许机制优化与 QoL 模组（如红石计算优化、渲染/性能优化、界面增强）存在；此类模组可改变原版的性能表现或交互感受，但不得向玩家暴露新的可获得内容。通过 Chapters + KubeJS 预留完整的阶段解锁框架，后续大版本更新以"解锁新内容章节"形式投放，而非重置世界。
3. **魔改即基础设施**：KubeJS 7.0 是核心魔改框架，配方、事件、自定义物品、阶段控制全部脚本化。魔改脚本须具备版本间可迁移性。
4. ~~**服务端与客户端同源**：所有模组选择必须同时支持服务端与客户端部署，配置与脚本通过版本控制统一管理，支持热更新。~~
5. **更正第4条**：服务端与客户端模组不完全相同，部分可仅服务端模组应仅存在服务端中，且服务端中绝对不能出现仅客户端模组。

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
| [C2ME](components/performance/c2me.md) | 并发区块管理引擎，优化区块加载与生成 | active | BOTH；alpha 版本 |
| [ModernFix](components/performance/modernfix.md) | 综合性能与启动优化 | active | BOTH |
| [Ferrite Core](components/performance/ferritecore.md) | 内存占用优化 | active | BOTH |
| [CullLeaves](components/performance/cullleaves.md) | 树叶渲染剔除优化 | active | CLIENT；依赖 MidnightLib |
| [Sodium Extra](components/performance/sodium-extra.md) | Sodium 扩展设置与 FPS 覆盖层 | active | CLIENT；依赖 Sodium |
| [ScalableLux](components/performance/scalablelux.md) | 光照引擎重写，修复光照性能与错误 | active | BOTH；alpha 版本 |
| [NewVisualKeybing](components/aesthetic/newvisualkeybing.md) | 键位可视化面板 | active | CLIENT |
| [Mouse Tweaks](components/utility/mousetweaks.md) | 鼠标手势库存管理 | active | CLIENT |
| [AppleSkin](components/utility/appleskin.md) | 食物与饥饿 HUD 信息增强 | active | CLIENT |
| [Just Enough Resources](components/utility/jeresources.md) | JEI 资源与世界生成信息扩展 | active | BOTH；依赖 JEI |
| [Just Enough Effects Descriptions](components/utility/jeed.md) | JEI 状态效果描述扩展 | active | BOTH；依赖 JEI |
| [Xaero's Minimap](components/utility/xaerominimap.md) | 小地图与导航 | active | CLIENT；可选 xaeroworldmap |
| [Xaero's World Map](components/utility/xaeroworldmap.md) | 全屏世界地图 | active | CLIENT；可选 xaerominimap |
| [Chunky](components/utility/chunky.md) | 区块预生成管理工具 | active | BOTH |
| [spark](components/utility/spark.md) | 性能剖析与诊断工具 | active | BOTH |
| [Jade Addons](components/integration/jadeaddons.md) | Jade 提示扩展（Create/Tinkers'/Lootr 等） | active | BOTH；依赖 Jade |
| [JER Integration](components/integration/jerintegration.md) | JER 与科技模组集成补丁 | active | CLIENT；依赖 JER |
| [KubeJS Data Component](components/integration/kubejs-datacomponent.md) | KubeJS DataComponent API 扩展 | active | BOTH；依赖 KubeJS |
| [LootJS](components/integration/lootjs.md) | KubeJS 战利品表修改扩展 | active | BOTH；依赖 KubeJS |
| [MidnightLib](components/library/midnightlib.md) | 轻量级配置库，CullLeaves 依赖 | active | BOTH |
| [BadOptimizations](components/performance/badoptimizations.md) | 小型渲染/逻辑微优化集合 | active | CLIENT |
| [FastSuite](components/performance/fastsuite.md) | 配方匹配并行化，降低自动合成 tick 压力 | active | BOTH；依赖 Placebo |
| [Flerovium](components/performance/flerovium.md) | 实体/地形渲染剔除与计算 shortcut | active | CLIENT |
| [Cupboard](components/performance/cupboard.md) | 底层修复与诊断工具 | active | BOTH |
| [Ambiance](components/aesthetic/ambiance.md) | 方块/物品/实体环境氛围粒子与音效 | active | CLIENT |
| [AmbientSounds](components/aesthetic/ambientsounds.md) | 程序化环境声景 | active | CLIENT；依赖 CreativeCore |
| [Visuality: Reforged](components/aesthetic/visuality.md) | 方块/实体接触点视觉粒子 | active | CLIENT |
| [Particular](components/aesthetic/particular.md) | 瀑布、落叶、萤火虫等大尺度环境粒子 | active | CLIENT |
| [Effectual](components/aesthetic/effectual.md) | 玩家状态与环境交互反馈粒子 | active | CLIENT |
| [Particle Effects](components/aesthetic/particle-effects.md) | 粒子效果配置库 | active | CLIENT；Inventory Particles 依赖 |
| [Inventory Particles](components/aesthetic/inventory-particles.md) | 物品栏物品移动粒子反馈 | active | CLIENT；依赖 Particle Effects |
| [Extra Sounds](components/aesthetic/extrasounds.md) | UI/交互音效扩展 | active | CLIENT |
| [Smooth Swapping](components/aesthetic/smoothswapping.md) | 物品栏物品平滑位移动画 | active | CLIENT |
| [Smooth Scrolling Reforged](components/aesthetic/smoothscrolling.md) | 快捷栏/聊天/列表平滑滚动 | active | CLIENT |
| [CreativeCore](components/aesthetic/creativecore.md) | CreativeMD 系列共享库 | active | BOTH；AmbientSounds 依赖 |
| [Chat Impressive Animation](components/aesthetic/chatimpressiveanimation.md) | 聊天栏/消息发送动画 | active | CLIENT |
| [No Chat Reports](components/utility/nochatreports.md) | 禁用微软聊天举报系统 | active | BOTH |
| [Chat Heads](components/utility/chat-heads.md) | 聊天框显示玩家头像 | active | CLIENT |
| [Beautified Chat Server](components/utility/beautifiedchatserver.md) | 服务端聊天格式与等级前缀 | active | BOTH；依赖 Collective |
| [ChatNotify](components/utility/chatnotify.md) | 聊天关键词高亮与通知 | active | CLIENT |
| [Better Advancements](components/utility/betteradvancements.md) | 改进进度界面可读性 | active | CLIENT |
| [Clickable Advancements](components/utility/clickadv.md) | 聊天中点击进度名称 | active | CLIENT |
| [Effect Descriptions](components/utility/effectdescriptions.md) | 物品提示中显示状态效果说明 | active | CLIENT |
| [Screenshot to Clipboard](components/utility/screenshotclipboard.md) | 截图直接复制到剪贴板 | active | CLIENT |
| [I18nUpdateMod](components/utility/i18nupdatemod.md) | 自动汉化更新 | active | CLIENT |
| [Just Enough Characters](components/utility/jecharacters.md) | JEI 拼音搜索 | active | CLIENT；依赖 JEI |
| [Architectury](components/library/architectury.md) | 多平台抽象库 | active | BOTH |
| [Cloth Config API](components/library/cloth-config.md) | 配置界面 API | active | CLIENT；大量模组的配置屏幕依赖 |
| [Collective](components/library/collective.md) | Serilum 系列共享库 | active | BOTH；Beautified Chat Server 依赖 |
| [Configuration](components/library/configuration.md) | 配置库 | active | BOTH |
| [Fzzy Config](components/library/fzzy-config.md) | 配置库与键位绑定 | active | CLIENT |
| [Kotlin for Forge](components/library/kotlinforforge.md) | Kotlin 运行时 | active | BOTH |
| [MossyLib](components/library/mossylib.md) | MossyMods 共享库 | active | BOTH |
| [Placebo](components/library/placebo.md) | Shadows_of_Fire 系列共享库 | active | BOTH；FastSuite 依赖 |
| [PuzzlesLib](components/library/puzzleslib.md) | Fuzs 系列共享库 | active | BOTH |
| [TLib](components/library/tlib.md) | TLMods 共享库 | active | BOTH |
| [YetAnotherConfigLib](components/library/yet-another-config-lib-v3.md) | 配置界面库 | active | CLIENT |
| [AsyncParticles](components/integration/asyncparticles.md) | 粒子 tick 异步化 | active | CLIENT |
| [Minecraft Mod Language Modpack](components/resourcepacks/minecraft-mod-language-modpack-converted-1-21-1.md) | 社区简体中文翻译资源包 | active | CLIENT |
| [Ixeris](components/performance/ixeris.md) | 输入缓冲与线程化事件轮询 | active | CLIENT |
| [Async Logger](components/performance/asynclogger.md) | 日志异步化与过滤 | active | BOTH |
| [Progress Peek](components/utility/progresspeek.md) | 任务栏显示游戏加载进度 | active | CLIENT |
| [Euphoria Patcher](components/aesthetic/euphoria-patcher.md) | 游戏内应用 Euphoria Patches 光影补丁 | active | CLIENT；依赖 Iris |
| [Complementary + EuphoriaPatches 光影](components/aesthetic/complementary-euphoria-shaders.md) | 官方默认光影（Unbound r5.8.1 + EuphoriaPatches 1.9.3） | active | CLIENT；Iris 加载 |
| [Ping Wheel](components/utility/pingwheel.md) | 多人位置标记（ping）系统 | active | BOTH |
| [Ping to Map](components/integration/pingtomapxaeros.md) | Ping Wheel → Xaero 临时路径点联动 | active | CLIENT；依赖 Ping Wheel |
| [Resourcify](components/utility/resourcify.md) | 游戏内资源包/数据包/光影浏览器 | active | CLIENT |
| [Polymorph](components/utility/polymorph.md) | 合成配方冲突结果选择 | active | BOTH |
| [Entity Texture Features](components/aesthetic/entity-texture-features.md) | 实体随机/发光纹理与眨眼支持 | active | CLIENT |
| [Presence Footsteps](components/aesthetic/presencefootsteps.md) | 材质感知程序化脚步声 | active | CLIENT |
| [Shoulder Surfing Reloaded](components/aesthetic/shouldersurfing.md) | 越肩第三人称视角 | active | CLIENT |
| [SSR Camera Fixes](components/integration/ssrcamerafixes.md) | 越肩视角与战斗模组相机兼容扩展 | active | CLIENT；依赖 Shoulder Surfing |
| [Obscure Tooltips](components/aesthetic/obscure-tooltips.md) | 风格化物品提示框与 3D 盔甲预览 | active | CLIENT；依赖 Fragmentum |
| [Fragmentum](components/library/fragmentum.md) | Obscuria 系列共享框架 | active | BOTH；Obscure Tooltips 依赖 |
| [Iceberg](components/library/iceberg.md) | Grend 系模组工具库 | active | BOTH |
| [Prism](components/library/prism.md) | Grend 系颜色功能库 | active | BOTH |
| [EntityCulling](components/performance/entityculling.md) | 异步路径追踪实体/方块实体剔除 | active | CLIENT；内嵌 TRansition/TRender |
| [Acedium Sodiumized](components/performance/acedium.md) | Nvidium 延续，NVIDIA mesh shader 渲染加速 | active | CLIENT；依赖 Sodium 0.8 |
| [Alternate Current](components/performance/alternate-current.md) | 红石线信号 BFS 重写 | active | BOTH |
| [Krypton FNP](components/performance/krypton-fnp.md) | 网络栈优化（Netty/原生传输） | active | BOTH |
| [Noisium](components/performance/noisium.md) | 世界生成噪声优化 | active | BOTH |
| [PacketFixer](components/performance/packetfixer.md) | 数据包大小/连接问题修复 | active | BOTH |
| [GUI Tween](components/aesthetic/guitween.md) | 全分组 GUI 动画（替代 SmoothGui） | experimental | CLIENT；主开关默认关闭 |
| [Stylish Effects](components/aesthetic/stylisheffects.md) | 状态效果显示重做（替代 EffectTimerPlus） | active | CLIENT；依赖 PuzzlesLib |
| [Enhanced Boss Bars](components/aesthetic/enhancedbossbars.md) | 改进 Boss 血条 | active | BOTH |
| [Overflowing Bars](components/aesthetic/overflowingbars.md) | 生命/护甲条超限扩展显示 | active | CLIENT；依赖 PuzzlesLib |
| [Leave My Bars Alone](components/aesthetic/leavemybarsalone.md) | 骑乘时保留饥饿/经验条 | active | CLIENT；依赖 PuzzlesLib |
| [Distinguished Potions](components/aesthetic/distinguishedpotions.md) | 药水视觉区分强化 | active | BOTH；依赖 PuzzlesLib |
| [Pick Up Notifier](components/utility/pickupnotifier.md) | 拾取物品通知 | active | BOTH；依赖 PuzzlesLib |
| [Pixelshot](components/utility/pixelshot.md) | 高分辨率截图与正交相机 | active | CLIENT；依赖 PuzzlesLib |
| [MEED](components/utility/meed.md) | JEI 效果描述数据集（与 JEED 重叠待裁决） | active | BOTH |
| [Inventory HUD+](components/utility/inventoryhud.md) | 护甲耐久/药水/迷你物品栏常驻 HUD | active | CLIENT |
| [Configured](components/utility/configured.md) | 全模组游戏内配置 GUI | active | CLIENT；JEI 可选集成 |
| [Armor Statues](components/utility/armorstatues.md) | 盔甲架姿势编辑 | active | BOTH；依赖 PuzzlesLib |
| [Leaves Be Gone](components/utility/leavesbegone.md) | 砍树后树叶快速腐烂 | active | BOTH；依赖 PuzzlesLib |
| [BetterDays](components/content/betterdays.md) | 时间流速与睡眠加速机制 | active | BOTH；内嵌 whitenoise |
| [Brutal Respawn](components/content/brutal-respawn.md) | 死亡惩罚：低血/低饱食重生 | active | BOTH；依赖 YACL |

## 跨系统约束

- 兼容性：新增模组应明确支持 Minecraft 1.21.1 与 NeoForge 21.1.x。
- 客户端/服务端范围：当前目录是客户端实例；加入联机或服务端内容时需要单独核对服务端依赖与配置同步方式。所有魔改脚本（KubeJS）须兼容服务端热重载。
- 存档兼容性：尚无玩法组件，后续加入世界生成、注册表内容或数据包时必须记录移除风险。新模组加入应尽量不影响已生成区块。
- 性能预算：当前 105 个模组，处于"中量包"区间。性能优化矩阵已覆盖渲染（Sodium + Iris、Acedium mesh shader 进阶）、实体剔除（EntityCulling、Flerovium）、客户端 HUD（ImmediatelyFast；Gnetum 已于第六批移除且 `hud_batching` 暂时关闭）、逻辑（Lithium）、红石（Alternate Current）、配方匹配（FastSuite）、区块（C2ME）、世界生成噪声（Noisium）、光照（ScalableLux）、内存（Ferrite Core + ModernFix）、网络栈（Krypton FNP + PacketFixer）、树叶剔除（CullLeaves）、扩展渲染选项（Sodium Extra）、输入管线（Ixeris）、日志异步化（Async Logger）以及多项微优化（BadOptimizations、Cupboard）。后续引入大型内容模组时需重点补充启动耗时与内存影响。参考基准：轻量包 <50 模组、中量包 50-200 模组、服务端内存预算 6-8GB、客户端建议 4-6GB。
- 阶段锁定：所有新增内容模组必须在 Chapters 阶段系统中定义默认解锁状态，确保未来可通过阶段进行内容开关。
- 着色器兼容性：Iris 已加载并预置官方默认光影（Complementary Unbound r5.8.1 + EuphoriaPatches 1.9.3，经 Euphoria Patcher 游戏内补丁）。后续渲染相关模组须验证与该光影管线的兼容性，避免引入与 Embeddium 相关的冲突模组；光影设置文件随包分发，升级策略待明确。
- 脚本依赖链：KubeJS → Rhino 为硬依赖；KubeJS Additions、KubeJS Data Component、LootJS → KubeJS 为硬依赖。脚本系统面向未来所有内容魔改。
- 视听叠加风险：第三批同时引入 Ambiance、AmbientSounds、Particular、Visuality、Effectual、Inventory Particles、Extra Sounds、Smooth Swapping/Scrolling 等大量视听反馈模组，第四批又叠加官方光影、Entity Texture Features、Presence Footsteps、Obscure Tooltips 与越肩视角。第六批再叠加 HUD 层模组（Stylish Effects、Inventory HUD+、Enhanced Boss Bars、Overflowing Bars、Leave My Bars Alone、Pick Up Notifier、GUI Tween）——其中 Stylish Effects 与 Inventory HUD+ 的药水 HUD 功能重叠需二选一，GUI Tween 因叠加顾虑主开关默认关闭。单个效果开销小，但叠加后在低配设备上可能产生明显的帧率与感知噪音，需在目标硬件上验证并准备关闭清单。
- 中文本地化双轨：I18nUpdateMod 与 Minecraft Mod Language Modpack 同时提供中文翻译，二者加载顺序与覆盖关系会直接影响玩家看到的文本。需要明确默认推荐顺序并在说明文档中告知玩家。
- 聊天/社交模组耦合：No Chat Reports、Chat Heads、Beautified Chat Server、ChatNotify 共同作用于聊天系统；服务端配置（NCR、Beautified Chat Server）与客户端配置（Chat Heads、ChatNotify）需保持兼容，避免聊天显示异常或安全提示冲突。
- 库版本耦合：Placebo → FastSuite、Collective → Beautified Chat Server、CreativeCore → AmbientSounds、Particle Effects → Inventory Particles、Fragmentum → Obscure Tooltips、Shoulder Surfing → SSR Camera Fixes、Ping Wheel → Ping to Map 等依赖链已建立。升级库模组时必须同步验证下游模组兼容性。

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

