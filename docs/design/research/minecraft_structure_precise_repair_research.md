# Minecraft 冒险结构精确自动修复（随时间，非区块重置）调研报告

> 调研日期：2026-07-16
> 核心需求：在**非完全隔离副本**的前提下，实现结构的**精确自动修复**——只修复被玩家破坏/修改的部分，修复过程是**随时间渐进的**，且**不采用粗暴的区块文件重置**

---

## 一、问题定义：为什么要"精确 + 随时间 + 非区块重置"

### 1.1 传统方案的局限

| 方案 | 问题 |
|------|------|
| **区块重置**（替换 .mca 文件） | 会丢失玩家探索痕迹、掉落物、临时实体，且造成明显"闪回"感 |
| **全结构重新放置**（/place structure） | 会覆盖玩家合理修改（如战利品箱已开启状态、门已打开） |
| **瞬间修复**（一次性 setblock） | 大型结构会造成 TPS 骤降，产生卡顿/崩溃 |
| **完全隔离副本** | 前文已讨论，用户明确表示不采用 |

### 1.2 理想行为

```
原始结构状态（Template）          玩家破坏后状态（World）
┌──────────────┐                 ┌──────────────┐
│ ████████████ │                 │ ██░░████████ │ ← 砖块被破坏（需要修复）
│ ██      ██   │                 │ ██      ██   │ ← 门被打开（保留状态）
│ ██[宝箱]██   │                 │ ██[已开]██   │ ← 宝箱已开（保留状态）
│ ████████████ │                 │ ████████████ │
└──────────────┘                 └──────────────┘

精确修复结果（随时间渐进）
┌──────────────┐
│ ████████████ │ ← 只修复被破坏的砖块，其他一切保留
│ ██  🚪  ██   │ ← 门的开关状态保留
│ ██[已开]██   │ ← 宝箱开启状态保留
│ ████████████ │
└──────────────┘
```

---

## 二、核心技术路线对比

### 路线 A：结构方块模板 + 差异检测 + 渐进克隆（数据包方案）

**核心思想**：将原始结构保存为 `.nbt` 模板，但**不直接重新加载整个结构**，而是只修复其中"与模板不符"的方块。

#### 实现原理

```
阶段 1：初始化
  ↓
1. 在结构中预埋"隐藏的结构方块"（Structure Block）
2. 用 Save 模式将整个结构保存为 .nbt 文件（结构模板）
3. 在结构中放置一个"镜像结构"于地下/虚空（作为恢复源）
   或：将 .nbt 保存在数据包中，作为恢复源

阶段 2：差异检测（定时执行，如每 600 ticks = 30秒）
  ↓
1. 遍历结构区域中的每个坐标
2. 比较当前方块 vs 模板中的方块
3. 记录所有"需要修复"的坐标到记分板/存储

阶段 3：渐进修复（每 tick 执行）
  ↓
1. 从"待修复列表"中取出前 N 个方块（如每 tick 修复 5 个）
2. 用 clone 或 setblock 将模板方块复制到目标位置
3. 如果列表为空，修复完成
```

#### 关键命令

```mcfunction
# === 阶段 1：初始化结构模板 ===

# 在结构中放置隐藏的结构方块（Save 模式）
/setblock ~ ~ ~ minecraft:structure_block{mode:"SAVE",name:"my_dungeon",sizeX:32,sizeY:16,sizeZ:32,ignoreEntities:1b}

# 激活保存
/setblock ~ ~1 ~ minecraft:redstone_block

# 结构已保存到 generated/minecraft/structures/my_dungeon.nbt


# === 阶段 2：检测差异（每 30 秒执行一次） ===

# 方案：在地下 Y=-64 处预先放置一个"镜像结构"作为恢复源
# 用 clone 命令比较当前方块与镜像方块

# 定义结构区域（x1,y1,z1 到 x2,y2,z2）
# 在恢复源（x1,y1-100,z1）放置一个完全相同的结构

# 每 tick 扫描一个切片（避免一次扫描过多）
execute if score scan_progress my_dungeon matches 0 run     execute positioned x1 y1 z1 run clone x1 y1 z1 x1+5 y2 z2 x1 y1-100 z2 filtered minecraft:stone_bricks force


# === 阶段 3：渐进修复（每 tick 执行） ===

# 从恢复列表中逐个修复
# 使用标记实体（Marker）存储待修复坐标
execute as @e[type=minecraft:marker,tag=dungeon_repair_queue,limit=5] at @s run     function my_dungeon:repair_single_block

# repair_single_block.mcfunction:
# 获取当前位置在模板中的原始方块
# 使用 clone 从恢复源复制单个方块
# 或使用 setblock 根据模板数据设置方块
clone ~ ~-100 ~ ~ ~-100 ~ ~ ~ ~ replace
kill @s
```

