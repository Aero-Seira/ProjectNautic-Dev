# ProjectNautic 模组添加代理指南

> 本文档用于指导代理在整合包中正确地添加、验证和记录模组。
> 与 `AGENTS.md` 配合使用：`AGENTS.md` 定义全局规则，本文档聚焦模组添加的具体操作流程。

---

## 1. 适用范围

本文档适用于以下操作：
- 向 `mods/` 目录添加新的模组 `.jar` 文件
- 替换/升级现有模组版本
- 验证模组兼容性
- 同步更新模组相关的配置文件

---

## 2. 添加前检查清单

在添加任何模组之前，必须确认以下信息：

| 检查项 | 要求 | 验证方式 |
|--------|------|----------|
| Minecraft 版本 | **1.21.1** | 查看模组页面或文件名 |
| 模组加载器 | **NeoForge 21.1.x** | 查看模组页面说明 |
| 依赖模组 | 所有硬依赖必须已满足或同时添加 | 查看模组 CurseForge / Modrinth 页面 |
| 服务端兼容性 | 确认是否仅客户端，或需要服务端同步 | 查看模组标签（Client-Side / Server） |
| 许可证 | 允许包含在整合包中 | 查看模组许可证 |

> ⚠️ **禁止添加**：Fabric 独占模组、Forge（非 NeoForge）独占模组、版本不匹配的模组。

---

## 3. 添加流程

### Step 1 — 准备模组文件

1. 将模组 `.jar` 文件放入 `mods/` 目录
2. 命名规范：保留原始文件名（包含版本号），不要重命名
   - ✅ 正确：`jei-1.21.1-neoforge-19.8.2.99.jar`
   - ❌ 错误：`jei.jar`

### Step 2 — 检查配置变更

启动游戏一次（或检查日志），确认：
- 模组是否正确加载（`logs/latest.log` 中搜索模组 ID）
- 是否在 `config/` 目录生成了新的配置文件
- 是否出现冲突或崩溃

### Step 3 — 记录变更

每次添加模组后，必须按 `AGENTS.md` 要求执行 `$maintain-modpack-design`：

1. **扫描整合包**：检查 `mods/` 目录新增/变更的文件
2. **更新 `docs/design/`**：
   - 在 `docs/design/change-log.md` 中记录新增模组（名称、版本、添加理由、设计影响）
   - 在 `docs/design/README.md` 的「系统地图」中新增或更新对应组件条目，链接使用分类路径（如 `components/performance/sodium.md`）
   - 按模组设计角色选择分类目录，创建 `components/<category>/<mod-id>.md`
   - 将模组的配置文件关联到对应组件文档，不单独为每个配置文件创建组件页
3. **更新自动生成清单**：
   - 重新生成 `docs/design/_generated/current-inventory.md`
   - 更新 `docs/design/_generated/pending-changes.md`
4. **接受基线**：确认 `pending-changes.md` 中无未处理的变更

### Step 4 — 验证仓库状态

执行以下命令，确认 Git 状态干净：
```bash
git status          # 仅应出现预期的新增/修改文件
git add .           # 暂存变更
git diff --cached   # 最终检查暂存内容
```

---

## 4. 模组分类与文档化

在 `docs/design/README.md` 的「系统地图」中，模组应按以下分类记录，且组件文档必须存放在对应的分类子目录中：

| 分类 | 目录 | 设计角色 | 示例 |
|------|------|----------|------|
| `platform` | `components/platform/` | 运行平台、加载器、基础环境配置 | `platform-runtime.md` |
| `performance` | `components/performance/` | 性能优化（帧率、延迟、内存） | Sodium、Lithium、ImmediatelyFast |
| `aesthetic` | `components/aesthetic/` | 视觉 / 音效 / UI 风格 | Iris、ModernUI、Sound Physics |
| `utility` | `components/utility/` | 辅助工具 / QoL | JEI、Jade、JourneyMap |
| `content` | `components/content/` | 新增游戏内容（方块、物品、生物等） | Create、Alex's Mobs |
| `integration` | `components/integration/` | 跨模组联动 / 兼容补丁 / API 扩展 | KubeJS Additions、Compat 模组 |
| `library` | `components/library/` | 依赖库 / 运行时，对玩家不可见 | Rhino、Architectury API、Cloth Config |
| `other` | `components/other/` | 无法归入上述类别的特殊组件 | 混合用途模组 |

**文档创建规则：**
1. 添加模组前，先确定其**设计角色**（不是技术实现），选择对应分类。
2. 若分类目录不存在，使用 `mkdir -p` 创建。
3. 在对应分类目录下创建 `components/<category>/<mod-id>.md`。
4. 系统地图中的链接必须包含分类路径，如 `[Sodium](components/performance/sodium.md)`。
5. 一个模组只能属于一个分类。若模组合并多种角色，按**主要设计意图**归类。

---

## 5. 常见问题

### Q: 模组版本号与 NeoForge 版本冲突？
A: 优先选择明确标注支持 NeoForge 21.1.x 的版本。若只有 Forge 版本，需手动测试兼容性，并在设计文档中标注风险。

### Q: 添加模组后游戏崩溃？
A: 按以下顺序排查：
1. 检查 `crash-reports/` 中的最新报告
2. 检查 `logs/latest.log` 末尾的异常堆栈
3. 确认所有依赖模组已安装
4. 若无法解决，**回退变更**，记录问题到 `docs/design/change-log.md`

### Q: 配置文件是否需要纳入版本控制？
A: 默认需要。`config/` 下的所有文件（除运行日志外）都应纳入 Git，以确保团队配置一致。

### Q: 可以删除模组吗？
A: 可以，但需：
1. 在 `docs/design/change-log.md` 中记录移除原因
2. 检查并清理其遗留的 `config/` 文件（若不再需要）
3. 检查存档兼容性影响（若已有 `saves/` 数据）

---

## 6. 快速参考命令

```bash
# 扫描模组目录
ls -la mods/

# 检查模组是否加载
grep -i "<mod_id>" logs/latest.log

# 统计当前模组数量
ls mods/*.jar | wc -l

# 查看模组依赖关系（NeoForge 启动日志中查找）
grep -i "dependencies\|requires\|missing" logs/latest.log
```

---

## 7. 与 AGENTS.md 的协作关系

| 文件 | 作用 |
|------|------|
| `AGENTS.md` | 全局代理规则：设计文档更新、基线验证、忽略运行时噪音 |
| `AGENT.MD`（本文档） | 模组添加专项流程：兼容性检查、文件管理、文档化规范 |

**执行顺序**：先遵循 `AGENTS.md` 的扫描→更新→验证流程，再参考本文档的模组专用检查项。
