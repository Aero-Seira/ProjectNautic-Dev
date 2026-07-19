# Minecraft 冒险结构 RPG 副本化调研报告

> 调研日期：2026-07-16
> 调研范围：Minecraft Java Edition 1.21+ 副本/地下城系统实现方案

---

## 一、什么是 RPG 副本系统（Instance Dungeon）

在 RPG/MMORPG 游戏中，**副本（Instance）** 指的是为特定玩家或队伍独立生成的隔离游戏空间。其核心特征包括：

| 特征 | 说明 |
|------|------|
| **隔离性** | 每个队伍拥有独立的副本空间，互不干扰 |
| **可重置性** | 副本完成后可以重置为初始状态，供下一批玩家挑战 |
| **条件进入** | 需要满足等级、道具、队伍人数等条件才能进入 |
| **目标驱动** | 有明确的胜利条件（击败Boss、触发机关、限时存活等） |
| **奖励机制** | 通关后获得专属 loot、经验、货币等奖励 |
| **进度管理** | 支持多阶段推进、存档点、失败重试等机制 |

---

## 二、Minecraft 中实现副本的 4 种技术路线

### 路线 A：服务端插件方案（推荐用于服务器）

**原理**：利用 Bukkit/Spigot/Paper API，在服务端层面动态复制世界文件夹，为每个队伍创建独立的世界实例。

**代表插件**：
- **MythicDungeons**（付费）— 功能最完善的副本系统
- **InstancedDungeons** — 支持 Paper 1.21+，开源
- **SinceDungeon**（付费）— 高性能异步处理，GUI编辑器
- **DungeonInstances** — 开源，轻量级

**核心流程**：
```
1. 管理员创建"模板世界"（Template World）
2. 玩家组队 → 申请进入副本
3. 插件复制模板世界 → 生成实例世界（instance_dungeon_<uuid>）
4. 传送玩家到实例世界
5. 玩家挑战副本（击败Boss/触发机关/限时目标）
6. 副本结束 → 插件删除实例世界，清理资源
```

**优点**：
- 功能完善（队伍系统、掉落、计时器、Boss战等）
- 无需客户端安装任何内容
- 已有成熟的插件生态（MythicMobs、MMOItems、Vault等）

**缺点**：
- 仅适用于服务端，单机/局域网无法使用
- 高频创建/删除世界可能影响磁盘 I/O
- 需要 Paper/Spigot 服务端，不兼容原版/Fabric/Forge

---

### 路线 B：自定义维度 + 数据包方案（推荐用于模组包/数据包）

**原理**：利用 Minecraft 1.16+ 的自定义维度（Custom Dimension）功能，配合数据包（Datapack）创建一个独立的副本维度。每个副本作为单独的维度存在，通过传送门或命令进入。

**技术要点**：

```json
// 自定义维度定义（data/example/dimension/dungeon_1.json）
{
  "type": "minecraft:overworld",
  "generator": {
    "type": "minecraft:flat",
    "settings": {
      "biome": "minecraft:the_void",
      "layers": [],
      "structures": {
        "structures": {}
      }
    }
  }
}
```

**结合结构方块（Structure Block）实现副本模板**：

1. 在创造模式下用结构方块保存副本建筑为 `.nbt` 文件
2. 将 `.nbt` 放入数据包的 `data/example/structure/` 目录
3. 使用拼图方块（Jigsaw Block）实现随机化、多阶段副本生成
4. 通过 `execute in` 命令将玩家传送到副本维度

**优点**：
- 纯原版/数据包实现，无需插件或模组
- 可以打包进模组包，玩家安装即可使用
- 兼容单人游戏和多人游戏

**缺点**：
- 副本维度是全局共享的，无法做到"每队伍一个独立副本"
- 需要手动重置副本状态（清理怪物、重置箱子等）
- 功能受限于原版命令和数据包能力

---

