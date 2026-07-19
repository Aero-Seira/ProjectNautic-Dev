# Minecraft 冒险结构机制保护调研报告

> 调研日期：2026-07-16
> 核心问题：在不使用完全隔离副本的前提下，防止玩家利用 Minecraft 高自由度跳过作者设计的冒险机制

---

## 一、核心设计哲学：从"阻止玩家作弊"到"设计冗余"

Minecraft 的开放性是其核心魅力，但也是冒险地图设计的最大挑战。与其试图阻止所有跳关行为（技术上不可能），优秀的设计通常采用 **"分层防御 + 体验引导"** 的策略：

```
第一层：物理隔离（冒险模式 + 屏障）      ← 阻止无意识破坏
第二层：逻辑检测（命令/数据包）            ← 检测异常状态并纠正
第三层：设计冗余（假设玩家会跳关）          ← 即使跳关也有合理体验
第四层：社交约束（任务书/FTB Quests）       ← 引导玩家按预期游玩
```

---

## 二、威胁模型：玩家可能如何跳关

### 2.1 破坏类绕过

| 手段 | 说明 | 风险等级 |
|------|------|---------|
| **徒手挖掘** | 冒险模式可完全阻止 | ⭐ |
| **工具挖掘** | 需配合 `can_break` 限制 | ⭐⭐ |
| **TNT/爆炸** | 苦力怕、TNT、床（下界/末地）、末影水晶 | ⭐⭐⭐⭐ |
| **活塞推动** | 推动方块破坏结构、推入禁用区域 | ⭐⭐⭐ |
| **水/岩浆流** | 冲走实体、破坏作物/红石、传送玩家 | ⭐⭐⭐ |

### 2.2 放置类绕过

| 手段 | 说明 | 风险等级 |
|------|------|---------|
| **垫脚搭高** | 跳过围墙、到达高处 | ⭐⭐⭐⭐⭐ |
| **搭桥** | 跨越障碍、跳过谜题区域 | ⭐⭐⭐⭐⭐ |
| **放置实体** | 船、床、重生锚、箱子等 | ⭐⭐⭐ |
| **放置水流** | 制造水流电梯、冲走实体 | ⭐⭐⭐ |

### 2.3 移动类绕过

| 手段 | 说明 | 风险等级 |
|------|------|---------|
| **末影珍珠** | 瞬移过墙、穿越屏障 | ⭐⭐⭐⭐⭐ |
| **鞘翅+烟花** | 飞行越过障碍 | ⭐⭐⭐⭐⭐ |
| **激流三叉戟** | 水中/雨中高速移动，穿透障碍 | ⭐⭐⭐⭐ |
| **紫颂果** | 随机瞬移，可能穿过薄墙 | ⭐⭐⭐ |
| **骑乘实体** | 马、猪、船、矿车跨越障碍 | ⭐⭐⭐ |
| **蜘蛛网/藤蔓** | 爬墙、缓降 | ⭐⭐⭐ |

### 2.4 其他机制绕过

| 手段 | 说明 | 风险等级 |
|------|------|---------|
| **末影箱** | 跨维度存取物品，绕过资源限制 | ⭐⭐⭐ |
| **钓鱼竿** | 拉取实体、触发压力板 | ⭐⭐⭐ |
| **雪球/鸡蛋** | 触发按钮、压力板 | ⭐⭐ |
| **幽灵方块** | 利用网络延迟制造可穿过的方块 | ⭐⭐⭐⭐ |

---

## 三、技术保护方案详解

### 3.1 冒险模式（Adventure Mode）— 基础防线

```
/gamemode adventure @a[tag=in_dungeon]
```

**核心特性**：
- 玩家不能徒手破坏任何方块
- 不能放置任何方块（除非物品有 `can_place_on` 标签）
- 可以与按钮、拉杆、箱子、门等交互
- 可以攻击生物
- 饥饿值正常消耗

**关键物品组件**（1.20.5+）:

```json
// 给玩家一把只能破坏特定方块的工具
/give @p minecraft:iron_pickaxe[can_break={blocks:["minecraft:stone","minecraft:deepslate"]}]

// 给玩家一个只能放在特定方块上的按钮
/give @p minecraft:stone_button[can_place_on={blocks:["minecraft:stone_bricks"]}]
```