#### 优点
- 纯数据包实现，无需插件/模组
- 精确到方块级别的修复
- 可以保留玩家合理的修改（如门的状态、已开启的箱子）

#### 缺点
- 需要"镜像结构"占用额外空间（或需要外部读取 .nbt）
- 差异检测效率较低（需要遍历大量坐标）
- 无法保留 Block Entity 中的动态数据（如箱子内的物品）

---

### 路线 B：Block-by-Block 渐进修复队列（数据包/命令方块方案）

**核心思想**：不依赖结构模板，而是**预设一个"修复队列"**，按预定顺序逐个修复方块。

#### 实现原理

```
结构被破坏后：
  ↓
1. 检测机制发现结构有损坏（通过 BlockBreakEvent 或定时扫描）
2. 将损坏坐标加入"修复队列"（Marker 实体列表）
3. 每 tick 处理队列前 N 项
4. 每个方块修复时带有"粒子效果"和"音效"，增强沉浸感
5. 修复完成后从队列移除
```

#### 命令示例

```mcfunction
# === repair_tick.mcfunction（每 tick 执行）===

# 每 tick 修复 3 个方块（可配置）
execute as @e[type=minecraft:marker,tag=repair_target,limit=3,sort=arbitrary] at @s run     function my_pack:do_repair

# === do_repair.mcfunction ===
# 在 Marker 位置播放修复特效
particle minecraft:happy_villager ~ ~ ~ 0.3 0.3 0.3 0 10
playsound minecraft:block.stone.place block @a ~ ~ ~ 0.5

# 将方块恢复为原始状态
# 原始状态存储在 Marker 的 NBT 中：
# {Tags:["repair_target"],data:{original_block:"minecraft:stone_bricks",original_state:"facing=north"}}
$execute if data entity @s data.original_state run     setblock ~ ~ ~ $(original_block)[$(original_state)]
$execute unless data entity @s data.original_state run     setblock ~ ~ ~ $(original_block)

# 移除已修复的 Marker
kill @s
```

#### 修复速度配置

| 每 tick 修复数 | 修复 1000 个方块所需时间 | 视觉感受 |
|---------------|------------------------|---------|
| 1 | 50 秒 | 极慢，适合"废墟缓慢重建" |
| 5 | 10 秒 | 中等，适合"结构自我修复" |
| 20 | 2.5 秒 | 较快，适合"快速刷新" |
| 100 | 0.5 秒 | 接近瞬间，适合小型结构 |

---

### 路线 C：区域快照 + 差异对比 + 批量恢复（插件/模组方案）

**核心思想**：类似 **Area Rewind** 和 **CoreProtect** 的设计——保存区域快照，检测变化，只恢复变化的部分。

#### 数据结构设计

```yaml
# 结构修复系统的内存/文件数据结构

StructureRepairTask:
  structure_id: "ancient_ruins_01"
  world: "minecraft:overworld"
  region_bounds: { x1: 100, y1: 50, z1: 100, x2: 132, y2: 66, z2: 132 }
  
  # 原始状态快照（初始化时保存）
  original_snapshot:
    # 使用 Palette + BlockArray 存储（类似 Anvil 格式）
    palette:
      - "minecraft:stone_bricks"
      - "minecraft:stone_bricks[facing=north]"
      - "minecraft:air"
      - "minecraft:chest[facing=east,type=single]"
    blocks: [0,0,1,0,2,0,3,0, ...]  # Palette 索引数组
    
  # 当前待修复列表（差异检测后生成）
  repair_queue: [
    { x: 105, y: 52, z: 110, original_id: 0 },  # 应恢复为 stone_bricks
    { x: 106, y: 52, z: 110, original_id: 1 },  # 应恢复为 stone_bricks[facing=north]
    ...
  ]
  
  # 修复配置
  config:
    blocks_per_tick: 5        # 每 tick 修复数量
    repair_interval: 600      # 多久检测一次差异（ticks）
    particle_effect: "happy_villager"
    sound_effect: "block.stone.place"
    preserve_containers: true # 是否保留容器内容
    preserve_doors: true      # 是否保留门的开关状态
    preserve_redstone: true   # 是否保留红石状态
```

