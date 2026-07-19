# AsyncParticles

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/[异步粒子] AsyncParticles-21.1.2.0+1.21.1.jar`
- 标识与版本：`asyncparticles` 21.1.2.0
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

AsyncParticles 将粒子更新与渲染剔除逻辑从主线程部分卸载到工作线程，是 ProjectNautic 粒子特效密集场景下的性能保险。它直接支撑整合包的**性能优先**支柱，确保 Particular、Visuality、Ambiance 等视觉模组共存时，大量粒子不会显著拉低帧率或造成卡顿。

该模组定位为视觉体验的幕后守护者：玩家应享受到更流畅的粒子效果，但通常不应感知到 AsyncParticles 本身的存在。

## 玩家可见行为

- **可发现性**：玩家不会在日常游戏中直接看到 AsyncParticles 的界面或提示；其效果体现在粒子密集场景（下雨、战斗、 Create 机械运转）中更稳定的帧率。
- **交互**：无直接交互。所有调优通过配置文件完成，不暴露游戏内 GUI。
- **进度/奖励**：无直接进度关联。
- **摩擦/失败**：若异步同步列表配置遗漏某些粒子的类路径，可能导致该类粒子渲染异常或崩溃；`failBehavior` 设置为 `RAISE_CRASH` / `MARK_AS_SYNC` 会在失败时提供明确反馈而非静默损坏。

## 集成关系

- **依赖**：NeoForge（required, CLIENT）、Minecraft 1.21.1（required, CLIENT）。
- **冲突**：与同样大规模修改粒子渲染管线的模组可能存在冲突；当前未记录具体冲突项。
- **协作**：与 Particular、Visuality、Ambiance、Effectual、Particle Effects 等视觉/粒子模组协同工作；为 Create 的降雨粒子与机械结构提供专门兼容配置。
- **配方/标签**：无。
- **任务**：无。
- **世界生成**：无。
- **UI**：无。
- **资源**：无。
- **脚本**：无 KubeJS 交互。
- **加载顺序假设**：标准客户端加载顺序即可；mixin 配置中声明的类需要在对应模组加载后解析，因此实际由 NeoForge 自动处理。

## 配置意图

以下配置选择反映了整合包对粒子性能与稳定性的权衡，**非全部默认项的逐条记录**。

### `config/asyncparticles/asyncparticles.json`（主配置）

- **particleLimit: `16384`** — 提高单场景粒子数量上限，允许更丰富的视觉效果而不被过早截断。
- **removeIfMissedTick: `false`** — 不在错过 tick 时强制移除粒子，避免复杂场景下粒子突然消失。
- **parallelQueueRemoval / parallelQueueEviction: `true`** — 启用并行队列清理与驱逐，减少主线程在粒子生命周期管理上的开销。
- **particleLightCache: `true`** — 缓存粒子光照计算，降低频繁光照查询的 CPU 消耗。
- **cullUnderwaterParticleType: `true`** — 在水下时按粒子类型剔除，避免不可见粒子浪费 GPU 资源。
- **animationTickMode / particleTickMode: `INTERRUPTIBLE`** — 动画与粒子 tick 使用可中断模式，在性能压力大时允许调度器放弃部分工作以保证主线程响应。
- **gpuOnlyAsyncParticleTick: `false`** — 不限于仅 GPU 侧粒子异步化，扩大收益面。
- **tickWeatherAsync: `true`** — 天气粒子（雨/雪）异步 tick，这是下雨场景卡顿的主要来源之一。
- **deferredTextureTick: `true`** — 延迟纹理 tick，进一步削峰主线程负载。
- **failPerSecLimit (tick): `5`** — tick 侧每秒最多允许 5 次失败，超过后按 `RAISE_CRASH` 处理，防止静默错误累积。
- **particleCulling: `SPHERE`** — 使用球体剔除，兼顾效率与粒子形状包容性。
- **particleRenderingMode: `SYNCHRONOUSLY`** — 渲染仍同步执行，确保与光影、后处理模组的兼容性；仅 tick 与剔除异步化。
- **gpuAcceleration: `true`** — 启用 GPU 加速路径。
- **appendNewParticlesToRenderer: `true`** — 新粒子直接追加到渲染器，减少中间拷贝。
- **cullWeathers: `true`** — 对天气粒子启用剔除。
- **failPerSecLimit (rendering): `20`** — 渲染侧失败阈值较高，超过后标记为同步渲染而非直接崩溃，优先保证画面不中断。
- **syncParticleClasses**: 显式列出需要同步 tick 或渲染的第三方粒子类，包括 Loot Beams、Corail Tombstone、Effective / Lady's Snakes、Better Combat Dummy 等。这些类因线程安全或渲染顺序要求被排除在异步路径之外，是避免崩溃与视觉错误的关键白名单。

### `config/asyncparticles/asyncparticles-mixin.properties`（mixin 调优）

- **particle$splitTick: `false`** — 关闭递归粒子 tick，保持简单可预测的生命周期。
- **safeBlockEntityMap / safeClassInstanceMultiMap / safeLegacyRandomSource: `false`** — 当前未启用额外的线程安全包装，依赖同步列表与 failBehavior 控制风险；若未来加入更多并发粒子模组，可重新评估。
- **particle$noCulling**: 显式禁用 `FXObject`、`SlashParticle` 的粒子剔除，避免视觉特效被错误裁切。
- **particle$noLightCache**: 禁用 Gateway Particle、Firefly Particle、FXObject、Weather Particle 的光照缓存，防止这些依赖动态光照或透明效果的粒子出现渲染错误。
- **particle$lockProvider / lockRequired**: 为 Epic Fight 的 TrailParticle 与 Photon 的 FXObject 配置自旋锁，使其在异步 tick 时仍保持渲染状态一致性。
- **replaceRandom**: 将 AE2 的 LightningArcFX / LightningFX 与 Falling Leaves 的 LeafUtil 替换为多线程安全随机源，避免并发访问崩溃。
- **create$contraptionNoParticleCollision**: 为 Create Big Cannons 的特定机械实体禁用粒子碰撞，防止大型机械上不必要的物理开销。

## 兼容性与性能

- **客户端/服务端范围**：CLIENT ONLY。粒子逻辑完全在客户端执行，服务端无需安装。
- **存档影响**：无。AsyncParticles 不写入存档数据。
- **已知不兼容**：未记录；与 Embeddium 等非 Sodium 渲染管线模组潜在不兼容需谨慎验证。
- **资源成本**：通过工作线程降低主线程 CPU 占用，但会增加少量线程调度与内存开销；粒子上限提升后内存占用相应增加。
- **缓解措施**：通过 `syncParticleClasses` 与 mixin properties 中的白名单/锁机制，将不稳定粒子强制回退到同步路径；`failBehavior` 分级处理 tick 侧崩溃与渲染侧降级。

## 验证

- [ ] 启动或加载测试
- [ ] 功能测试
- [ ] 多人游戏测试（如适用）
- [ ] 性能或世界生成测试（如适用）
- [ ] 相关系统的回归检查

## 风险与开放问题

1. **白名单遗漏风险**：未来新增视觉模组若其粒子类未加入 `syncParticleClasses` 或 mixin 锁列表，可能在异步路径下出现偶发崩溃或渲染错误。每次新增粒子类模组后需回归测试。
2. **天气异步化的视觉同步性**：`tickWeatherAsync: true` 与 `cullWeathers: true` 在极端帧率波动下可能导致雨水轨迹与音效/世界状态出现可感知的错位。

## 历史

- 2026-07-20: `asyncparticles-mixin.properties` 再次重新生成（仍仅时间戳变化，无设计 delta）
- 2026-07-19: `asyncparticles-mixin.properties` 重新生成（仅时间戳变化，mixin 锁/白名单内容未变，无设计 delta）
- 2026-07-16: 作为第三批 QoL 模组添加
