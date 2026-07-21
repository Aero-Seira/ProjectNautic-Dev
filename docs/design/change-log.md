# 设计变更记录

## 2026-07-21 - Base Pack 收尾：归档实机验证产生的最终变更，105 模组基线定稿

- 背景：维护者于当日 19:49 前后完成一次完整实机启动验证（各配置/光影/会话文件 mtime 一致印证），随后宣布当前整合包作为 **Base Pack** 收尾。
- 修改（实质 delta，已更新对应组件页历史）：
  - **GUI Tween 主开关 `enable` false → true**（`config/guitween-client.toml`，verified）：第六批遗留的"有意默认还是试用遗留"开放问题由实机行动裁决，组件状态 experimental → active；分组动画参数全部保持作者预设默认值
  - **汉化资源包内容更新**：`Minecraft-Mod-Language-Modpack-Converted-1.21.1.zip` 文件数 2157→2158、lang 文件 1096→1097（+734 字节），连续第三日刷新，覆盖新增模组译名（verified）
- 噪音项（仅记录，不建档）：`asyncparticles-mixin.properties`、`iris.properties`、`packetfixer.properties`、光影设置 txt 均为关闭游戏时的运行时重写（大小不变，仅时间戳/键序刷新，verified）；`usercache.json` 为登录会话缓存续期（expiresOn 2026-08-21），属运行数据
- 设计文档更新：[GUI Tween 组件页](components/aesthetic/guitween.md)（状态、配置意图、开放问题裁决、历史）、[汉化包组件页](components/resourcepacks/minecraft-mod-language-modpack-converted-1-21-1.md)（历史）、README（当前阶段标记 Base Pack 定稿、GUI Tween 条目与视听叠加约束同步）
- 设计影响：
  - Base Pack 内容基线定稿：105 个模组（NeoForge 96 + Fabric 8 格式）、1 个资源包、3 个光影包、165 个配置、9 项脚本内容，全部进入已接受清单，后续变更均从新基线起算
  - 视听叠加风险约束中"GUI Tween 关闭"的缓解假设失效，HUD/动画层全量生效状态下的低配实机回归优先级上升
  - 后续开发按设计支柱以"章节解锁"方式投放内容模组，不再直接扩充 Base Pack 本体
- 验证状态：7 项 pending 变更全部经本地文件核实；GUI Tween 启用后的动画叠加观感、Stylish Effects vs Inventory HUD+ 药水 HUD 二选一裁决仍待实机确认
- 剩余问题：继承第六批全部剩余问题（HUD 重叠裁决、MEED/JEED 重叠、Acedium 非 NVIDIA 行为、BetterDays 睡眠提示本地化等）；新增"GUI Tween 启用后动画叠加回归"一项

## 2026-07-21 - 加载第六批 HUD/性能/机制模组，首次落地机制改动并归档 4 个移除组件

- 新增模组 **21 个**，按设计角色分类：
  - **性能**：[EntityCulling](components/performance/entityculling.md) 1.10.5（异步路径追踪实体剔除，内嵌 TRansition/TRender）、[Acedium Sodiumized](components/performance/acedium.md) 0.4.1（Nvidium 延续，依赖 Sodium 0.8，NVIDIA mesh shader）、[Alternate Current](components/performance/alternate-current.md) 1.9.0（红石线 BFS 重写）、[Krypton FNP](components/performance/krypton-fnp.md) 0.2.28.1（网络栈优化）、[Noisium](components/performance/noisium.md) 2.7.0（世界生成噪声优化）、[PacketFixer](components/performance/packetfixer.md) 3.3.1（数据包容错）
  - **视听/HUD**：[GUI Tween](components/aesthetic/guitween.md) 3.3.7（全分组 GUI 动画，主开关默认关闭，experimental）、[Stylish Effects](components/aesthetic/stylisheffects.md) 21.1.3、[Enhanced Boss Bars](components/aesthetic/enhancedbossbars.md) 1.0.0、[Overflowing Bars](components/aesthetic/overflowingbars.md) 21.1.1、[Leave My Bars Alone](components/aesthetic/leavemybarsalone.md) 21.1.2、[Distinguished Potions](components/aesthetic/distinguishedpotions.md) 21.1.1
  - **工具/QoL**：[Pick Up Notifier](components/utility/pickupnotifier.md) 21.1.1、[Pixelshot](components/utility/pixelshot.md) 21.1.1、[MEED](components/utility/meed.md) 8.0.1（与 JEED 功能重叠待裁决）、[Inventory HUD+](components/utility/inventoryhud.md) 3.4.28、[Configured](components/utility/configured.md) 2.6.3、[Armor Statues](components/utility/armorstatues.md) 21.1.0、[Leaves Be Gone](components/utility/leavesbegone.md) 21.1.1
  - **内容/机制**：[BetterDays](components/content/betterdays.md) 3.3.6.3（时间流速/睡眠加速，昼夜速度保持 1.0）、[Brutal Respawn](components/content/brutal-respawn.md) 1.1（1 血 0 食重生，落地 [死亡惩罚调研](research/death-penalty-mods.md) 需求 1 结论）
