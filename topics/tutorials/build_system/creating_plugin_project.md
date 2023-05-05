<!-- Copyright 2000-2023 JetBrains s.r.o. and contributors. Use of this source code is governed by the Apache 2.0 license. -->

# 创建插件 Gradle 项目。

<link-summary>创建并运行基于 Gradle 的 IntelliJ 平台插件项目。</link-summary>

本文档页面描述了使用 [New Project Wizard](https://www.jetbrains.com/help/idea/new-project-wizard.html)生成的基于Gradle的插件项目，但使用 [](plugin_github_template.md)生成的项目涵盖所有已描述的文件和目录。

## Creating a Plugin with New Project Wizard

<procedure title="Create IDE Plugin" id="create-ide-plugin">

通过 <ui-path>File | New | Project...</ui-path> 操作启动 <control>New Project</control>向导，并提供以下信息：
1. 从左侧列表中选择 <control>IDE插件</control> 生成器类型。
2. 指定项目 <control>Name</control> 和 <control>Location</control>。
3. 在项目 <control>Type</control> 中选择 <control>Plugin</control> 选项。
4. _仅适用于早于2023.1版本的IntelliJ IDEA：_

   选择插件将用于实现的 <control>Language</control> 。
   对于本例，选择 <control>Kotlin</control> 选项。
   有关更多信息，请参阅 [面向插件开发人员的Kotlin](using_kotlin.md)。

   > 在IntelliJ IDEA 2023.1或更新版本中生成的项目，默认情况下支持Kotlin和Java源代码。
   > 项目生成器自动创建 <path>$PLUGIN_DIR$/src/main/kotlin</path> 源目录。
   > 要添加Java源代码，请创建 <path>$PLUGIN_DIR$/src/main/java</path> 目录。
   >
   {style="note"}

5. 提供 <control>Group</control> ，通常是倒置的公司域（例如 `com.example.mycompany` ）。
   它用于在Gradle项目的构建脚本中的Gradle属性 `project.group` 值。
6. 提供 <control>Artifact</control> ，这是构建项目构件的默认名称（没有版本）。
   它还用于在Gradle项目 <path>settings.gradle.kts</path> 文件中的属性 `rootProject.name` 值。
   对于此示例，请输入 `my_plugin`。
7. 选择 <control>JDK</control> 11。
   此JDK将是运行Gradle的默认JRE，并且是编译插件源代码时使用的JDK版本。

<include from="snippets.md" element-id="apiChangesJavaVersion"/>

8. 提供所有信息后，单击 <control>Create</control> 按钮生成项目。

</procedure>

### 由向导生成的 Gradle IntelliJ 平台插件的组件

对于上述步骤创建的示例 `my_plugin`，_IDE 插件_ 生成器会创建以下目录内容：

```text
my_plugin
├── .run
│   └── Run IDE with Plugin.run.xml
├── gradle
│   └── wrapper
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── src
│   └── main
│       ├── kotlin
│       └── resources
│           └── META-INF
│               ├── plugin.xml
│               └── pluginIcon.svg
├── .gitignore
├── build.gradle.kts
├── gradle.properties
├── gradlew
├── gradlew.bat
└── settings.gradle.kts
```

* 默认的 IntelliJ 平台 <path>build.gradle.kts</path> 文件（见下一段）。
* <path>gradle.properties</path> 文件，包含 Gradle 构建脚本使用的属性。
* <path>settings.gradle.kts</path> 文件，包含 `rootProject.name` 和所需存储库的定义。
* Gradle Wrapper 文件，特别是 <path>gradle-wrapper.properties</path> 文件，它指定用于构建插件的 Gradle 版本。
  如果需要，IntelliJ IDEA Gradle 插件会下载此文件中指定的 Gradle 版本。
* 默认 `main` [source set](https://docs.gradle.org/current/userguide/java_plugin.html#sec:java_project_layout) 下的 <path>META-INF</path> 目录包含插件 [配置文件](plugin_configuration_file.md)和 [插件图标](plugin_icon_file.md)。
* _Run Plugin_ [运行配置](https://www.jetbrains.com/help/idea/run-debug-configuration.html)。

生成的 `my_plugin` 项目 <path>build.gradle.kts</path> 文件：

```kotlin
plugins {
  id("java")
  id("org.jetbrains.kotlin.jvm") version "1.8.20"
  id("org.jetbrains.intellij") version "1.13.3"
}

group = "com.example"
version = "1.0-SNAPSHOT"

repositories {
  mavenCentral()
}

// Configure Gradle IntelliJ Plugin
// Read more: https://plugins.jetbrains.com/docs/intellij/tools-gradle-intellij-plugin.html
intellij {
  version.set("2022.2.5")
  type.set("IC") // Target IDE Platform

  plugins.set(listOf(/* Plugin Dependencies */))
}

tasks {
  // Set the JVM compatibility versions
  withType<JavaCompile> {
    sourceCompatibility = "17"
    targetCompatibility = "17"
  }
  withType<org.jetbrains.kotlin.gradle.tasks.KotlinCompile> {
    kotlinOptions.jvmTarget = "17"
  }

  patchPluginXml {
    sinceBuild.set("222")
    untilBuild.set("232.*")
  }

  signPlugin {
    certificateChain.set(System.getenv("CERTIFICATE_CHAIN"))
    privateKey.set(System.getenv("PRIVATE_KEY"))
    password.set(System.getenv("PRIVATE_KEY_PASSWORD"))
  }

  publishPlugin {
    token.set(System.getenv("PUBLISH_TOKEN"))
  }
}
```

* 三个 Gradle 插件被显式声明：
  * [Gradle Java](https://docs.gradle.org/current/userguide/java_plugin.html) 插件（`java`）。
  * [Kotlin Gradle](https://kotlinlang.org/docs/gradle-configure-project.html#apply-the-plugin) 插件（`org.jetbrains.kotlin.jvm`）。
  * [](tools_gradle_intellij_plugin.md)（`org.jetbrains.intellij`）。
* [New Project](#create-ide-plugin) 向导中的 <control>Group</control> 是 `project.group` 值。
* `sourceCompatibility` 行被注入以强制使用 Java 11 JDK 编译 Java 源代码。
* [`intellij.version`](tools_gradle_intellij_plugin.md#intellij-extension-version) 和 [`intellij.type`](tools_gradle_intellij_plugin.md#intellij-extension-type) 属性的值指定要用于构建插件的 IntelliJ 平台的版本和类型。
* [插件依赖](tools_gradle_intellij_plugin.md#intellij-extension-plugins) 的空占位符列表。
* [`patchPluginXml.sinceBuild`](tools_gradle_intellij_plugin.md#tasks-patchpluginxml-sincebuild) 和 [`patchPluginXml.untilBuild`](tools_gradle_intellij_plugin.md#tasks-patchpluginxml-untilbuild) 属性的值指定插件兼容的 IDE 构建的最小和最大版本。
* 初始 [`signPlugin`](tools_gradle_intellij_plugin.md#tasks-signplugin) 和 [`publishPlugin`](tools_gradle_intellij_plugin.md#tasks-publishplugin) 任务配置。
  有关更多信息，请参见 [](publishing_plugin.md#publishing-plugin-with-gradle) 部分。

> 考虑使用 [IntelliJ 平台插件模板](https://github.com/JetBrains/intellij-platform-plugin-template)，该模板还提供了由 GitHub Actions 覆盖的 CI 设置。

#### 插件 Gradle 属性和插件配置文件的节点元素

Gradle 属性 `rootProject.name` 和 `project.group` 通常不会与相应的 [插件配置文件](plugin_configuration_file.md) <path>plugin.xml</path> 元素 [`<name>`](plugin_configuration_file.md#idea-plugin__name) 和 [`<id>`](plugin_configuration_file.md#idea-plugin__id) 匹配。
它们没有 IntelliJ 平台相关的原因，因为它们具有不同的功能。

`<name>` 元素（用作插件的显示名称）通常与 `rootProject.name` 相同，但可以更加说明性。

`<id>` 值必须是所有插件中唯一的标识符，通常是指定 <control>Group</control> 和 <control>Artifact</control> 的串联。
请注意，在发布的插件中修改 `<id>` 将导致现有安装的自动更新失效。

## 使用 `runIde` Gradle 任务运行插件

Gradle 项目从 IDE 的 Gradle 工具窗口运行。

### 添加代码到项目中

在运行 [`my_plugin`](#components-of-a-wizard-generated-gradle-intellij-platform-plugin) 之前，可以添加一些代码以提供简单的功能。
有关逐步添加菜单操作 的说明，请参见 [Creating Actions](working_with_custom_actions.md) 教程。

### 执行插件

_IDE 插件_ 生成器自动创建了 _Run Plugin_ 运行配置，可以通过 <ui-path>Run | Run...</ui-path> 操作执行，也可以在 <control>Gradle</control> 工具窗口的 <control>Run Configurations</control> 节点下找到。

要直接执行 Gradle `runIde` 任务，请打开 <control>Gradle</control> 工具窗口并在 <control>Tasks</control> 节点下搜索 <control>runIde</control> 任务。
如果列表中没有它，请单击 Gradle 工具窗口顶部的 [工具栏](https://www.jetbrains.com/help/idea/jetgradle-tool-window.html#gradle_toolbar) 中的重新导入按钮。
当 <control>runIde</control> 任务可见时，双击它以执行。

要在 _独立_ 的 IDE 实例中调试插件，请参见 [How to Debug Your Own IntelliJ IDEA Instance](https://medium.com/agorapulse-stories/how-to-debug-your-own-intellij-idea-instance-7d7df185a48d) 博客文章。

> 有关如何使用基于 Gradle 的项目的更多信息，请参见 [Working with Gradle in IntelliJ IDEA](https://www.youtube.com/watch?v=6V6G3RyxEMk) 屏幕录像和 IntelliJ IDEA 帮助中的处理 [Gradle 任务](https://www.jetbrains.com/help/idea/work-with-gradle-tasks.html)。
>
