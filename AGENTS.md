# 仓库贡献指南

## 项目结构与模块组织

本项目是基于 Java 8 的 Maven 多模块工程，采用分层 DDD 结构。根目录的 `pom.xml` 统一管理共享版本和所有模块。

- `home-kitchen-api`：公共接口与契约。
- `home-kitchen-domain`：领域模型与业务规则。
- `home-kitchen-application`：用例编排。
- `home-kitchen-infra`：基础设施与持久化适配器。
- `home-kitchen-web`：Spring Boot 应用及面向 Web 的代码。

Java 源码放在 `src/main/java/com/zyz/home/kitchen/<layer>`，资源文件放在 `src/main/resources`，测试放在包结构对应的 `src/test/java` 中。应用入口为 `home-kitchen-web/src/main/java/com/zyz/home/kitchen/web/Main.java`。模块依赖应保持单向：application 依赖 API 和 domain，infra 依赖 domain，web 依赖 API。

## 构建、测试与开发命令

请在仓库根目录执行以下命令：

```powershell
mvn clean test                              # 清理并运行所有模块测试
mvn -DskipTests package                     # 跳过测试进行编译和打包
mvn -pl home-kitchen-web -am package        # 构建 web 模块及其依赖
```

本地开发时，请在 IDE 中启动 `Main`。项目尚未配置 Spring Boot Maven 运行插件；在添加相关支持前，请使用 IDE 的运行配置。

## 编码风格与命名约定

- 使用四空格缩进、UTF-8 编码和兼容 Java 8 的 API。
- 包名使用小写，类名使用 PascalCase，方法和字段使用 camelCase，常量使用 UPPER_SNAKE_CASE。
- 测试类命名为 `<ClassName>Test`，并放在与生产代码对应的包中。
- 在根 `pom.xml` 中声明共享版本；使用方模块应显式声明跨模块依赖。
- 项目尚未配置格式化、代码检查或 Checkstyle 工具。请遵循周围代码风格，避免无关的格式变更。

## 测试规范

项目尚未配置测试框架或覆盖率门槛。新增测试时，优先使用 JUnit 5，并将依赖添加到对应模块；测试应覆盖领域规则和应用工作流。测试方法名应描述具体行为，例如 `rejectsDuplicateRecipeName()`。运行单个模块的测试：

```powershell
mvn -pl home-kitchen-domain -am test
```

在测试建立之前，`mvn clean test` 主要用于验证编译。

## 提交与拉取请求规范

Git 历史目前只包含 `Initial commit` 和 `项目初始化` 等简短、无正文的提交，尚未形成前缀约定。提交标题应简洁并使用祈使语气，不超过 72 个字符；可在正文中说明动机和影响。

拉取请求应说明变更内容、受影响的模块、关联 Issue 和验证命令。Web 界面出现可见变化时，应附上截图。请勿提交凭据、生成的 `target/` 文件或无关的 IDE 变更。

## 配置与安全

运行时配置放在 `home-kitchen-web/src/main/resources`，并通过按环境区分的配置外置密钥。不要提交密码、密钥或私有连接字符串。
