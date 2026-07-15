# Particular

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/particular-1.21.1-NeoForge-1.5.5.jar`
- 标识与版本：particular 1.5.5
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Particular 为 ProjectNautic 提供大尺度环境粒子效果，包括瀑布水花、雨滴涟漪、萤火虫、落叶、洞穴尘埃、气泡等。它直接支撑"视觉沉浸感"支柱，将地形与水体的动态表现得更加自然，而不改变方块或世界生成逻辑。

该模组与 Visuality、Ambiance、Effectual 同属环境粒子层：Particular 负责宏观自然环境现象，Visuality 负责接触点微粒子，Ambiance 负责特定对象氛围，Effectual 负责玩家状态与交互。

## 玩家可见行为

- **可发现性**：玩家靠近瀑布会看到水雾与水花，雨天看到水面涟漪，夜晚在温暖生物群系看到萤火虫，森林看到落叶，洞穴看到漂浮尘埃，水下看到气泡；吃蛋糕时也有特殊粒子。
- **交互**：无需主动操作；效果由环境与玩家行为自动触发。
- **进度/奖励**：无内置进度；环境粒子作为探索自然景观的感官奖励。
- **摩擦/失败**：瀑布与落叶效果在低配置设备上可能掉帧；若与同样修改水体渲染的模组叠加，可能出现视觉冲突。

## 集成关系

- **依赖**：Minecraft 1.21.1（required, CLIENT）。
- **冲突**：无已知严重冲突。
- **协作**：与 **Visuality**、**Ambiance**、**Effectual** 同属环境粒子层。
- **配方/标签**：无直接参与。
- **任务**：无。
- **世界生成**：无。
- **UI**：无新增 UI，配置依赖配置文件。
- **资源**：依赖内置粒子纹理，无需额外资源包。
- **脚本**：无直接 KubeJS 交互。
- **加载顺序假设**：标准 CLIENT 端加载。

## 配置意图

`config/particular-common.toml` 当前启用了全部主要效果，并在参数上保持克制：

- **启用效果**：水花（`waterSplash=true`）、瀑布喷雾（`waterfallSpray=true`）、萤火虫（`fireflies=true`）、落叶（`fallingLeaves=true`）、洞穴尘埃（`caveDust=true`）、箱子/桶/灵魂沙气泡（`chestBubbles=true`、`barrelBubbles=true`、`soulSandBubbles=true`）、雨滴涟漪（`rainRipples=true`）、熔岩飞溅（`lavaSplash=true`）等全部开启。意图是在视觉沉浸与性能之间取得平衡，默认启用全套自然环境效果。
- **萤火虫设置**：`startTime=12000`、`endTime=23000`，仅在夜间出现；`minTemp=0.5`、`maxTemp=0.99`，避免在过冷或过热生物群系生成；`canSpawnInRain=false`，雨天不生成；`biomes` 与 `spawnBlocks` 为空，使用默认生成规则。
- **流体兼容**：`waterLikeFluids` 中预置了 TFC 相关流体（`tfc:river_water`、`tfc:salt_water`、`tfc:spring_water` 等），说明配置已为 TerraFirmaCraft 风格整合预留兼容，但当前包中这些模组不存在，保留作为未来扩展。
- **落叶设置**：`spawnChance=60`（值越大越稀疏），`spawnRipples=true`、`layFlatOnGround=true`，落叶落地会产生涟漪并平躺在地面，增强真实感。
- **水花设置**：`minFallDistance=0.0` 使最轻微落水也有反馈；`splashOpacity=0.75`；`softEntryParticles=true` 对小落水使用柔和气泡；`cuboidSplashDroplets=true` 使用体素化水滴以获得统一视觉风格；`waterCuboidBiomeTint=true` 让瀑布喷雾按生物群系水色着色。
- **洞穴尘埃**：`spawnChance=700`、`baseMaxAge=200`、`color=8421504`（中灰色），并排除繁茂洞穴、滴水石洞穴与深暗之域，避免这些已具强烈视觉特征的区域被尘埃遮挡。

## 兼容性与性能

- **客户端/服务端范围**：仅 **CLIENT**。
- **存档影响**：无。
- **已知不兼容**：无。
- **资源成本**：瀑布喷雾、落叶、萤火虫等效果对 GPU 粒子填充率有持续压力；低端设备上大量瀑布场景可能导致帧率下降。
- **缓解措施**：通过 `spawnChance`、`opacity`、`minFallDistance` 等参数控制密度；所有效果均可通过配置文件单独关闭。

## 验证

- [ ] 启动或加载测试
- [ ] 功能测试
- [ ] 多人游戏测试（如适用）
- [ ] 性能或世界生成测试（如适用）
- [ ] 相关系统的回归检查

## 风险与开放问题

1. 当前配置中 TFC 流体条目指向未安装的模组，未来若引入 TFC 需重新验证；若不引入，这些条目为无害遗留。
2. 瀑布喷雾与落叶为粒子密集型效果，需在目标硬件上测试密集场景下的帧率稳定性。

## 历史

- 2026-07-16: 作为第三批 QoL 模组添加
