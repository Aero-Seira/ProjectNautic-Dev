# Entity Texture Features (ETF)

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/[实体纹理特性] entity_texture_features_1.21-neoforge-7.1.jar`
- 标识与版本：mod_id = `entity_texture_features`；版本 `7.1`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

ETF 为实体纹理提供**资源包驱动的增强能力**（verified，mods.toml 描述）：兼容 OptiFine 的随机/自定义实体纹理与发光纹理，并扩展眨眼、玩家皮肤特性等功能。它支撑"视觉风格统一"支柱，为后续引入高质量实体材质资源包（及配合光影的发光效果）提供基础设施。

## 玩家可见行为

- 安装兼容资源包后：生物随机皮肤变体、眼睛等部位发光、眨眼动画。
- 玩家皮肤可启用额外透明/特性支持。
- 未安装相关资源包时基本无感（原版纹理不受影响）。

## 集成关系

- **依赖**：Minecraft（CLIENT 侧声明）；常配合 Entity Model Features（EMF）使用，当前未安装（inferred：后续如需自定义实体模型再补）。
- **协作**：与 Iris/光影（发光纹理在光影下效果更佳）、资源包体系（Resourcify 可下载 ETF 兼容包）。

## 配置意图

- **`config/entity_texture_features.json`**：随机/自定义纹理、发光纹理（`emissiveRenderMode=DULL`，柔和发光避免过曝）、附魔发光、盔甲与纹饰、玩家皮肤特性、眨眼（`blinkFrequency=150`）均启用——保留 ETF 完整特性集，作为资源包能力上限；`textureUpdateFrequency_V2=Fast` 平衡动态纹理流畅度与开销。
- **`config/etf_warnings.json`**：空警告忽略列表，属生成文件。

## 兼容性与性能

- 纯客户端，无存档影响。
- 实体密集场景中随机纹理与发光渲染有额外开销；与视听叠加风险约束一并纳入低配验证。

## 验证

- [ ] 启动测试
- [ ] 功能测试（安装 ETF 兼容资源包抽查随机纹理/发光/眨眼）

## 风险与开放问题

1. 是否与光影的实体发光管线存在双重渲染开销，需实测。
2. 未来若引入 EMF，需同步记录两模组版本搭配。

## 历史

- 2026-07-19: 作为第四批视觉与协作体验批次添加
