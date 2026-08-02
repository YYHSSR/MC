---
name: mc-skill
description: 面向 Minecraft Forge 1.20.1 模组的工程化开发、最小风险优化、构建和故障诊断；在需要修改 Forge 源码、审计兼容性、构建 JAR 或处理 Gradle/Python 环境时使用。
---

# MC-Skill

目标：仅处理 Minecraft 1.20.1 Forge 模组；以功能、配置、存档、公开 API、Mixin 注入点和资源加载正确性不变为前提，得到更小、更兼容、运行更流畅且启动更快的实现。

## 交流与工具

- 执行读写、构建或命令前，用不超过几句话说明目的和步骤；请求权限时用一句话说明直接目的。
- 最终回答用 1–3 句话，按“结论 → 证据/影响 → 下一步”表达；像工程问题一样说明风险和依据，不猜测、不客套、不展示无关输出。
- 需要 Python 时只能调用 `D:\python\miniconda\envs\py10\python.exe`；不得使用系统 Python、`python`、`py` 或其他 Conda 环境。

## 开发与优化

1. 先识别 Forge 1.20.1、JDK 17、模块职责、调用频率、兼容范围和可复现问题，再编辑。
2. 保持所有原功能与用户可见行为不变；不要重命名公开 API、改变事件顺序、配置/存档格式、资源包优先级或 Mixin 注入点。
3. 只修改可证实热点：每 tick/帧遍历、重复解析、临时集合、重 I/O、无上限线程池。无可证实收益时标注“无需重构”。
4. 使用语义等价的直接循环替代热点 Stream；资源扫描、资源回调和依赖解析禁止 `parallelStream()`；后台线程池最大为可用 CPU 核心数。
5. 不改渲染核心、第三方 vendored 代码、跨加载器兼容层或存档逻辑，除非有明确日志、复现步骤和验证方案。
6. 不替换 Forge 的 `ResourcePackLoader`、`PathPackResources`、`DelegatingPackResources` 或资源枚举链路；任何资源丢失问题优先撤销相关资源缓存/拦截实现，而非添加资源覆盖包。

## 构建与交付

- 使用 JDK 17：`C:\Program Files\Java\jdk-17.0.20_8`。先读取项目 Wrapper 与插件版本；默认 Gradle 8.11，但插件要求其他 Gradle 8.x 时使用该项目 Wrapper，不强降级/升级。
- 多加载器项目只执行 Forge 发布任务；Forge 子项目优先 `:forge:remapJar` 或 `:forge:build --configure-on-demand --no-daemon`，单 Forge 项目使用 `reobfJar` 或 `build --no-daemon`。
- 仅在 `BUILD SUCCESSFUL` 后交付；排除 `-dev`、`-sources`、`-javadoc`、`-shadow`、`-slim` 和未重混淆 JAR。
- 验证最终 JAR 含 `META-INF/mods.toml`、正确 Mixin 清单与 Forge 1.20.1 元数据；复制到 `E:\CODE\MC\mods\<mod>-forge-1.20.1.jar` 前确认不覆盖同名文件。
- Gradle 缓存清理仅删除已确认未使用的 `C:\Users\zf\.gradle\wrapper\dists` 目录；保留 `caches`、`jdks`、`native` 和仍被项目 Wrapper 使用的 Gradle 8.x。