### 路线 C：Fabric/Forge 模组方案（推荐用于模组开发）

**原理**：通过模组代码直接操作世界生成、实体生成、玩家传送等底层机制，实现真正的副本实例化。

**技术实现方向**：

| 方向 | 说明 |
|------|------|
| **虚拟维度** | 创建临时维度，副本结束后销毁 |
| **区块克隆** | 将模板区块动态克隆到目标位置 |
| **结构生成** | 利用 `StructureTemplateManager` 程序化放置 `.nbt` 结构 |
| **区域隔离** | 在同一世界中划分独立区域，通过 WorldEdit/Region 隔离玩家 |

**代表模组**：
- **When Dungeons Arise** — 程序化生成大型地牢结构
- **Dungeon Crawl** — 地下城生成模组
- **Integrated Dungeons and Structures (IDAS)** — 集成化结构生成

**优点**：
- 可以实现真正的实例化副本（每队伍独立）
- 性能优化空间大（按需加载、异步生成）
- 可以深度定制游戏机制

**缺点**：
- 开发门槛高，需要 Java/Kotlin 编程能力
- 需要维护多版本兼容性（Fabric/Forge/NeoForge）
- 玩家必须安装模组

---

### 路线 D：混合方案（推荐用于大型项目）

**原理**：结合数据包 + KubeJS/CraftTweaker + 轻量级模组，在 Fabric/Forge 环境中实现副本系统。

**技术栈示例**：
```
- 结构模板：用结构方块/WorldEdit 制作副本建筑
- 副本逻辑：KubeJS 脚本控制进入条件、进度、奖励
- 维度管理：Custom Dimensions API（模组）创建临时维度
- 怪物生成：MythicMobs（插件）或 数据包自定义刷怪笼
- 掉落系统：LootJS 自定义战利品表
- UI界面：FTB Quests 或 自定义模组
```

**代表整合包**：
- **Integrated MC** — CraisinLord 的 dungeon-busting 整合包
- **Vault Hunters** — 以副本/宝库为核心玩法的整合包

---

## 三、副本系统的核心设计要素

### 3.1 进入机制

| 机制 | 实现方式 |
|------|---------|
| **NPC对话** | Citizens 插件 / 数据包交互实体 / 模组自定义村民 |
| **传送门** | 自定义方块 + 传送逻辑（Multiverse-NetherPortals / 模组） |
| **命令/菜单** | 插件命令（/dungeon join）/ FTB Quests 任务触发 |
| **地图物品** | 自定义物品 + 右键触发传送 |
| **物理入口** | 在世界中生成副本入口结构（如地下城大门） |

### 3.2 队伍系统

```
队伍组建 → 人数检查（min-max） → 等级/装备检查 → 入場费用扣除 → 副本创建 → 传送
   ↑___________________________________________↓
                     邀请/加入机制
```

**关键功能**：
- 队伍创建与邀请（点击消息接受/拒绝）
- 队长权限（开始副本、踢出队员）
- 副本中禁止退队/换队（防 sabotage）
- 离线玩家处理（保留位置/踢出队伍）

### 3.3 副本目标类型

| 类型 | 说明 | 示例 |
|------|------|------|
| **Boss击杀** | 击败特定Boss | 末影龙、凋灵、MythicMobs自定义Boss |
| **机关触发** | 激活按钮/拉杆/压力板 | 解密地下城 |
| **波次生存** | 抵挡N波怪物进攻 | 竞技场模式 |
| **限时挑战** | 在规定时间内完成目标 | 速通副本 |
| **收集任务** | 收集特定物品/钥匙 | 钥匙开门机制 |
| **混合目标** | 多阶段组合目标 | 先清怪→再解密→最后Boss |

### 3.4 进度与阶段管理（Stage/Gate系统）

```
[入口] → [第一阶段：清怪] → [Gate解锁] → [第二阶段：Boss战] → [通关奖励]
             ↑                                              ↓
        [失败：重置/退出]                              [失败：全队死亡]
```