- 移除模组 **4 个**（页面已归档至 `components/_archive/`，README 系统地图同步删除）：
  - SmoothGui → 由 [GUI Tween](components/aesthetic/guitween.md) 替代
  - EffectTimerPlus → 由 [Stylish Effects](components/aesthetic/stylisheffects.md) 替代（功能上位）
  - Tiny Item Animations → 物品缩放动画由 GUI Tween 与既有动画层覆盖（inferred）
  - Gnetum → 第五批加入仅一天即撤出；HUD 分帧收益未验证即遭遇 HUD 扩容，与 ImmediatelyFast `hud_batching`（本批亦关闭）同维度叠加，待单独评估（inferred）
- 废弃配置清理（用户明确要求，删除前已快速核对均为默认或无迁移价值）：`config/effecttimerplus.json`、`config/gnetum.json`、`config/smoothgui.json`、`config/tia-client.toml`；其中 `config/gnetum.json` 为本次首启新生成后随即删除，既不算新增也不算遗留
- 修改（实质 delta，已更新对应组件页历史）：
  - ImmediatelyFast `hud_batching` true → false（规避与新 HUD 层叠加）
  - ModernUI 增强 tooltip 关闭（`[tooltip] enable=false`，推断让位 Obscure Tooltips）
  - Smooth Scrolling 快捷栏/聊天平滑度收敛至 0~0.1、关闭快捷栏 rollover
  - Smooth Swapping 动画曲线首控制点 0.1875 → 0.1171875
- 噪音项（仅记录，不建档）：JEI 按存档生成的 `config/jei/world/local/新的世界/bookmarks.json` 与 `lookupHistory.json`；JEI `ingredient-list-mod-sort-order.ini` 因新模组加入变化（预期）；`config/spark/activity.json`、`usercache.json` 运行数据；AsyncParticles mixin 属性、modernfix-mixins、Iris `enableShaders`（本地开关 false→true）、语言资源包 zip、光影设置 txt 时间戳刷新
- 孤儿配置（无对应模组 jar，暂不建档，待确认来源）：`config/raritycore/` 下 6 个 json（对应 RarityCore 类稀有度模组，mods/ 中无此 jar，疑为试用残留）；`config/healthbars-client.toml`（Fuzs 风格血条模组配置，无对应 jar）；`config/catalogue.properties` 与 `catalogue_favourites.txt`（MrCrayfish Catalogue 模组菜单配置，本包装的是 Configured 而非 Catalogue）。归属已查明并计入对应组件页：`nvidium-config.json` 属 Acedium、`whitenoise-client.toml` 属 BetterDays 内嵌库、`transition.json`/`trender.json` 属 EntityCulling 内嵌库
- 设计影响：
  - HUD/血条体系大幅扩充，药水效果 HUD 出现 Stylish Effects vs Inventory HUD+ 的重叠，需二选一
  - 死亡惩罚机制落地（调研结论需求 1）；需求 2（连死改复活点 KubeJS 脚本）尚未落地，1 血 0 食配置下连死风险敞口存在
  - 性能矩阵补齐网络栈（Krypton FNP）、数据包容错（PacketFixer）、红石（Alternate Current）、世界生成噪声（Noisium）、实体剔除（EntityCulling）、渲染进阶（Acedium）六个维度；模组总数 105
  - BetterDays 是首个机制类模组（时间/睡眠），配置克制（昼夜 1.0，仅睡眠加速）