#### 渐进修复算法（伪代码）

```java
// 服务器主线程每 tick 执行
public void onServerTick(ServerTickEvent event) {
    for (StructureRepairTask task : activeTasks) {
        if (task.getRepairQueue().isEmpty()) continue;
        
        int repairedThisTick = 0;
        Iterator<RepairEntry> iter = task.getRepairQueue().iterator();
        
        while (iter.hasNext() && repairedThisTick < task.getBlocksPerTick()) {
            RepairEntry entry = iter.next();
            BlockPos pos = entry.getPosition();
            BlockState originalState = task.getOriginalState(entry.getIndex());
            BlockState currentState = task.getWorld().getBlockState(pos);
            
            // 智能保留策略
            if (shouldPreserve(currentState, originalState)) {
                iter.remove(); // 跳过，不移除
                continue;
            }
            
            // 恢复方块
            task.getWorld().setBlock(pos, originalState, 3);
            
            // 播放效果
            spawnRepairParticles(task.getWorld(), pos);
            playRepairSound(task.getWorld(), pos);
            
            iter.remove();
            repairedThisTick++;
        }
    }
}

// 智能保留策略：判断是否应该保留当前状态
private boolean shouldPreserve(BlockState current, BlockState original) {
    // 如果当前是打开的门/活板门，保留
    if (current.getBlock() instanceof DoorBlock && current.getValue(DoorBlock.OPEN)) {
        return true;
    }
    // 如果当前是已开启的宝箱，保留内容和状态
    if (current.getBlock() instanceof ChestBlock && isChestOpened(current)) {
        return true;
    }
    // 如果当前是红石激活状态，保留
    if (current.hasProperty(RedstoneStoneBlock.POWERED) && current.getValue(RedstoneStoneBlock.POWERED)) {
        return true;
    }
    // 其他情况：恢复为原始状态
    return false;
}
```

#### 差异检测触发时机

| 触发方式 | 说明 | 开销 |
|---------|------|------|
| **定时扫描** | 每 N ticks 全区域扫描一次 | 中等（区域越大开销越高） |
| **事件驱动** | 监听 BlockBreak/BlockPlace 事件，只检测变化坐标 | 极低 |
| **混合模式** | 事件驱动记录变化 + 定时扫描兜底 | 推荐 |

---

### 路线 D：利用 Structure Template API（模组方案，最精确）

**核心思想**：利用 Minecraft 内置的 `StructureTemplate` 和 `StructureTemplateManager`，程序化地将 `.nbt` 模板中的方块"放置"到世界中，但可以精确控制**只放置哪些方块**。

#### 技术要点

```java
// Fabric 1.20.1+ 伪代码
import net.minecraft.world.level.levelgen.structure.templatesystem.StructureTemplate;
import net.minecraft.world.level.levelgen.structure.templatesystem.StructurePlaceSettings;

public class ProgressiveStructureRestorer {
    private StructureTemplate template;
    private BlockPos origin;
    private List<StructureTemplate.StructureBlockInfo> allBlocks;
    private Queue<StructureTemplate.StructureBlockInfo> repairQueue;
    
    public ProgressiveStructureRestorer(StructureTemplate template, BlockPos origin) {
        this.template = template;
        this.origin = origin;
        
        // 解析模板中的所有方块
        this.allBlocks = template.palettes.get(0).blocks();
        
        // 初始化时创建修复队列（按某种顺序排列）
        this.repairQueue = new LinkedList<>(allBlocks);
    }
    
    public void tick(ServerLevel level) {
        int blocksPerTick = 5;
        int repaired = 0;
        
        while (!repairQueue.isEmpty() && repaired < blocksPerTick) {
            StructureTemplate.StructureBlockInfo info = repairQueue.poll();
            BlockPos worldPos = origin.offset(info.pos());
            BlockState current = level.getBlockState(worldPos);
            BlockState expected = info.state();
            
            // 差异检测：当前方块是否与模板一致
            if (!current.equals(expected)) {
                // 智能保留判断
                if (!shouldPreserve(current, expected)) {
                    level.setBlock(worldPos, expected, 3);
                    spawnParticles(level, worldPos);
                    repaired++;
                }
            }
        }
        
        if (repairQueue.isEmpty()) {
            // 修复完成，可以重新填充队列（周期性修复）
        }
    }
}
```

