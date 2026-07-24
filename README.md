# Toudai · 灯台

> **Toudai（灯台）** 是 ProjectNautic 系列 Minecraft 项目的 **Basepack 基底整合包**。
> 如灯塔之于航船：它自身不装载游戏内容，而是为所有衍生包提供统一、稳定、可复用的技术与体验基座。

## 定位

Toudai 是一个"类原版"基础包——**暂未新增任何模组物品、方块、实体等注册表内容**，但预置完整的性能优化矩阵、魔改框架、QoL 工具与视听体验层。本项目衍生整合包（ProjectNautic 各章节内容包、服务端包等）都基于 Toudai 构建，通过 KubeJS + 阶段解锁框架在此基座上投放内容，而非重复搭建底层。

## 运行基线

| 项目 | 版本 |
| --- | --- |
| Minecraft | 1.21.1 |
| 模组加载器 | NeoForge 21.1.235（FML 4.0.42） |
| Java | 21 |
| 当前模组规模 | 105 个（Base Pack 基线已于 2026-07-21 归档接受） |
| 客户端内存建议 | 4–6 GB |
| 服务端内存预算 | 6–8 GB |

## 设计支柱

1. **长线运营优先** —— 面向服务器长期运营，重视版本迭代兼容性、存档连续性与玩家进度迁移成本。
2. **类原版起步，阶段式解锁** —— 基底包只允许机制优化与 QoL 模组存在，不向玩家暴露新的可获得内容；后续大版本以"解锁新内容章节"形式投放，而非重置世界。
3. **魔改即基础设施** —— KubeJS 7.0 是核心魔改框架，配方、事件、自定义物品、阶段控制全部脚本化，且须具备版本间可迁移性。
4. **客户端 / 服务端范围分明** —— 本仓库是客户端实例；仅服务端模组不应出现在客户端，服务端中绝对不能出现仅客户端模组。所有魔改脚本须兼容服务端热重载。

## 基座已覆盖的能力层

- **性能矩阵**：渲染（Sodium + Iris、Acedium mesh shader）、实体剔除（EntityCulling、Flerovium）、逻辑（Lithium）、红石（Alternate Current）、配方匹配（FastSuite）、区块（C2ME）、世界生成噪声（Noisium）、光照（ScalableLux）、内存（Ferrite Core + ModernFix）、网络栈（Krypton FNP + PacketFixer）、输入管线（Ixeris）、日志异步化（Async Logger）等。
- **魔改框架**：KubeJS 7.0 + Rhino + LootJS + KubeJS Additions / Data Component，脚本系统面向未来所有内容魔改。
- **信息展示与 QoL**：JEI / Jade / Xaero 地图系、汉化双轨（I18nUpdateMod + 社区汉化包）、拼音搜索、聊天与社交增强、HUD / 血条体系等。
- **视听体验**：Iris 光影管线（官方默认 Complementary Unbound + EuphoriaPatches）、环境粒子与声景、GUI 动画、越肩视角等。
- **诊断工具**：spark、Chunky、Cupboard，支持性能剖析与预生成管理。

## 仓库结构

```
├── mods/                # 模组文件（105 个基线模组）
├── config/              # 全部模组配置（纳入版本控制）
├── defaultconfigs/      # 新存档默认配置
├── kubejs/              # KubeJS 魔改脚本
├── resourcepacks/       # 资源包（含中文本地化包）
├── shaderpacks/         # 官方默认光影
├── docs/design/         # 设计知识库（见下）
├── PCL/                 # 启动器实例配置
└── dist/                # 分发产物
```

> `logs/`、`crash-reports/`、`saves/`、`cache/`、`downloads/` 等为运行时噪音，不纳入版本控制。

## 设计知识库

本包的全部设计决策记录于 [`docs/design/`](docs/design/README.md)：

- [设计文档总览](docs/design/README.md) —— 产品定义、设计支柱、系统地图、跨系统约束
- [变更日志](docs/design/change-log.md) —— 每批模组的添加理由与设计影响
- [组件文档](docs/design/components/) —— 按 `platform / performance / aesthetic / utility / content / integration / library` 分类的逐模组文档
- [调研报告](docs/design/research-report-v1.md) —— 1.21.1 NeoForge 生态与长线运营方案调研

## 参与开发

1. 修改任何模组、配置、资源包、脚本等包内容前，先阅读 [`AGENTS.md`](AGENTS.md) 与 [`AGENT.MD`](AGENT.MD)。
2. 每次变更后执行 `$maintain-modpack-design` 流程：扫描整合包 → 更新 `docs/design/` → 重新生成清单 → 确认 `pending-changes.md` 无待处理变更。
3. 新模组必须明确支持 Minecraft 1.21.1 + NeoForge 21.1.x，并检查依赖链与服务端兼容性。
4. 新增内容模组必须在阶段系统中定义默认解锁状态。

## 命名

**灯台（とうだい / Toudai）**：灯塔。基底包如灯塔般固定、可见、可靠——不随内容章节变动，为所有航线（衍生包）指引方向。
