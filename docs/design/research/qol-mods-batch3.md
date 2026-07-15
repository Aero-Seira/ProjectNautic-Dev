# 第三批：非视听觉美化 QoL 模组调研（修正版）

> 基于 Labpack-1.21.1 已有模组品味重新整理
> 调研时间：2026-07-15

---

## 一、从 Labpack 中观察到的 QoL 选择风格

你的 Labpack 中已包含大量 QoL 模组，可以总结出以下品味倾向：

| 倾向 | 代表模组 | 说明 |
|------|----------|------|
| **极度重视性能** | ModernFix、FerriteCore、FastSuite、C2ME、各种剔除优化、内存泄漏修复 | 不仅基础渲染优化，还深入到配方匹配、区块并发、属性系统 |
| **重视地图导航** | Xaero 小地图 + 世界地图 + 增强 | 完整地图体系 |
| **重视建造/挖掘效率** | FTB Ultimine、建筑手杖 | 大幅简化重复劳动 |
| **重视信息展示** | BetterF3、BetterAdvancements、AppleSkin、JEI、Jade | 界面信息密度和可读性 |
| **重视中文体验** | 自动汉化、拼音搜索 | 本地化优先 |
| **重视测试/开发** | 试验假人、Spark | 方便验证和调试 |
| **重视小便利** | 截图到剪贴板、多态合成、跳过末地传送动画 | 细节处的流畅感 |
| **重视聊天/社交** | 聊天头像、禁用聊天举报 | 多人环境体验 |

**关键发现**：你对 QoL 的门槛很高——不仅要"有用"，还要"高效"和"稳定"。性能优化不是可选项，是基础设施。

---

## 二、Labpack 已有、ProjectNautic 尚缺的 QoL

以下是从 Labpack 中提取的、ProjectNautic 目前**还没有**的 QoL 模组，按优先级分层：

### Tier 1 — 核心性能层（建议现在加）

这些和现有 Sodium + Lithium + ImmediatelyFast 形成完整的性能基础设施：

| 模组 | 作用 | 设计角色 | 范围 | 备注 |
|------|------|----------|------|------|
| **ModernFix** | 启动加速、动态资源加载、内存去重 | `performance` | BOTH | Labpack 已验证稳定 |
| **FerriteCore** | 减少内存占用（方块状态、模型、NBT 压缩） | `performance` | BOTH | 与 Sodium/Lithium 互补，侧重"省内存" |
| **FastSuite** | 配方匹配性能优化 | `performance` | BOTH | 模组多了以后配方查找卡顿明显 |
| **AttributeFix** | 修复属性系统上限问题 | `utility` | BOTH | 很多模组依赖，防止属性溢出 |

> 这 4 个模组和现有的 Sodium + Lithium + ImmediatelyFast 组合后，ProjectNautic 的性能基线就和 Labpack 对齐了。

### Tier 2 — 操作效率层（建议现在加）

| 模组 | 作用 | 设计角色 | 范围 | 备注 |
|------|------|----------|------|------|
| **FTB Ultimine** | 连锁挖掘（同类型方块） | `utility` | BOTH | 需 FTB Library；建筑/挖矿质变 |
| **Construction Wand** | 建筑手杖，批量放置同类型方块 | `utility` | BOTH | 建筑效率核心工具 |
| **Polymorph** | 冲突配方时提供选择按钮 | `utility` | BOTH | 多个模组添加相同配方时的救星 |
| **Ksyxis** | 跳过末地传送门加载动画 | `utility` | BOTH | 小细节，往返末地节省数秒 |

### Tier 3 — 信息展示层（建议现在加）

| 模组 | 作用 | 设计角色 | 范围 | 备注 |
|------|------|----------|------|------|
| **BetterF3** | F3 调试界面模块化、可配置、更美观 | `utility` | CLIENT | 不是视觉美化，是信息组织优化 |
| **BetterAdvancements** | 进度界面更大、显示要求更清晰 | `utility` | CLIENT | 长线包进度多，此模组价值上升 |
| **Screenshot to Clipboard** | 截图直接复制到剪贴板 | `utility` | CLIENT | 极小，方便分享 |

### Tier 4 — 开发/测试工具（按需）

| 模组 | 作用 | 设计角色 | 范围 | 备注 |
|------|------|----------|------|------|
| **MmmMmmMmmMmm (试验假人)** | 放置假人测试 DPS/伤害 | `utility` | BOTH | 开始写 KubeJS 战斗相关内容时加 |
| **Spark** | 性能分析，定位卡顿源 | `utility` | BOTH | 服务端长线运营诊断必备 |