#### StructureBlockInfo 的优势

`StructureTemplate.StructureBlockInfo` 包含：
- `pos()` — 方块在模板内的相对坐标
- `state()` — 方块的 BlockState（包括所有属性）
- `nbt()` — Block Entity 的 NBT 数据（如箱子内容、告示牌文字等）

这意味着可以**精确恢复**包括 Block Entity 在内的完整方块状态。

---

### 路线 E：Anvil Region 文件直接操作（最底层，风险最高）

**核心思想**：直接读写世界数据的 `.mca` 文件，在**不加载区块**的情况下修改方块数据。

#### 技术原理

Minecraft 的 Anvil 格式：
```
region/r.0.0.mca
  ├── Chunk 1 (NBT Compound)
  │     ├── Level
  │     │     ├── Sections[]
  │     │     │     ├── Palette[]   ← 方块类型列表
  │     │     │     ├── BlockStates ← Palette 索引的 packed long array
  │     │     │     └── BlockEntities[] ← 方块实体数据
```

#### 直接操作的思路

```python
# 伪代码：Python 直接修改 .mca 文件
from quarry.types.nbt import RegionFile

# 打开区域文件
region = RegionFile.open("region/r.0.0.mca")

# 读取某个 chunk
chunk_nbt = region.load_chunk(chunk_x, chunk_z)

# 修改 BlockStates
sections = chunk_nbt.body['Level']['Sections']
for section in sections:
    palette = section['Palette']
    blockstates = section['BlockStates']
    
    # 找到需要恢复的方块索引
    # 修改 blockstates 数组中的对应值
    # ...

# 写回
region.save_chunk(chunk_x, chunk_z, chunk_nbt)
```

#### 为什么不推荐

| 问题 | 说明 |
|------|------|
| **文件锁定** | 服务端运行时锁定 .mca 文件，无法安全写入 |
| **缓存不一致** | 服务端内存中的 chunk 数据与磁盘数据不同步 |
| **数据损坏风险** | 格式错误可能导致整个区域文件损坏 |
| **无光照更新** | 直接修改方块数据不会触发光照重新计算 |
| **客户端不同步** | 已加载的 chunk 需要重新发送给客户端 |

**结论**：除非在关服状态下执行，否则不推荐直接操作 .mca 文件。

---

## 三、推荐方案："事件驱动 + 队列渐进 + 智能保留" 三层架构

### 3.1 架构概览

```
┌─────────────────────────────────────────────────────────────┐
│                    结构修复管理器                            │
├─────────────┬─────────────┬─────────────┬─────────────────┤
│ 事件监听器   │ 定时扫描器   │ 修复调度器   │ 智能保留策略     │
├─────────────┼─────────────┼─────────────┼─────────────────┤
│ BlockBreak  │ 每 30s 扫描  │ 每 tick 取  │ 门/活板门状态    │
│ BlockPlace  │ 全区域 diff  │ 队列前 N 项 │ 宝箱开启状态     │
│ BlockExplode│ 兜底检测     │ 渐进修复    │ 红石信号状态     │
│ EntityExplode│            │ 粒子+音效   │ 玩家放置火把     │
└─────────────┴─────────────┴─────────────┴─────────────────┘
                              ↓
                    ┌─────────────────┐
                    │   修复队列       │
                    │ [pos, original] │
                    └─────────────────┘
```

### 3.2 修复流程

```
[结构初始化]
    ↓
1. 保存原始结构快照（StructureTemplate / BlockState 数组）
2. 注册结构区域（AABB 边界框）
3. 启动事件监听和定时扫描
    ↓
[运行期]
    ↓
玩家破坏方块 ──→ 事件监听器捕获 ──→ 坐标加入修复队列
定时扫描发现差异 ──→ 坐标加入修复队列
    ↓
[修复期]
    ↓
每 tick：从队列取前 N 个
  ↓
对每个坐标：
  a. 读取当前 BlockState
  b. 智能保留判断（是否跳过）
  c. 如需要修复：setBlock() + 粒子 + 音效
  d. 从队列移除
    ↓
[完成]
    ↓
队列为空 → 等待下一次触发
```