- 验证状态：新模组元数据均从 jar 内 neoforge.mods.toml 核实；启动测试、HUD 重叠裁决、联机回归、Noisium 同种子世界生成一致性验证均待完成
- 剩余问题：
  - Stylish Effects 与 Inventory HUD+ 药水 HUD 二选一
  - MEED 与 JEED 效果描述重叠的实机裁决
  - GUI Tween 主开关 `enable=false` 是有意默认还是试用遗留
  - Armor Statues 依赖的 `statuemenus` 提供者待启动日志确认
  - Acedium 在非 NVIDIA 环境的行为待验证
  - BetterDays 睡眠提示消息未本地化

## 2026-07-20 - 完成惜命机制（死亡惩罚）模组调研

- 新增 `docs/design/research/death-penalty-mods.md`：围绕"重生低血量/低饱食度惩罚"与"连续死亡改复活点防刷命"两个需求，核实了 12+ 个候选模组的 NeoForge 1.21.1 支持情况。
- 推荐结论：**Brutal Respawn 1.1**（需求 1，已验证 neoforge 1.21.1 构建）+ **KubeJS 脚本自实现连死改复活点**（需求 2，随机复活类模组均无 1.21.1 构建）；备选为全 KubeJS 自实现；Better Respawn / Corail Tombstone / PlayerRevive 等方向相反而不推荐。
- 设计影响：若采纳，将新增 1 个纯服务端模组与 1 个 KubeJS server script，符合"魔改即基础设施"与服务端/客户端分离支柱。
- 验证状态：候选模组版本与加载器经 Modrinth API / CurseForge 核实；Brutal Respawn 实际数值与脚本效果待实测。

## 2026-07-20 - 加载第五批性能与修复类更新

- 新增模组 **4 个**（均为 decce 作品）：
  - **性能**：Ixeris 4.5.2（输入缓冲与线程化事件轮询，clientSideOnly）、Async Logger 2.1.2（log4j2 异步化与过滤）、Gnetum 3.3.6（HUD 更新分帧）
  - **QoL**：Progress Peek 1.0.2（任务栏显示游戏加载进度）
- 升级替换（旧 jar 删除、历史合并入组件页）：Placebo 9.9.1 → 9.9.2、Rhino 2101.2.7-build.85 → 2101.2.8-build.91、ModernFix 5.27.15 → 5.27.20
- 新增配置：`asynclogger.toml`（全默认，预留过滤文件 `asynclogger/default.toml`）、`ixeris.toml`（`flexibleThreading=true`，实验性缓存不启用）、`progresspeek.toml`（默认启用）、`newvisualkeybing/keybind_profiles.json`（NewVisualKeybing 生成的新档案文件，profiles 为空，无新模组）
- 修改（实质 delta）：Obscure Tooltips 关闭 3D 盔甲预览（`armorPreviewEnabled=false`）；Smooth Scrolling 快捷栏 `smoothness` 0.5 → 0.1（回调优先响应速度）；Iris `enableShaders` true → false（本地状态，官方默认光影指向不变）
- 修改（噪音，仅记录）：AsyncParticles mixin 配置与 EuphoriaPatches 设置 txt 仅时间戳刷新；`sodium-options.json` 仅捐赠提示已读标记；语言资源包 zip 内容再次更新
- 设计文档更新：新建 4 个组件页（performance/ixeris、asynclogger、gnetum + utility/progresspeek）；更新 Rhino / ModernFix / Placebo / Iris / Obscure Tooltips / Smooth Scrolling / NewVisualKeybing / AsyncParticles / 语言包 / 光影页历史；README 系统地图、阶段描述与性能预算（约 90 模组）同步
- 设计影响：
  - 性能矩阵补齐输入管线（Ixeris）、HUD 分帧（Gnetum）、日志 I/O（Async Logger）三个维度，配合 HUD 模组众多的现状针对性明显
  - 模组数约 90，启动耗时上升，Progress Peek 缓解等待体验
  - Rhino 升级涉及 KubeJS 硬依赖链，需回归脚本系统
