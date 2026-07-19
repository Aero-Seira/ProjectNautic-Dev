# Resourcify

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/Resourcify (1.21.1-neoforge)-1.8.5.jar`
- 标识与版本：mod_id = `resourcify`；版本 `1.8.5`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）；Kotlin 编写（依赖 kotlinforforge）

## 设计作用

Resourcify 提供**游戏内资源包 / 数据包 / 光影浏览器**（verified，mods.toml 描述），玩家无需退出游戏即可从 Modrinth 搜索、下载并启用资源。它降低玩家个性化客户端的门槛，同时把资源来源收敛到 Modrinth 这一可审计渠道，符合长线运营的可维护性要求。

## 玩家可见行为

- 在资源包/光影/数据包选择界面内嵌浏览与下载入口，默认源为 Modrinth。
- 下载后可直接启用，无需手动拷贝文件。

## 集成关系

- **依赖**：Minecraft `[1.21,1.21.1]`、NeoForge（required, BOTH）；运行时依赖 Kotlin for Forge（已就位，见 library/kotlinforforge.md）。
- **协作**：与 Iris（光影管理界面）、官方默认光影共存；下载的光影进入 `shaderpacks/`。

## 配置意图

- **`config/resourcify.json`**：`defaultService=Modrinth`（收敛资源渠道）；资源包、数据包、光影、世界四类浏览均启用；`adsEnabled=true` 保留默认（inferred：未做 deliberate 调整）。

## 兼容性与性能

- 纯客户端功能（displayTest=IGNORE_ALL_VERSION），服务端无需安装；无存档影响。
- 下载内容属玩家个人数据，不纳入版本控制。

## 验证

- [ ] 启动测试
- [ ] 功能测试（搜索/下载/启用资源包与光影）

## 风险与开放问题

1. 玩家自行下载的资源可能与整合包视听基线冲突（如重叠粒子资源包），需在玩家文档中提示。
2. 数据包下载能力若被用于服务器世界，可能绕过版本控制，需运营层面约束。

## 历史

- 2026-07-19: 作为第四批视觉与协作体验批次添加