### 3.3 智能保留策略（白名单）

以下状态的方块**不应被修复**（保留玩家/环境造成的合理变化）：

| 类型 | 具体规则 | 理由 |
|------|---------|------|
| **门/活板门/栅栏门** | 保持当前开关状态 | 玩家打开的门应该保持打开 |
| **容器（箱子/木桶等）** | 保持已开启状态和内容物 | 玩家拿走的物品不应恢复 |
| **红石元件** | 保持激活/非激活状态 | 玩家触发的红石机关应保留 |
| **火把/灯笼** | 如果玩家放置了额外的光源，保留 | 合理的环境改造 |
| **水流/岩浆流** | 保持当前流动状态 | 避免破坏红石/水机关 |
| **作物** | 保持当前生长阶段 | 自然生长的作物不应重置 |

---

## 四、具体实现方案推荐

### 方案 1：数据包实现（适合纯原版/轻量整合包）

**核心工具**：Marker 实体 + 存储 + schedule function

```mcfunction
# === init.mcfunction ===
# 创建记分板用于追踪结构状态
scoreboard objectives add structure_repair dummy

# === detect_changes.mcfunction（每 30 秒执行一次）===
# 使用 execute positioned 遍历结构区域
# 对于每个坐标，比较当前方块与"恢复源"（地下镜像结构）
# 如果发现差异，在该位置召唤 Marker

execute positioned 100 50 100 if block ~ ~ ~ minecraft:air unless block ~ ~-100 ~ minecraft:air run     summon minecraft:marker ~ ~ ~ {Tags:["repair_needed"],data:{source_y:-100}}

# === repair_tick.mcfunction（每 tick 执行）===
# 逐个修复
execute as @e[type=minecraft:marker,tag=repair_needed,limit=5,sort=random] at @s run     function my_pack:repair_block

# === repair_block.mcfunction ===
# 从恢复源复制方块
clone ~ ~-100 ~ ~ ~-100 ~ ~ ~ ~ replace
# 播放效果
particle minecraft:happy_villager ~ ~ ~ 0.2 0.2 0.2 0 5
playsound minecraft:block.stone.place block @a ~ ~ ~ 0.3
# 移除 Marker
kill @s
```

**缺点**：数据包的差异检测能力有限，需要预置"恢复源"结构。

### 方案 2：KubeJS 实现（适合 Fabric/Forge 整合包）

```javascript
// server_scripts/structure_repair.js

// 结构定义
const STRUCTURES = {
    "ancient_ruins": {
        world: "minecraft:overworld",
        bounds: { x1: 100, y1: 50, z1: 100, x2: 132, y2: 66, z2: 132 },
        blocksPerTick: 5,
        scanInterval: 600, // 30 秒
        preserve: ["door", "trapdoor", "chest", "barrel", "redstone"]
    }
};

// 修复队列
let repairQueues = new Map();

// 初始化：保存结构快照
ServerEvents.loaded(event => {
    for (let [id, config] of Object.entries(STRUCTURES)) {
        let snapshot = captureSnapshot(config);
        Utils.server.persistentData.structureSnapshots[id] = snapshot;
    }
});

// 事件驱动：方块破坏时加入队列
BlockEvents.broken(event => {
    let pos = event.block.pos;
    for (let [id, config] of Object.entries(STRUCTURES)) {
        if (isInBounds(pos, config.bounds)) {
            addToRepairQueue(id, pos);
        }
    }
});

// 定时扫描（兜底）
ServerEvents.tick(event => {
    let server = event.server;
    let tickCount = server.tickCount;
    
    for (let [id, config] of Object.entries(STRUCTURES)) {
        // 每 scanInterval ticks 扫描一次
        if (tickCount % config.scanInterval === 0) {
            scanForChanges(id, config);
        }
        
        // 每 tick 执行修复
        processRepairQueue(id, config, server);
    }
});

function processRepairQueue(id, config, server) {
    let queue = repairQueues.get(id) || [];
    let world = server.getLevel(config.world);
    let snapshot = server.persistentData.structureSnapshots[id];
    
    let repaired = 0;
    while (queue.length > 0 && repaired < config.blocksPerTick) {
        let entry = queue.shift();
        let pos = BlockPos(entry.x, entry.y, entry.z);
        let currentBlock = world.getBlock(pos);
        let expectedBlock = snapshot.blocks[entry.index];
        
        // 智能保留判断
        if (shouldPreserve(currentBlock, expectedBlock, config.preserve)) {
            continue;
        }
        
        // 恢复方块
        world.setBlock(pos, expectedBlock.state, 3);
        
        // 特效
        world.spawnParticles("happy_villager", pos.x + 0.5, pos.y + 0.5, pos.z + 0.5, 5, 0.2, 0.2, 0.2, 0);
        world.playSound(null, pos, "block.stone.place", "blocks", 0.5, 1.0);
        
        repaired++;
    }
    
    repairQueues.set(id, queue);
}

function shouldPreserve(current, expected, preserveList) {
    let blockId = current.id.toString();
    
    // 门：保留开关状态
    if (preserveList.includes("door") && blockId.includes("door")) {
        return true; // 保持当前状态
    }
    
    // 容器：保留已开启状态
    if (preserveList.includes("chest") && (blockId.includes("chest") || blockId.includes("barrel"))) {
        return true;
    }
    
    // 红石：保留激活状态
    if (preserveList.includes("redstone") && blockId.includes("redstone")) {
        return true;
    }
    
    return false;
}
```

