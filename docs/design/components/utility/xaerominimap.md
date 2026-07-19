# Xaero's Minimap 小地图

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/[Xaero的小地图] xaerominimap-neoforge-1.21.1-26.4.2.jar`
- 标识与版本：mod_id = `xaerominimap`；版本 `26.4.2`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Xaero's Minimap 是 ProjectNautic 整合包的客户端小地图模组，提供接近原版风格的全屏小地图、实体雷达、路径点与位置信息共享。它支撑整合包**QoL 工具**与**Vanilla+ 起步**的设计意图：在不破坏原版探索节奏的前提下，为玩家提供基础导航辅助，降低在大型世界中迷路的风险。

Xaero's Minimap 为 CLIENT 侧模组，主要功能不依赖服务端，但与 Xaero's World Map 可形成集成体验。

## 玩家可见行为

- **可发现性**：玩家进入世界后屏幕右上角（默认位置）会出现小地图，显示周围地形、玩家位置与方向。
- **交互**：
  - 默认按键打开小地图设置与路径点管理
  - 鼠标悬停小地图可查看坐标
  - 路径点可用于标记基地、矿洞、兴趣点
  - 实体雷达显示周围生物（可配置）
- **进度/奖励**：无直接进度关联。间接提升探索效率与基地管理便利性。
- **摩擦**：默认风格接近原版，UI 元素可高度自定义，不会给玩家造成突兀感。
- **失败状态**：配置冲突可能导致小地图位置与其他 HUD 模组重叠。

## 集成关系

- **依赖**：Minecraft `1.21.1`、NeoForge `>=21.0.110-beta`、xaerolib `>=1.0`（required, BOTH）；xaeroworldmap `>=1.43.0`（optional, BOTH）；openpartiesandclaims `>=0.27.0`（optional, BOTH）
- **冲突**：当前批次中无已知硬冲突。与 JourneyMap 等其他小地图模组功能重叠，不建议同时安装。
- **协作**：与 Xaero's World Map 可形成完整地图体验（世界地图作为小地图的全屏扩展）。
- **配方/标签**：无直接参与
- **任务**：无
- **世界生成**：无
- **UI**：在屏幕一角叠加小地图面板；提供独立的设置界面
- **资源**：`config/xaero/` 下包含大量默认生成的配置文件与雷达分类定义
- **脚本**：无 KubeJS 脚本交互需求
- **加载顺序**：需在 xaerolib 之后加载

## 配置意图

`config/xaero/` 目录下生成了一系列默认配置文件，包括：

- `lib/client.cfg`、`lib/common.cfg`、`lib/profiles/default.cfg`
- `minimap/client.cfg`、`minimap/common.cfg`、`minimap/profiles/default.cfg`
- `minimap/default_radar_categories_client.json` / `server.json`
- `minimap/profiles/entity_radar_categories/default.cfg.json`
- `minimap/profiles/info_display_config/default.cfg.txt`

这些文件当前均为**生成默认值，未做 deliberate 覆盖**。整合包选择保留 Xaero 的默认小地图体验，原因如下：

- 默认风格已接近原版，符合 Vanilla+ 起步定位。
- 小地图属于高度个人偏好的 UI 元素，强制统一配置可能引起玩家反感。
- 未来可根据长线运营阶段的实际反馈，逐步调整路径点共享、雷达显示等设置。

当前唯一值得关注的是 `config/xaerohud.txt` 与 `config/xaeropatreon.txt` 亦为生成文件，后者为空文件，用于 Patreon 功能标识。

## 兼容性与性能

- **客户端/服务端范围**：标记为 BOTH，但核心功能在 CLIENT 侧。服务端安装可支持部分数据同步与雷达分类配置。
- **存档影响**：无直接存档影响。路径点与地图缓存存储在客户端 `xaero/` 目录下，换设备不保留。
- **已知不兼容**：与 JourneyMap 等其他小地图模组不建议同时安装。
- **资源成本**：小地图需要持续渲染周围地形并缓存地图数据，内存与显存占用随探索范围增长；雷达功能在实体密集区域可能轻微影响 FPS。
- **缓解措施**：默认配置已平衡信息量与性能；若玩家设备较低端，可关闭实体雷达或降低小地图更新频率。

## 验证

- [ ] 启动或加载测试
- [ ] 功能测试（小地图显示、路径点、雷达）
- [ ] 多人游戏测试（路径点与地图数据同步）
- [ ] 性能测试（长时间探索后的内存占用）
- [ ] 与 Xaero's World Map、Jade、AppleSkin 的 HUD 布局回归检查

## 风险与开放问题

1. **HUD 布局冲突**：小地图默认位于右上角，可能与未来的状态栏、效果显示模组重叠，需要统一规划。
2. **地图数据管理**：客户端地图缓存随探索范围膨胀，长期运行后可能占用较多磁盘空间；需考虑是否提供清理指引。
3. **Patreon 功能边界**：`xaeropatreon.txt` 为空文件，未来若启用付费功能需评估对整合包一致性的影响。

## 历史

- 2026-07-19: 升级 26.3.0 → 26.4.2（例行小版本升级，替换旧 jar）；新增 [Ping to Map](../integration/pingtomapxaeros.md) 联动（ping 自动生成临时路径点）
- 2026-07-16: 作为第二批基础模组与 QoL 扩展批次添加