**适用场景**：
- 解谜区域：玩家只能破坏指定的"脆弱墙壁"
- 战斗区域：玩家无法搭方块规避战斗
- 剧情区域：保护命令方块、结构完整性

---

### 3.2 屏障方块（Barrier Block）— 隐形墙壁

```
/give @p minecraft:barrier 64
```

**特性**：
- 完全透明，冒险模式不可破坏
- 生存模式不可破坏（但可通过命令获取）
- 不阻挡光线、不连接纹理
- 可以含水

**使用技巧**：

| 场景 | 做法 |
|------|------|
| 阻止攀爬 | 在墙顶外侧放置屏障 |
| 阻止飞行 | 在天空层铺设屏障天花板 |
| 阻止地下 | 在地板下方铺设屏障地板 |
| 区域边界 | 用屏障包围整个冒险区域 |

**注意**：屏障在冒险模式下**可以破坏**（如果工具带有 `can_break` 标签），所以需要配合命令持续清除玩家手中的工具。

---

### 3.3 命令方块/数据包 — 逻辑检测与纠正

#### 方案 A：区域边界检测

```mcfunction
# 每 tick 检测玩家是否越界（数据包 function）
execute as @a[tag=in_dungeon] at @s unless entity @s[x=100,y=50,z=100,dx=50,dy=30,dz=50] run     tp @s 125 60 125
```

**说明**：如果玩家离开指定区域（100,50,100 到 150,80,150），立即传送回中心点。

#### 方案 B：高度检测（防止搭高）

```mcfunction
# 检测玩家是否到达不该到达的高度
execute as @a[tag=in_dungeon] at @s if entity @s[y=80,dy=100] run     effect give @s levitation 1 0 true
# 或者
execute as @a[tag=in_dungeon] at @s if entity @s[y=80,dy=100] run     tp @s ~ 75 ~
```

#### 方案 C：物品清除（防止危险道具）

```mcfunction
# 清除玩家手中的末影珍珠、紫颂果、TNT等
clear @a[tag=in_dungeon] minecraft:ender_pearl
clear @a[tag=in_dungeon] minecraft:chorus_fruit
clear @a[tag=in_dungeon] minecraft:tnt
clear @a[tag=in_dungeon] minecraft:end_crystal
clear @a[tag=in_dungeon] minecraft:firework_rocket
# 清除附魔鞘翅/激流三叉戟
```

#### 方案 D：状态效果控制

```mcfunction
# 给玩家持续施加效果，防止特定行为
effect give @a[tag=in_dungeon] minecraft:jump_boost 1 128 true
# 跳力128 = 无法跳跃（防止跳上奇怪的地方）

effect give @a[tag=in_dungeon] minecraft:slowness 1 5 true
# 缓慢，限制快速移动

effect give @a[tag=in_dungeon] minecraft:mining_fatigue 1 4 true
# 挖掘疲劳，进一步阻止破坏
```

---

### 3.4 游戏规则（Gamerules）— 全局限制

```mcfunction
# 禁止方块破坏掉落（即使破坏也不掉东西）
gamerule doTileDrops false

# 禁止实体爆炸破坏（苦力怕、TNT、床）
gamerule mobGriefing false

# 禁止火焰蔓延（防止岩浆/火焰破坏）
gamerule doFireTick false

# 禁止亡灵在白天燃烧（避免利用阳光）
# 这个无法直接用 gamerule 控制，需要用数据包

# 禁止玩家间PVP（避免恶意击杀）
gamerule pvp false
```

---

### 3.5 结构保护（数据包层面）

在数据包中定义结构时，使用 `processor_list` 添加保护：

```json
// data/example/worldgen/processor_list/dungeon_protection.json
{
  "processors": [
    {
      "processor_type": "minecraft:block_age",
      "mossiness": 0.0
    }
  ]
}
```

更实际的做法是：**在结构生成后，用命令方块在关键位置放置屏障**：

```mcfunction
# 结构生成后执行的保护函数
execute positioned 100 50 100 run fill ~-5 ~-5 ~-5 ~25 ~20 ~25 minecraft:barrier replace minecraft:air
# 然后清除内部空气（只保留外壳）
```

---

### 3.6 实体管理 — 防止骑乘/推挤