### 方案 3：Fabric 模组实现（最高性能、最精确）

```java
// 核心类设计
public class StructureRepairManager {
    private final Map<ResourceLocation, StructureRepairTask> tasks = new HashMap<>();
    
    public void registerStructure(ResourceLocation id, BlockPos origin, 
                                   StructureTemplate template, RepairConfig config) {
        StructureRepairTask task = new StructureRepairTask(id, origin, template, config);
        tasks.put(id, task);
    }
    
    @SubscribeEvent
    public void onServerTick(ServerTickEvent event) {
        for (StructureRepairTask task : tasks.values()) {
            task.tick(event.getServer().overworld());
        }
    }
    
    @SubscribeEvent
    public void onBlockBreak(BlockEvent.BreakEvent event) {
        BlockPos pos = event.getPos();
        for (StructureRepairTask task : tasks.values()) {
            if (task.contains(pos)) {
                task.scheduleRepair(pos);
            }
        }
    }
}

public class StructureRepairTask {
    private final StructureTemplate template;
    private final BlockPos origin;
    private final RepairConfig config;
    private final Queue<BlockPos> repairQueue = new LinkedList<>();
    private final Map<BlockPos, BlockState> originalStates = new HashMap<>();
    
    public void tick(ServerLevel level) {
        int limit = config.blocksPerTick();
        int repaired = 0;
        
        while (!repairQueue.isEmpty() && repaired < limit) {
            BlockPos pos = repairQueue.poll();
            BlockState current = level.getBlockState(pos);
            BlockState expected = getExpectedState(pos);
            
            if (shouldRestore(current, expected)) {
                level.setBlock(pos, expected, Block.UPDATE_ALL);
                spawnRepairEffects(level, pos);
                repaired++;
            }
        }
    }
    
    private boolean shouldRestore(BlockState current, BlockState expected) {
        // 如果当前与预期一致，不需要修复
        if (current.equals(expected)) return false;
        
        // 智能保留策略
        Block block = current.getBlock();
        
        // 保留打开的门
        if (block instanceof DoorBlock && current.getValue(DoorBlock.OPEN)) {
            return false;
        }
        
        // 保留容器（即使空了也不恢复内容）
        if (block instanceof AbstractChestBlock) {
            return false;
        }
        
        // 保留红石信号状态
        if (current.hasSignal) {
            return false;
        }
        
        return true;
    }
    
    private void spawnRepairEffects(ServerLevel level, BlockPos pos) {
        // 粒子效果
        level.sendParticles(
            ParticleTypes.HAPPY_VILLAGER,
            pos.getX() + 0.5, pos.getY() + 0.5, pos.getZ() + 0.5,
            8, 0.3, 0.3, 0.3, 0
        );
        
        // 音效
        level.playSound(null, pos, SoundEvents.STONE_PLACE, 
                        SoundSource.BLOCKS, 0.5f, 1.0f);
    }
}
```

---

## 五、性能考量

### 5.1 修复频率与 TPS 影响

