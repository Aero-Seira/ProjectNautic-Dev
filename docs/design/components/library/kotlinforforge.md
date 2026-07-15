# Kotlin for Forge

## 记录

- 类型：mod
- 状态：active
- 证据可信度：verified
- 来源路径：`mods/kotlinforforge-5.12.0-all.jar`
- 标识与版本：mod_id = `kotlinforforge`；版本 `5.12.0`
- 加载器或包格式：NeoForge 21.1.235（Minecraft 1.21.1）

## 设计作用

Kotlin for Forge 是 Kotlin 运行时与 Forge/NeoForge 适配层，为使用 Kotlin 编写的模组提供标准库、协程、序列化与模组入口支持。它在包内属于语言基础设施层：没有它，依赖 Kotlin 的模组将无法初始化类或加载入口。

在 ProjectNautic 中，它支撑**技术一致性**支柱，使 Kotlin 模组（如 Fzzy Config）能在 NeoForge 环境下与 Java 模组共存，而无需玩家手动准备 Kotlin 依赖。

## 玩家可见行为

普通玩家不会直接看到 Kotlin for Forge 的名称，也没有任何可交互功能。它仅在后台提供 Kotlin 语言运行时。

## 集成关系

- **依赖**：Minecraft `[1.20.6,1.21.9)`（required, BOTH）
- **被依赖**：
  - **fzzy_config**：需要 Kotlin for Forge `>=5.3.0`（required, BOTH）
- **冲突**：当前批次中无已知硬冲突。
- **协作**：作为 Kotlin 模组的基础运行时，与未来可能添加的其他 Kotlin 模组共享。
- **配方/标签**：无直接参与。
- **任务**：无。
- **世界生成**：无。
- **UI**：无直接 UI。
- **资源**：无。
- **脚本**：无 KubeJS 脚本交互需求。
- **加载顺序假设**：标准。Kotlin for Forge 使用自定义 modLoader `kotlinforforge`，NeoForge 会确保其在消费者之前就绪。

## 配置意图

Kotlin for Forge 本身不暴露 Minecraft 级别的配置文件。其行为由以下因素决定：

- **版本锁定**：fzzy_config 要求 Kotlin for Forge `>=5.3.0`。当前安装的 `5.12.0` 满足此约束。更新 Kotlin for Forge 前需确认所有 Kotlin 模组的兼容性声明。
- **JarJar 结构**：该 jar 通过 JarJar 内嵌 Kotlin 标准库、协程、序列化与 KFF 子模块，保持单文件部署，避免依赖冲突。

## 兼容性与性能

- **客户端/服务端范围**：BOTH。Kotlin 运行时在所有使用 Kotlin 代码的侧端都需要存在。
- **存档影响**：无直接存档影响。
- **已知不兼容**：当前批次中无已知冲突。
- **资源成本**：Kotlin 标准库与协程库会增加启动时的类加载与内存占用，但属于一次性开销，运行时影响很小。
- **缓解措施**：保持与 Kotlin 模组版本同步；避免手动删除或替换；不要与其他 Kotlin 标准库 jar 重复放置。

## 验证

- [ ] 启动或加载测试
- [ ] fzzy_config 功能测试
- [ ] 多人游戏测试（如适用）
- [ ] 类加载无重复 Kotlin 依赖

## 风险与开放问题

1. **版本绑定**：Kotlin for Forge 的更新与 fzzy_config 等 Kotlin 模组紧密耦合，不能独立升级 major 版本。
2. **JarJar 调试复杂度**：内嵌依赖使得排查 Kotlin 版本冲突时不如独立 jar 直观，需要借助日志中的 JarJar 元数据定位。

## 历史

- 2026-07-16: 作为第三批 QoL 模组添加