```mcfunction
# 杀死玩家附近的船、矿车（防止用船越墙）
kill @e[type=minecraft:boat,distance=..10]
kill @e[type=minecraft:chest_minecart,distance=..10]

# 如果玩家骑着实体，强制下马
execute as @a[tag=in_dungeon] if entity @s[nbt={RootVehicle:{}}] run ride @s dismount

# 清除玩家附近的末影水晶
tkill @e[type=minecraft:end_crystal,distance=..20]
```

---

### 3.7 维度/区域限制（服务器插件层面）

如果使用服务器，推荐以下插件：

| 插件 | 功能 | 适用场景 |
|------|------|---------|
| **WorldGuard** | 区域保护、禁用特定命令、禁用PVP | 通用区域保护 |
| **GriefPrevention** | 玩家领地保护 | 防止玩家互拆 |
| **Denizen** | 脚本系统，自定义NPC和事件 | 复杂逻辑控制 |
| **MythicMobs** | 自定义怪物，包括区域锁定AI | 战斗区域控制 |
| **Dimensional Dungeons** | 维度级别限制建筑和破坏 | 模组包方案 |

**WorldGuard 示例配置**：
```
/rg define dungeon_area
/rg flag dungeon_area block-break deny
/rg flag dungeon_area block-place deny
/rg flag dungeon_area enderpearl deny
/rg flag dungeon_area chorus-fruit deny
/rg flag dungeon_area vehicle-place deny
/rg flag dungeon_area chest-access allow
/rg flag dungeon_area use allow
```

---

## 四、针对 ProjectNautic 的推荐方案

### 4.1 纯数据包方案（无模组依赖）

```
冒险结构进入时：
  ↓
1. 切换玩家到冒险模式
2. 清除危险物品（末影珍珠、TNT、水晶、烟花火箭）
3. 记录玩家进入时的物品栏（用于退出时恢复）
4. 施加缓慢+挖掘疲劳效果
  ↓
冒险结构内实时检测（每 tick）：
  ↓
1. 位置检测：越界则传送回检查点
2. 高度检测：过高则拉回或施加 levitation
3. 物品检测：新获得危险物品立即清除
4. 实体检测：清除附近的船、矿车、水晶
5. 骑乘检测：强制下马
  ↓
冒险结构退出时：
  ↓
1. 恢复玩家原始游戏模式
2. 恢复玩家原始物品栏
3. 清除所有持续效果
```

### 4.2 KubeJS 增强方案（适用于整合包）

```javascript
// 监听玩家进入指定区域
PlayerEvents.tick(event => {
    let player = event.player;
    let pos = player.blockPosition();
    
    // 检测是否在冒险结构区域内
    if (isInDungeonArea(pos)) {
        // 强制冒险模式
        player.setGameMode('adventure');
        
        // 清除危险物品
        let dangerousItems = [
            'minecraft:ender_pearl',
            'minecraft:chorus_fruit',
            'minecraft:tnt',
            'minecraft:end_crystal',
            'minecraft:firework_rocket',
            'minecraft:elytra'
        ];
        
        dangerousItems.forEach(item => {
            player.inventory.clear(ItemFilter.of(item));
        });
        
        // 检测是否装备了鞘翅
        if (player.getItemBySlot('chest').id == 'minecraft:elytra') {
            player.setItemBySlot('chest', 'minecraft:air');
        }
        
        // 检测是否持有激流三叉戟
        let mainHand = player.getItemBySlot('mainhand');
        if (mainHand && mainHand.enchantments.contains('riptide')) {
            player.setItemBySlot('mainhand', 'minecraft:air');
        }
    }
});

// 阻止玩家放置方块
BlockEvents.placed(event => {
    if (isInDungeonArea(event.block.pos)) {
        event.cancel();
        event.player.tell('你不能在这里放置方块！');
    }
});

// 阻止玩家破坏方块
BlockEvents.broken(event => {
    if (isInDungeonArea(event.block.pos)) {
        // 只允许破坏指定方块（如"隐藏墙壁"）
        let allowedBlocks = ['minecraft:cracked_stone_bricks'];
        if (!allowedBlocks.includes(event.block.id)) {
            event.cancel();
        }
    }
});
```

### 4.3 模组方案（如果开发自定义模组）

可参考 **Dimensional Dungeons** 的设计：在自定义维度中，通过 `Level` 或 `Dimension` 的覆盖方法，直接禁用所有方块放置和破坏事件：