| blocks_per_tick | 修复 100 个方块 | 修复 1000 个方块 | TPS 影响 |
|----------------|----------------|-----------------|---------|
| 1 | 5 秒 | 50 秒 | 几乎无感知 |
| 5 | 1 秒 | 10 秒 | 轻微 |
| 20 | 0.25 秒 | 2.5 秒 | 中等 |
| 100 | 0.05 秒 | 0.5 秒 | 明显卡顿 |

**推荐**：对于正常游玩，**5-10 blocks/tick** 是最佳平衡点；对于"废墟缓慢重建"的视觉效果，**1-2 blocks/tick** 更有沉浸感。

### 5.2 差异检测开销

| 检测方式 | 时间复杂度 | 空间复杂度 | 推荐场景 |
|---------|-----------|-----------|---------|
| 全区域扫描 | O(n³) | O(1) | 小型结构（< 32³） |
| 事件驱动 | O(1) | O(k) | 所有场景（推荐） |
| 混合模式 | O(1) avg, O(n³) worst | O(k) | 大型结构 |

### 5.3 减少检测开销的技巧

1. **空间哈希**：将结构区域划分为子区域，只检测玩家附近的子区域
2. **惰性检测**：只在玩家离开结构后才启动修复
3. **优先级队列**：优先修复"可见"的方块（玩家视线范围内）

---

## 六、与 ProjectNautic 的结合建议

### 6.1 推荐技术栈

对于 ProjectNautic 模组包，**推荐采用 KubeJS + 数据包混合方案**：

```
ProjectNautic/
├── kubejs/
│   ├── server_scripts/
│   │   └── structure_repair.js      ← KubeJS 核心逻辑
│   └── startup_scripts/
│       └── structure_registry.js    ← 注册需要自动修复的结构
├── data/
│   └── projectnautic/
│       ├── structures/               ← 结构模板 .nbt
│       │   ├── sunken_temple.nbt
│       │   └── coral_ruins.nbt
│       └── functions/
│           └── repair/
│               ├── detect.mcfunction ← 差异检测
│               └── effects.mcfunction← 修复特效
```

### 6.2 设计模式建议

| 结构类型 | 修复策略 | 修复速度 | 保留规则 |
|---------|---------|---------|---------|
| **海底神庙** | 事件驱动 + 定时扫描 | 5 blocks/tick | 保留门/宝箱状态 |
| **沉船遗迹** | 事件驱动 | 3 blocks/tick | 保留所有红石/容器 |
| **珊瑚迷宫** | 定时扫描（低频率） | 10 blocks/tick | 保留玩家放置的火把 |
| **Boss 竞技场** | 事件驱动（战斗结束后） | 20 blocks/tick | 完全重置（无保留） |

### 6.3 视觉叙事增强

渐进修复不仅是技术需求，更是**叙事工具**：

```
修复效果建议：
- 水下结构：气泡粒子 + 海晶石放置音效
- 废墟结构：尘土粒子 + 石块碎裂反向音效
- 魔法结构：末地烛粒子 + 附魔音效
- 自然结构：树叶粒子 + 骨粉使用音效
```

---

## 七、总结

实现"结构的精确自动修复（随时间，非区块重置）"的核心技术要点：

| 要点 | 关键决策 |
|------|---------|
| **修复精度** | 采用 BlockState 级别的差异检测，只修复变化的部分 |
| **渐进修复** | 每 tick 修复 N 个方块（推荐 5-10），避免卡顿 |
| **非区块重置** | 不使用 .mca 文件替换，而是通过 `setBlock`/`clone` 逐个恢复 |
| **智能保留** | 白名单策略保留玩家合理的修改（门状态、容器内容等） |
| **触发机制** | 事件驱动为主（BlockBreak/BlockPlace），定时扫描兜底 |
| **实现路径** | KubeJS（推荐）> 数据包 > Fabric 模组 > 服务端插件 |

对于 **ProjectNautic**，最推荐的方案是：

> **KubeJS 事件监听 + Marker 实体修复队列 + 结构模板快照**

- 用 KubeJS 监听方块破坏事件，精确记录变化坐标
- 用数据包函数或 KubeJS 的 `ServerEvents.tick` 实现渐进修复
- 为每个结构预置 `.nbt` 模板作为"原始状态快照"
- 通过配置化的"保留规则"确保玩家合理的修改不被覆盖

---

> 本报告由 Kimi Work 生成，基于 Minecraft Wiki、Fabric API 文档、quarry NBT 库文档、Area Rewind 插件源码等社区资源整理。