**Stage 系统功能**：
- **Gate（关卡门）**：阻挡玩家进入下一阶段，需完成条件解锁
- **CheckPoint（检查点）**：死亡后在此重生
- **并行阶段**：多个目标可同时完成
- **顺序阶段**：必须按顺序完成

### 3.5 掉落与奖励系统

| 掉落类型 | 实现方式 |
|---------|---------|
| **箱子Loot** | LootTable JSON / 插件自定义掉落池 |
| **Boss掉落** | MythicMobs Drops / 数据包战利品表 |
| **通关奖励** | 个人GUI奖励界面（防止抢装备） |
| **时间奖励** | 速通额外奖励（Tier系统） |
| **保底机制** | 计分板统计抽奖次数，达到阈值必出稀有物品 |

### 3.6 安全与防作弊

| 威胁 | 防护措施 |
|------|---------|
| TPA传送进副本 | 禁止非副本内TP、关闭跨世界TP |
| 物品带出/带入 | 副本内独立背包系统 / 入場清空背包 |
| 命令绕过 | 禁用副本内非白名单命令 |
| 区块加载漏洞 | 限制副本视距、禁止旁观者模式 |
| 刷物品 | 副本内禁止丢物品、禁止漏斗/箱子交互 |

---

## 四、现有方案对比

| 方案 | 类型 | 成本 | 技术门槛 | 适用场景 | 独立性 |
|------|------|------|---------|---------|--------|
| **MythicDungeons** | 插件 | 付费 | 低 | 大型RPG服务器 | ⭐⭐⭐⭐⭐ |
| **SinceDungeon** | 插件 | 付费 | 低 | 现代Paper服务器 | ⭐⭐⭐⭐⭐ |
| **InstancedDungeons** | 插件 | 免费 | 中 | 中小型服务器 | ⭐⭐⭐⭐ |
| **DungeonInstances** | 插件 | 免费 | 中 | 轻量级需求 | ⭐⭐⭐ |
| **纯数据包** | 数据包 | 免费 | 高 | 原版模组包 | ⭐⭐ |
| **Fabric模组开发** | 模组 | 免费 | 很高 | 深度定制 | ⭐⭐⭐⭐⭐ |
| **KubeJS+模组混合** | 混合 | 免费 | 中高 | 整合包开发 | ⭐⭐⭐⭐ |

---

## 五、推荐实现路径

### 场景 1：开 RPG 服务器（ quickest to production ）

**推荐**：Paper 服务端 + SinceDungeon / MythicDungeons + MythicMobs + MMOItems

**配置示例**：
```yaml
# SinceDungeon 副本配置示例
dungeons:
  ancient_ruins:
    template_world: "ruins_template"
    min_players: 1
    max_players: 5
    time_limit: 1800  # 30分钟
    lives: 3          # 每人3条命
    entry_cost:
      money: 1000
      items:
        - "minecraft:diamond:5"
    stages:
      - id: "entrance"
        order: 1
        gate: true
        mobs:
          - type: "skeleton"
            count: 10
            trigger: "ON_PLAYER_NEAR"
      - id: "boss_room"
        order: 2
        gate: true
        boss: "ancient_guardian"
    rewards:
      completion:
        - item: "mmoitems:sword_legendary"
          chance: 0.1
        - command: "give %player% diamond 10"
```

### 场景 2：制作整合包/模组包

**推荐**：自定义维度数据包 + 结构模板 + KubeJS 脚本

**工作流**：
1. 用 WorldEdit/结构方块 设计副本建筑
2. 导出为 `.nbt` 放入数据包
3. 用 KubeJS 写进入逻辑：
   ```javascript
   // KubeJS 示例：副本进入逻辑
   BlockEvents.rightClicked('minecraft:stone_bricks', event => {
     if (event.item.id == 'kubejs:dungeon_key') {
       let player = event.player;
       let party = getParty(player); // 自定义队伍管理
       if (party && party.size >= 2) {
         createInstance('dungeon_1', party);
         teleport(party, 'custom_dimensions:dungeon_1');
       }
     }
   });
   ```