```java
// 伪代码：Fabric 事件监听
@Mixin(ServerLevel.class)
public class DungeonDimensionMixin {
    @Inject(method = "destroyBlock", at = @At("HEAD"), cancellable = true)
    private void onDestroyBlock(BlockPos pos, boolean dropBlock, Entity entity, 
                                 int recursionLeft, CallbackInfoReturnable<Boolean> cir) {
        if (isDungeonDimension((ServerLevel)(Object)this)) {
            cir.setReturnValue(false); // 直接阻止破坏
        }
    }
}
```

---

## 五、设计冗余：即使跳关也有合理体验

### 5.1 前向封锁（Forward Locking）

即使玩家到达后面的区域，没有完成前面的目标也无法继续：

```
[区域A] ---[需钥匙A]---> [区域B] ---[需钥匙B]---> [Boss房]
   ↑                                              ↑
玩家可以跳过区域A，但区域B的门需要钥匙A
钥匙A只能在区域A中获得
```

### 5.2 软锁定（Soft Locking）

不强制阻止跳关，但让跳关没有收益：

- **Boss战**：即使玩家到达Boss房，没有收集到"古代符文"，Boss处于无敌状态
- **宝箱**：没有完成前置谜题，宝箱是空的（LootTable 条件判断）
- **出口**：没有完成区域目标，出口传送门不激活

### 5.3 检查点系统

```
[入口] → [检查点1] → [检查点2] → [Boss房]
   ↑          ↑           ↑          ↑
 死亡重生    死亡重生    死亡重生    死亡重生
 到这里     到这里      到这里      到这里
```

使用 `spawnpoint` 命令或数据包 `set_world_spawn` 动态设置重生点。

---

## 六、推荐的保护层级配置

### 轻量级保护（适用于探索型结构）

```yaml
措施:
  - 冒险模式（可选，默认生存）
  - 结构底部铺设屏障（防止挖地道）
  - 顶部铺设屏障（防止鞘翅飞太高）
  - 清除TNT/末影水晶

保留的玩家能力:
  - 放置火把（照明）
  - 挖掘矿石（资源收集）
  - 使用末影珍珠（机动性）
```

### 中等级保护（适用于解谜型结构）

```yaml
措施:
  - 强制冒险模式
  - 全区域屏障外壳
  - 清除所有危险物品（末影珍珠、鞘翅、TNT、水晶）
  - 位置越界检测+传送
  - 高度检测
  - 禁止骑乘

保留的玩家能力:
  - 与按钮/拉杆/门交互
  - 打开箱子
  - 攻击生物
  - 使用特定道具（如解谜钥匙）
```

### 重量级保护（适用于剧情/竞技场结构）

```yaml
措施:
  - 强制冒险模式
  - 全区域屏障外壳
  - 清除所有物品（提供预设装备）
  - 持续施加缓慢+挖掘疲劳
  - 禁止跳跃（Jump Boost 128）
  - 禁止所有实体交互
  - 动态检查点重生
  - 死亡后重置区域状态

保留的玩家能力:
  - 移动
  - 攻击
  - 与剧情NPC交互
```

---

## 七、总结

对于 **ProjectNautic** 的冒险结构，推荐采用 **"中等级保护 + 设计冗余"** 的组合策略：

**核心措施**：
1. **进入结构时切换冒险模式**（防止无意识的方块破坏）
2. **结构外围用屏障方块封闭**（防止挖掘和飞行绕过）
3. **清除/禁用末影珍珠、鞘翅、TNT**（防止主要跳关手段）
4. **区域边界检测**（越界传送回检查点）
5. **前向封锁设计**（即使到达后面的区域，没有道具也无法继续）

**实现工具**：
- 纯数据包即可实现大部分功能
- 如需更强控制，可配合 KubeJS 或 Fabric 模组
- 对于服务器，WorldGuard 提供现成的区域保护

**关键设计原则**：
> 不要试图阻止所有跳关行为——这会导致过度限制，降低玩家体验。相反，应该让"按预期游玩"成为最有趣、最有收益的选择。

---

> 本报告由 Kimi Work 生成，基于 Minecraft Wiki、MC百科、CurseForge、Modrinth 等社区资源整理。
