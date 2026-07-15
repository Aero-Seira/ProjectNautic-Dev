# Visuality

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/[可视性：重铸] visuality-forge-2.1.0.jar`
- 标识与版本：visuality 2.1.0
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Visuality 为 ProjectNautic 提供方块与实体的视觉粒子增强，直接服务于"视觉沉浸感"设计支柱。它在不破坏原版可读性的前提下，为矿石、灵魂沙、生物受击、装备等场景添加细腻的微粒子反馈，使世界看起来更有生气。

该模组与 Particular、Ambiance、Effectual 同属环境粒子层：Visuality 聚焦"接触点"粒子（踩踏、受击、装备、方块微光），Particular 负责大尺度自然现象，Ambiance 负责特定对象氛围，Effectual 负责玩家状态与交互。

## 玩家可见行为

- **可发现性**：玩家 mining 金/钻石/绿宝石矿时会看到对应颜色的 sparkle 粒子；踩在灵魂沙上会出现灵魂粒子；攻击骷髅、鸡、村民等生物会飞溅骨头/羽毛/绿宝石粒子；穿戴金/钻石盔甲会散发微光。
- **交互**：无需主动操作，粒子效果由玩家在世界中的行为自然触发。
- **进度/奖励**：无内置进度；粒子作为探索与战斗的视觉奖励。
- **摩擦/失败**：大量粒子在密集区域同时生成时可能降低 CLIENT 端帧率；与同样覆盖这些对象的粒子模组可能产生重复。

## 集成关系

- **依赖**：Minecraft 1.21.1（required, CLIENT）。
- **冲突**：无已知严重冲突。
- **协作**：与 **Particular**、**Ambiance**、**Effectual** 同属环境粒子层；与 **Ambiance** 在灵魂沙等方块上存在效果互补。
- **配方/标签**：无直接参与。
- **任务**：无。
- **世界生成**：无。
- **UI**：无新增 UI，配置依赖配置文件。
- **资源**：依赖内置粒子纹理，无需额外资源包。
- **脚本**：无直接 KubeJS 交互。
- **加载顺序假设**：标准 CLIENT 端加载。

## 配置意图

Visuality 的配置分散在 `config/visuality/` 目录下，当前配置保持默认并做少量风格统一：

### `config/visuality/config.toml`

- **`slime`**：`enabled = true`，保留史莱姆 blob 粒子。
- **`charge`**：`enabled = true`，保留 charge 粒子效果。
- **`waterCircle`**：`enabled = true`、`colored = true`、`density = 16`，启用彩色水圈，密度保持默认 16，在水体交互中提供清晰但不夸张的视觉反馈。

### `config/visuality/particle_emitters/block_ambient.json`

- `enabled=true`，`interval=10`。
- 为金/钻石/绿宝石矿及其深板岩变种、紫水晶簇添加 sparkle 粒子，颜色与矿物本身保持一致（金 `#C7FFBF`、钻石 `#70FFEE`、绿宝石 `#D9FFEB`）。
- 灵魂沙/灵魂土向上生成 `visuality:soul` 粒子，与 Ambiance 的灵魂氛围形成呼应。

### `config/visuality/particle_emitters/block_step.json`

- `enabled=true`，`interval=10`。
- 仅在踩在灵魂沙/灵魂土上时生成灵魂粒子，保持粒子来源清晰，避免所有方块都产生踩踏粒子导致视觉噪音。

### `config/visuality/particle_emitters/entity_armor.json`

- `enabled=true`，`interval=20`。
- 仅金套与钻石套散发 sparkle 粒子，颜色分别对应 `#C7FFBF` 与 `#70FFEE`，强化高级装备的身份感，不覆盖铁/皮革/锁链套以保持克制。

### `config/visuality/particle_emitters/entity_hit.json`

- `enabled=true`，`min_amount=1`、`max_amount=20`。
- 骷髅/骷髅马/流浪者受击产生骨头粒子；凋灵骷髅产生凋灵骨头粒子；鸡产生羽毛；村民/流浪商人产生绿宝石粒子。这些反馈与原版掉落物语义一致，增强打击感。

## 兼容性与性能

- **客户端/服务端范围**：仅 **CLIENT**。
- **存档影响**：无。
- **已知不兼容**：无。
- **资源成本**：粒子为轻量 CLIENT 端开销；大量矿物 sparkle 与受击粒子在密集战斗中可能短暂增加 GPU 负载。
- **缓解措施**：通过 `interval` 控制发射频率，通过白名单限制触发对象，避免无差别粒子泛滥。

## 验证

- [ ] 启动或加载测试
- [ ] 功能测试
- [ ] 多人游戏测试（如适用）
- [ ] 性能或世界生成测试（如适用）
- [ ] 相关系统的回归检查

## 风险与开放问题

1. 矿物 sparkle 粒子在大型矿洞中大量同时出现时，可能对低端 GPU 造成压力。
2. 与 Ambiance 同时作用于灵魂沙时，需验证粒子是否过度堆叠。

## 历史

- 2026-07-16: 作为第三批 QoL 模组添加