- 验证状态：已扫描整合包，新模组元数据均从 jar 内 mods.toml 核实；启动测试、HUD 分帧显示回归、KubeJS 脚本回归尚待完成
- 剩余问题：
  - Gnetum 的 HUD 分帧可能造成饥饿值/效果计时等 HUD 元素轻微跳变，需主观验证
  - Async Logger 异步写入在崩溃瞬间可能丢失日志尾部，排障时需知悉
  - Iris `enableShaders=false` 是本地状态还是有意默认待定（若作为整合包默认需在说明中告知玩家如何开启）

## 2026-07-19 - 加载第四批视觉与协作体验模组，确立官方默认光影

- 新增模组 **14 个**，按设计角色分类：
  - **视觉/音效/视角**：Euphoria Patcher 1.9.3、Entity Texture Features 7.1、Presence Footsteps [FORGE] 1.0.0（非官方 NeoForge 移植）、Shoulder Surfing Reloaded 5.0.7、Obscure Tooltips 4.2.2
  - **多人协作 / QoL**：Ping Wheel 1.12.2、Resourcify 1.8.5、Polymorph 1.1.0+1.21.1
  - **集成扩展**：Ping to Map 1.1.0（Ping Wheel → Xaero 临时路径点）、SSR Camera Fixes 2.0.0（越肩视角战斗兼容，Epic Fight 等未到位，处前向兼容空窗期）
  - **依赖库**：Fragmentum 2.2.4（Obscure Tooltips 前置）、Iceberg 1.3.2、Prism 1.0.11（Grend 系库，暂无包内下游，引入动机待确认）
- 新增光影：**Complementary Unbound r5.8.1 + EuphoriaPatches 1.9.3**（基础 zip + 补丁衍生目录 + 设置 txt），`iris.properties` 已预置为默认 `shaderPack`——官方默认视觉基调首次确立
- 升级替换（旧 jar 删除、历史合并入组件页）：JEI 19.38.0.366 → 19.39.0.368、Xaero's Minimap 26.3.0 → 26.4.2、Xaero's World Map 1.43.0 → 1.44.2
- 修改：Placebo 9.9.1 jar 刷新（无版本 delta）、Smooth Scrolling 快捷栏 `smoothness` 0.2 → 0.5、AsyncParticles mixin 配置重新生成（仅时间戳）、语言资源包内容更新、Xaero minimap profile 文件（属 `config/xaero/` 个人数据，git 忽略）
- 新增配置：ETF、Euphoria Patcher、Obscure Tooltips、Ping to Map、Ping Wheel（客户端+服务端限流）、Presence Footsteps（关闭内置更新检查）、Resourcify（Modrinth 默认源）、Shoulder Surfing、SSR Camera Fixes
- 设计文档更新：新建 14 个组件页 + 1 个光影页；更新 JEI / Xaero 两图 / Iris / Smooth Scrolling / AsyncParticles / Placebo / 语言包页面；README 系统地图、性能预算（约 85 模组）、着色器与依赖链约束同步
- 设计影响：
  - 视觉基线从"无预置光影"升级为"官方默认光影 + 实体纹理增强 + 风格化 tooltip"，视觉风格统一支柱落地
  - 多人协作闭环建立：Ping Wheel（标记）→ Ping to Map（沉淀为 Xaero 临时路径点）
  - 新增依赖链：Fragmentum → Obscure Tooltips、Shoulder Surfing → SSR Camera Fixes、Ping Wheel → Ping to Map
  - 视听叠加风险进一步上升（光影 + ETF + 脚步声 + tooltip 动画），低配验证紧迫性提高
- 验证状态：已扫描整合包，新增模组元数据均从 jar 内 mods.toml 核实；启动、多人 ping 联动、光影帧率测试尚待完成
- 剩余问题：
  - Prism / Iceberg 暂无包内下游模组，引入动机待确认（可考虑移除或为 Grend 系模组预留）
  - 光影设置 txt 随包分发与玩家本地修改的升级冲突策略未定
  - Presence Footsteps 为非官方移植，长期维护性待观察
  - SSR Camera Fixes 对应的战斗模组（Epic Fight 等）尚未安装，处于空窗期
  - 仓库根目录存在若干研究笔记 md（结构修复/地牢/机制保护等主题），未纳入 docs/design，是否归档待决定

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