### Tier 5 — 地图导航（Vanilla+ 起步可延后）

| 模组 | 作用 | 设计角色 | 范围 | 备注 |
|------|------|----------|------|------|
| **Xaero's Minimap** | 小地图 | `utility` | CLIENT | 和 Jade 不冲突 |
| **Xaero's World Map** | 世界地图（已探索区域全图） | `utility` | CLIENT | 与小地图配套 |
| **XaeroPlus** | Xaero 地图增强 | `utility` | CLIENT | 依赖前两者 |

> 小地图属于强个人偏好。你的 Labpack 中用了全套 Xaero，但 Vanilla+ 起步阶段可以先不加，等探索内容多了再上。

### Tier 6 — 社交/聊天（可选）

| 模组 | 作用 | 设计角色 | 范围 | 备注 |
|------|------|----------|------|------|
| **Chat Heads** | 聊天框显示玩家头像 | `utility` | CLIENT | 多人环境识别发言者 |
| **No Chat Reports** | 禁用微软聊天举报系统 | `utility` | BOTH | 私服运营有价值 |

### Tier 7 — 中文本地化（建议现在加）

| 模组 | 作用 | 设计角色 | 范围 | 备注 |
|------|------|----------|------|------|
| **I18nUpdateMod** | 自动汉化更新 | `utility` | CLIENT | 中文玩家体验基础 |
| **Just Enough Characters** | JEI 拼音搜索 | `utility` | CLIENT | JEI 中搜"shitou"找石头 |

---

## 三、Labpack 中有但建议 ProjectNautic **不加**的

| 模组 | 原因 |
|------|------|
| C2ME | Alpha 阶段，稳定性风险。ProjectNautic 长线运营优先，等 C2ME 更成熟 |
| Entity Culling / cullleaves / moreculling | 这些和 Sodium 的剔除功能有重叠，Sodium 0.8+ 已内置大部分。除非测试证明有提升，否则不建议叠加 |
| BadOptimizations | 小型优化集合，效果有限，维护活跃度一般 |
| flerovium / cupboard / alltheleaks | 非常底层的优化/修复，Labpack 中作为实验性添加。长线运营包应保守，等明确需要再加 |
| GPU Memory Leak Fix | 特定显卡驱动问题，非普遍需求 |
| Reese's Sodium Options | Sodium 界面改进，属于"界面美化"边缘 |
| Resourcify | 资源包下载管理，Vanilla+ 起步阶段不需要 |
| Euphoria Patcher | 着色器补丁，属于视觉 |
| Tiny Item Animations | 小动画，属于视觉 |

---

## 四、推荐添加批次（修正后）

### 第三批-A：性能基础设施（现在加）

1. **ModernFix**
2. **FerriteCore**
3. **FastSuite**
4. **AttributeFix**

> 这 4 个和现有 Sodium + Lithium + ImmediatelyFast 组成完整的性能层。

### 第三批-B：操作效率（现在加）

5. **FTB Ultimine**（依赖 FTB Library）
6. **Construction Wand**
7. **Polymorph**
8. **Ksyxis**

### 第三批-C：信息展示（现在加）

9. **BetterF3**
10. **BetterAdvancements**
11. **Screenshot to Clipboard**

### 第三批-D：中文基础（现在加）

12. **I18nUpdateMod**
13. **Just Enough Characters**

### 第三批-E：社交（可选）

14. **Chat Heads**
15. **No Chat Reports**

### 延后添加：

- **Xaero 地图系列** — 等探索内容模组加入后再上
- **试验假人** — 等开始写战斗/伤害相关 KubeJS 脚本时加
- **Spark** — 等服务器部署或出现性能问题时加

---

## 五、与现有模组的兼容性

| 新增模组 | 与现有模组的关系 |
|----------|-----------------|
| ModernFix | 与 Sodium/Lithium 互补，无冲突 |
| FerriteCore | 与 Sodium 互补（Sodium 优化渲染，FerriteCore 优化内存） |
| FastSuite | 与 JEI/KubeJS 配方系统兼容，加速配方匹配 |
| FTB Ultimine | 需 FTB Library；与 KubeJS 无冲突 |
| Polymorph | 与 JEI 兼容，冲突配方时显示选择按钮 |
| BetterF3 | 纯客户端 HUD，无冲突 |
| BetterAdvancements | 纯客户端界面，无冲突 |

---

*基于 Labpack 实际品味重新整理完成。*
