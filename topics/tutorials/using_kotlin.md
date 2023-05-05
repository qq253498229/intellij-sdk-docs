# 配置 Kotlin 支持

<!-- Copyright 2000-2023 JetBrains s.r.o. and contributors. Use of this source code is governed by the Apache 2.0 license. -->

<link-summary>使用 Kotlin 开发插件的优势和所需配置。</link-summary>

<tldr>

**主页**: [Kotlin](https://kotlinlang.org)

**项目模板**: [](plugin_github_template.md)

</tldr>

<link-summary>使用或针对 Kotlin 编程语言开发插件。</link-summary>

本页面介绍如何使用 [Kotlin](https://kotlinlang.org) 编程语言开发插件。

> 要在 IDE 中实现操作 Kotlin 代码的插件，请配置 Kotlin [plugin dependency](plugin_dependencies.md) (`org.jetbrains.kotlin`)。
> 有关如何支持多种 JVM 语言（包括 Kotlin）的信息，请参阅 [UAST](uast.md) 页面。

## 在 Kotlin 中开发插件的优势

使用 Kotlin 编写 IntelliJ 平台插件与使用 Java 相似。
现有的插件开发者可以通过使用 J2K 转换器（Kotlin 插件的一部分）将样板 Java 类转换为 Kotlin 等效类来开始开发，开发者可以轻松混合和匹配 Kotlin 类和现有的 Java 代码。

除了 [空安全](https://kotlinlang.org/docs/null-safety.html)和 [类型安全构建器](https://kotlinlang.org/docs/type-safe-builders.html)外，Kotlin 语言还提供了许多方便的插件开发功能，使插件更易于阅读和更简单维护。
与 [Android 上的 Kotlin](https://kotlinlang.org/docs/android-overview.html)类似，IntelliJ 平台大量使用回调，这在 Kotlin 中易于表示为 [lambdas](https://kotlinlang.org/docs/lambdas.html) 表达式。

同样，在 IntelliJ 平台中使用 [扩展](https://kotlinlang.org/docs/extensions.html)很容易自定义内部类的行为。
例如，通常会 [保护日志语句](https://www.slf4j.org/faq.html#logging_performance)以避免参数构造的成本，导致在使用日志时出现以下仪式：

```java
if (logger.isDebugEnabled()) {
  logger.debug("..."+expensiveComputation());
}
```
我们可以在 Kotlin 中更简洁地实现相同的结果，通过声明以下扩展方法：

```kotlin
inline fun Logger.debug(lazyMessage: () -> String) {
  if (isDebugEnabled) {
    debug(lazyMessage())
  }
}
```

现在我们可以直接这样编写：

```kotlin
logger.debug { "..." + expensiveComputation() }
```

以此来获得轻量级日志的所有优点，同时减少代码冗余。

通过实践，您将能够识别出许多在 IntelliJ 平台上可用 Kotlin 简化的习惯用法。

### Kotlin 中的 UI 表单

IntelliJ 平台提供了 [类型安全的 DSL](kotlin_ui_dsl_version_2.md)，允许以声明方式构建 UI 表单。

> 使用 _UI Designer_ 插件来使用 Kotlin [是不支持](https://youtrack.jetbrains.com/issue/KTIJ-791)的。
>

## 添加 Kotlin 支持

> [](plugin_github_template.md)提供了一个预配置的使用 Kotlin 的项目。

IntelliJ IDEA 捆绑了必要的 Kotlin 插件，无需进一步配置。
有关详细说明，请参阅 [Kotlin documentation](https://kotlinlang.org/docs/getting-started.html)。

### Kotlin Gradle 插件

向基于 Gradle 的项目添加 Kotlin 源文件编译支持需要添加并配置 [Kotlin JVM Gradle 插件](https://kotlinlang.org/docs/gradle.html#targeting-the-jvm)。

See the <path>build.gradle.kts</path> from [kotlin_demo](%gh-sdk-samples%/kotlin_demo) sample plugin:
请参阅 [kotlin_demo](%gh-sdk-samples%/kotlin_demo)示例插件的 <path>build.gradle.kts</path>：

```kotlin
```

{src="kotlin_demo/build.gradle.kts" include-lines="2-"}

### Kotlin 标准库

自 Kotlin 1.4 起，将自动添加对标准库 `stdlib` 的依赖项 ([API文档](https://kotlinlang.org/api/latest/jvm/stdlib/))。
在大多数情况下，不需要将其包含在插件分发中，因为平台已经捆绑了它。

要退出，请在 <path>gradle.properties</path> 中添加以下行：

```
kotlin.stdlib.default.dependency = false
```

[](tools_gradle_intellij_plugin.md) 使用 [](tools_gradle_intellij_plugin.md#tasks-verifypluginconfiguration) 检查此 Gradle 属性的存在。
如果该属性不存在，则在插件配置验证期间将报告警告，因为当 Kotlin stdlib 被捆绑在插件存档中时，这是一个常见问题。
如果希望使 Kotlin stdlib 存在于最终存档中，请使用 `kotlin.stdlib.default.dependency = true` 显式指定它。

如果插件支持 [多个平台版本](build_number_ranges.md)，则必须针对最低捆绑 `stdlib` 版本或在插件分发中提供特定版本。

| IntelliJ 平台版本 | 捆绑的 stdlib 版本 |
|---------------|---------------|
| 2023.1        | 1.8.0         |
| 2022.3        | 1.7.0         |
| 2022.2        | 1.6.21        |
| 2022.1        | 1.6.20        |
| 2021.3        | 1.5.10        |
| 2021.2        | 1.5.10        |
| 2021.1        | 1.4.32        |
| 2020.3        | 1.4.0         |
| 2020.2        | 1.3.70        |
| 2020.1        | 1.3.70        |
| 2019.3        | 1.3.31        |
| 2019.2        | 1.3.3         |
| 2019.1        | 1.3.11        |

有关更多详细信息，请参见 [依赖于标准库](https://kotlinlang.org/docs/gradle.html#dependency-on-the-standard-library)。

> 如果需要将 Kotlin 标准库添加到您的 **测试项目** 依赖项中，请参见 [](testing_faq.md#how-to-test-a-jvm-language)部分。
>

### 增量编译

Kotlin Gradle 插件支持 [增量编译](https://kotlinlang.org/docs/gradle-compilation-and-caches.html#incremental-compilation)，允许跟踪源文件中的更改，因此编译器只处理更新的代码。

Kotlin `1.8.20` 发布了一种 [新的增量编译方法](https://kotlinlang.org/docs/gradle-compilation-and-caches.html#a-new-approach-to-incremental-compilation)，默认情况下已启用。
不幸的是，它与 IntelliJ 平台不兼容 - 在读取大型 JAR 文件（如 <path>app.jar</path> 或 <path>3rd-party-rt.jar</path>）时，会导致 `Out of Memory` 异常：

```
Execution failed for task ':compileKotlin'.
> Failed to transform app.jar to match attributes {artifactType=classpath-entry-snapshot, org.gradle.libraryelements=jar, org.gradle.usage=java-runtime}.
   > Execution failed for ClasspathEntrySnapshotTransform: .../lib/app.jar.
      > Java heap space
```

要避免这种异常，请将以下行添加到 <path>gradle.properties</path>：

```
kotlin.incremental.useClasspathSnapshot=false
```

您可以在 [KT-57757](https://youtrack.jetbrains.com/issue/KT-57757/Reduce-classpath-snapshotter-memory-consumption) 中找到问题的当前状态。

### 其他捆绑的 Kotlin 库

请参见 [第三方软件和许可证](https://www.jetbrains.com/legal/third-party-software/)。

## 注意事项

插件 *可能* 使用 [Kotlin 类](https://kotlinlang.org/docs/classes.html) (`class` 关键字) 来实现 [插件配置文件](plugin_configuration_file.md)中的声明。
在注册扩展时，平台使用依赖注入框架在运行时实例化这些类。
因此，插件 *不应* 使用 [Kotlin 对象](https://kotlinlang.org/docs/object-declarations.html#object-declarations-overview) (`object` 关键字) 来实现任何 [plugin.xml](plugin_configuration_file.md) 声明。
管理扩展的生命周期是平台的责任，并且将这些类实例化为 Kotlin 单例可能会导致问题。

## Kotlin 代码 FAQ

[如何缩短引用](https://intellij-support.jetbrains.com/hc/en-us/community/posts/360010025120-Add-new-parameter-into-kotlin-data-class-from-IDEA-plugin?page=1#community_comment_360002950760)

## 用 Kotlin 实现的示例插件

有许多基于 IntelliJ 平台构建的[开源 Kotlin 插件](https://jb.gg/ipe?language=kotlin)。
对于想要获取最新的 Kotlin 插件实现示例的开发人员，可以参考以下项目以获得灵感：

* [Presentation Assistant](https://github.com/chashnikov/IntelliJ-presentation-assistant)
* [Rust](https://github.com/intellij-rust/intellij-rust)
* [TeXiFy IDEA](https://github.com/Hannah-Sten/TeXiFy-IDEA)
* [Deno](%gh-ij-plugins%/Deno)
