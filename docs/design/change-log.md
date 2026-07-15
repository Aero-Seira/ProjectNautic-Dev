# 设计变更记录

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

