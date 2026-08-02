# Forge 1.20.1 Java 模组开发标准模板 (Template Mod)

这是一个预先配置好的 **Minecraft 1.20.1 (Forge 47.3.0, Java 17)** 官方 MDK 模组开发模板。支持官方 Mojang 映射、Mixin 扩展，并将 Gradle User Home 固化在项目目录内部，免除对 C 盘空间的占用。

---

## 🌟 项目特性

- **官方 Forge 1.20.1 (ForgeGradle 6.x) MDK 架构**：纯正官方规范，支持最新 Forge 47.3.0 及 Java 17 Toolchain。
- **镜像源优化**：预置阿里云与腾讯云 Maven 镜像，中国大陆网络环境下无需科学上网即可快速解析依赖。
- **本地化 Gradle User Home (`gradle_home`)**：Gradle 缓存、解压包及下载资源全部存放在项目根目录 `gradle_home/` 中，零占用 C 盘空间。
- **自带 Mixin 支持**：已预配 SpongePowered Mixin 0.8.5 处理器及示例配置文件 `template_mod.mixins.json`。
- **开源协议**：采用 [CC BY-NC 4.0](LICENSE) 协议，严格禁止商业用途，同时允许开源交流、学习与非商业二次开发。

---

## 🚀 快速上手：基于此模板新建模组

当你使用此模板开发新模组时，只需以下 3 步修改：

### 1. 修改 `gradle.properties`
根据新模组信息修改以下字段：
```properties
mod_id=my_awesome_mod
mod_name=My Awesome Mod
mod_version=1.0.0
mod_authors=YourName
mod_description=Description of your mod
maven_group=com.yourname.myawesomemod
```

### 2. 修改 Java 包名与主类
1. 将 `src/main/java/com/example/template` 重命名为你的包路径（如 `com/yourname/myawesomemod`）。
2. 修改主类中的 `@Mod("my_awesome_mod")` 标识。
3. 若使用 Mixin，将 `src/main/resources/template_mod.mixins.json` 重命名为 `<your_mod_id>.mixins.json`，并同步更新文件内的 `"package"` 字段。

### 3. 常用开发命令

在项目根目录下打开命令行/终端，执行以下命令：

- **生成 IDE 启动配置 (VSCode)**：
  ```cmd
  .\gradlew.bat genVSCodeRuns
  ```
- **启动游戏客户端进行测试**：
  ```cmd
  .\gradlew.bat runClient
  ```
- **启动游戏服务端**：
  ```cmd
  .\gradlew.bat runServer
  ```
- **构建打包 Mod Jar 包**：
  ```cmd
  .\gradlew.bat build
  ```
  打包产物将输出在 `build/libs/` 目录下。
- **清理构建临时文件**：
  ```cmd
  .\gradlew.bat clean
  ```

---

## 📁 目录结构说明

```
.
├── build.gradle               # ForgeGradle 6 官方主构建脚本
├── settings.gradle            # 依赖与插件仓库配置 (内置阿里云/腾讯云镜像)
├── gradle.properties          # 模组与 Gradle 全局配置文件 (含 gradle.user.home 重定向)
├── gradlew / gradlew.bat      # Gradle Wrapper 启动脚本 (已注入 GRADLE_USER_HOME)
├── gradle_home/               # 项目专用 Gradle User Home 缓存目录 (替代 C:\Users\xxx\.gradle)
├── MC-Skill/                  # AI 辅助开发技能资源
├── src/
│   └── main/
│       ├── java/              # Java 源代码
│       └── resources/         # 模组资源文件 (mods.toml, pack.mcmeta, mixins.json)
└── .vscode/                   # VSCode 配置文件 (已配置本地 gradle_home)
```