4. 用 FTB Quests 做任务线和进度追踪

### 场景 3：从零开发模组

**推荐**：Fabric API + Custom Dimension API + Structure Template API

**核心代码方向**：
```java
// 伪代码：副本实例管理器
public class DungeonInstanceManager {
    // 创建副本实例（复制模板维度）
    public DungeonInstance createInstance(DungeonTemplate template, Party party) {
        UUID instanceId = UUID.randomUUID();
        World templateWorld = getTemplateWorld(template);
        World instanceWorld = copyWorld(templateWorld, "dungeon_" + instanceId);
        
        // 注册实例状态
        instances.put(instanceId, new DungeonInstance(instanceId, party, instanceWorld));
        
        // 传送队伍
        for (Player player : party.getMembers()) {
            teleport(player, instanceWorld.getSpawnPos());
        }
        
        return instances.get(instanceId);
    }
    
    // 清理副本实例
    public void destroyInstance(UUID instanceId) {
        DungeonInstance instance = instances.remove(instanceId);
        if (instance != null) {
            // 传送所有玩家回主城
            for (Player player : instance.getPlayers()) {
                teleport(player, hubWorld.getSpawnPos());
            }
            // 异步删除世界文件夹
            asyncDeleteWorld(instance.getWorld());
        }
    }
}
```

---

## 六、关键技术资源

### 结构/拼图方块教程
- [Minecraft Wiki: Custom Structures Tutorial](https://minecraft.wiki/w/Tutorial:Custom_structures)
- [GentlemanRevvnar's Jigsaw Guide](https://gist.github.com/GentlemanRevvnar/98a8f191f46d28f63592672022c41497)

### 插件资源
- [MythicDungeons](https://git.lumine.io/mythiccraft/MythicDungeons)
- [InstancedDungeons](https://modrinth.com/project/cwU9AXgo)
- [SinceDungeon](https://modrinth.com/plugin/sincedungeon)
- [DungeonInstances](https://modrinth.com/project/ZHmOTZF8)

### 模组/数据包资源
- [When Dungeons Arise](https://www.curseforge.com/minecraft/mc-mods/when-dungeons-arise)
- [Dungeon Crawl](https://www.curseforge.com/minecraft/mc-mods/dungeon-crawl)
- [Integrated Dungeons and Structures](https://www.curseforge.com/minecraft/mc-mods/integrated-dungeons-and-structures)

---

## 七、总结

将 Minecraft 的冒险结构变成 RPG 副本，核心要解决三个问题：

1. **空间隔离** — 如何让每个队伍有独立的副本空间？
   - 插件方案：动态复制世界（最成熟）
   - 模组方案：创建临时维度（最灵活）
   - 数据包方案：固定维度+区域管理（最简单）

2. **状态重置** — 如何让副本每次进入都是初始状态？
   - 插件：每次创建新世界副本
   - 模组：区块级重置/结构重新生成
   - 数据包：命令方块重置机制

3. **游戏流程** — 如何设计有意义的副本体验？
   - 队伍系统 → 目标机制 → 阶段推进 → 奖励反馈
   - 需要配合 MythicMobs（自定义怪物）、LootTable（掉落）、FTB Quests（任务线）等工具

**对于 ProjectNautic 模组包**，推荐采用 **数据包 + KubeJS + 自定义结构** 的混合方案，既能打包进模组包，又能实现较为完整的副本体验。如果未来考虑开服，可以平滑迁移到 Paper + 插件方案。

---

> 本报告由 Kimi Work 生成，数据来源：CurseForge、Modrinth、SpigotMC、MC百科、Minecraft Wiki 等社区资源。
