<!-- Copyright 2000-2022 JetBrains s.r.o. and contributors. Use of this source code is governed by the Apache 2.0 license. -->

# IDE 开发实例

<link-summary>在插件开发期间运行和调试插件所使用的 IDE 实例的概述。</link-summary>

JetBrains提供的一个功能是在基于IntelliJ平台的IDE（如IntelliJ IDEA）内部运行或调试插件项目。
对于基于Gradle的项目，选择 [`runIde`](creating_plugin_project.md#running-a-plugin-with-the-runide-gradle-task)任务（对于DevKit-based项目，则是 [Run](running_and_debugging_a_theme.md)菜单），将启动带有插件的 _开发实例_。
本页面描述了如何控制开发实例的一些设置。

> 另请参阅 [`runIde` 任务](tools_gradle_intellij_plugin.md#tasks-runide)属性和 [高级配置](https://www.jetbrains.com/help/idea/tuning-the-ide.html)，以获取通用VM选项和属性。
>

## 使用JetBrains Runtime开发实例

一个常见的用例是对一个插件项目进行开发（构建），例如使用Java 8的JDK，并在IDE的开发实例中运行或调试该插件。
在这种情况下，开发实例必须使用JetBrains Runtime（JBR），而不是用于构建插件项目的JDK。

JetBrains Runtime是在Windows、macOS和Linux上运行基于IntelliJ平台的IDE的环境。
它由JetBrains进行了一些修改，例如修复了官方JDK构建中不存在的本地崩溃。
JetBrains Runtime的版本与所有基于IntelliJ平台的IDE捆绑在一起。
为了在开发实例中运行或调试插件项目时产生准确的结果，请按照以下步骤确保开发实例使用JetBrains Runtime。

### 确定JetBrains Runtime版本

JetBrains Runtime由用于构建插件项目的JDK版本确定，无论它是在macOS、Windows还是Linux上构建的。

<procedure title="确定一个示例JetBrains Runtime版本">

如果一个插件是针对Java 8 SE Development Kit 8 for macOS（<path>jdk-8u212-macosx-x64.dmg</path>）进行开发，则需要获得兼容的JetBrains Runtime：

1. 前往 [GitHub JetBrains Runtime发布页面](https://github.com/JetBrains/JetBrainsRuntime)获取一般信息和最新构建。
2. 打开 [Releases](https://github.com/JetBrains/JetBrainsRuntime/releases)页面以访问所有发布版本。
3. 选择与平台和SDK版本相应的软件包名称。
   在这种情况下，软件包名称是 `jbrsdk8-osx-x64`，用于 **J**et**B**rains **R**untime _SDK_ 版本8，macOS x64硬件。
4. 在文件列表中，查找名称满足以下条件：
   * 版本和构建号与用于构建插件项目的JDK匹配。
     例如，`jbrx-8u252-osx-x64` 与 Java 8 JDK, build 252：`jdk-8u252-macosx-x64` 相匹配。
   * 选取可用的最高JetBrains Runtime构建号。
     例如，文件为 <path>jbrx-8u252-osx-x64-b1649.2.tar.gz</path>，表示匹配Java 8 JDK build 252的此JetBrains Runtime的构建 build 1649.2。
</procedure>

### JetBrains Runtime 变体

JetBrains Runtime 以各种变体提供，用于不同的目的，如调试、开发运行或与 IDE 捆绑。

可用的 JBR 变体包括：
- `jcef` - 附带 [JCEF](jcef.md) 浏览器引擎的发布版
- `sdk` - 用于开发目的的 JBR SDK 包
- `fd` - fastdebug 包，还包括 `jcef` 模块
- `dcevm` - 捆绑 DCEVM（动态代码演进虚拟机）
- `nomod` – 不附带任何其他模块的发布版

> 对于 `JBR 17`，默认情况下会捆绑 `dcevm`。
> 因此，不再提供单独的 `dcevm` 和 `nomod` 变体。
>
{style="note"}

<tabs group="project-type">

<tab title="Gradle" group-key="gradle">

默认情况下，Gradle 插件将获取并使用与构建插件项目所使用的 IntelliJ 平台版本对应的开发实例的 JetBrains Runtime 版本。
如果需要，可以使用 [`runIde.jbrVersion`](tools_gradle_intellij_plugin.md#tasks-runide-jbrversion) 任务属性指定另一版本。

</tab>

<tab title="DevKit" group-key="devkit">

基于 DevKit 的插件项目的 [运行配置](https://www.jetbrains.com/help/idea/run-debug-configuration.html)控制在开发实例中运行和调试插件项目的 JDK。
默认的运行配置将相同的 JDK 用于构建插件项目和在开发实例中运行插件。

要更改开发实例的运行时，将运行配置编辑对话框中的 _JRE_ 字段设置为下载 JetBrains Runtime。

</tab>
</tabs>

## 启用自动重载

从 2020.1 开始，这适用于兼容的[动态插件](dynamic_plugins.md)。
它可以在检测到代码更改（当 JAR 文件被修改）后避免对开发实例进行完全重新启动，从而实现更快的开发周期。

请注意，任何在生产环境中出现卸载问题都会要求用户重新启动 IDE。

> 当沙盒 IDE 实例在调试器下运行时，自动重新加载功能不起作用。
>
{style="warning"}

<tabs group="project-type">

<tab title="Gradle" group-key="gradle">

默认情况下，针对目标平台 2020.2 或更高版本启用。

设置属性 [`runIde.autoReloadPlugins`](tools_gradle_intellij_plugin.md#tasks-runide-autoreloadplugins) 为 `true`，以在较早的平台版本中启用它，或设置为 `false` 明确禁用它，请参阅 [](tools_gradle_intellij_plugin_faq.md#how-to-disable-automatic-reload-of-dynamic-plugins)。

在修改插件项目后启动沙盒 IDE 实例后，在聚焦回沙盒实例时运行 [`buildPlugin`](tools_gradle_intellij_plugin.md#tasks-buildplugin) 任务以触发重新加载。

> 为了解决 _一次只能运行一个 IDEA 实例_ 问题，必须[明确禁用](tools_gradle_intellij_plugin_faq.md#how-to-disable-building-searchable-options) [`buildSearchableOptions`](tools_gradle_intellij_plugin.md#tasks-buildsearchableoptions) 任务。
>
{style="warning"}

</tab>

<tab title="DevKit" group-key="devkit">

在插件 DevKit 的 [运行配置](running_and_debugging_a_theme.md)中添加系统属性 `idea.auto.reload.plugins`。

要禁用自动重新加载，请明确设置 `idea.auto.reload.plugins` 为 `false`（2020.1.2+）。

</tab>

</tabs>

## 开发实例沙盒目录

_Sandbox Home_ 目录包含 IDE 开发实例的 [设置、缓存、日志和插件](#development-instance-settings-caches-logs-and-plugins)。
这些信息存储在不同于 [安装 IDE 本身](https://intellij-support.jetbrains.com/hc/en-us/articles/206544519-Directories-used-by-the-IDE-to-store-settings-caches-plugins-and-logs)的位置。

<tabs group="project-type">
<tab title="Gradle" group-key="gradle">

在插件 Gradle 项目中，沙盒主目录的默认位置为：
* Windows：<path>$PROJECT_DIRECTORY$\\build\\idea-sandbox</path>
* Linux/macOS：<path>$PROJECT_DIRECTORY$/build/idea-sandbox</path>

可以使用 [`intellij.sandboxDir`](tools_gradle_intellij_plugin.md#intellij-extension-sandboxdir) 属性配置沙盒主目录位置。

</tab>

<tab title="DevKit" group-key="devkit">

对于基于 DevKit 的插件，默认的 <control>Sandbox Home</control> 目录位置定义在 IntelliJ 平台插件 SDK 中。
有关如何在 IntelliJ 平台 SDK 中设置沙盒主目录的信息，请参阅 [设置主题开发环境](setting_up_theme_environment.md#add-intellij-platform-plugin-sdk)。

默认的沙盒主目录位置是：
* Windows：<path>$USER_HOME$\\.$PRODUCT_SYSTEM_NAME$$PRODUCT_VERSION$\\system\\plugins-sandbox\\</path>
* Linux：<path>~/.$PRODUCT_SYSTEM_NAME$$PRODUCT_VERSION$/system/plugins-sandbox/</path>
* macOS：<path>~/Library/Caches/$PRODUCT_SYSTEM_NAME$$PRODUCT_VERSION$/plugins-sandbox/</path>

</tab>
</tabs>

### 开发实例设置、缓存、日志和插件

在沙盒主目录中，有开发实例的子目录：
* <path>config</path> 包含 IDE 实例的设置。
* <path>plugins</path> 包含在 IDE 实例中运行的每个插件的文件夹。
* <path>system/caches</path> 或 <path>system\caches</path> 保存 IDE 实例数据。
* <path>system/log</path> 或 <path>system\log</path> 包含 IDE 实例的 <path>idea.log</path> 文件。

可以手动清除这些沙盒主目录子目录以重置 IDE 开发实例。
在下次启动开发实例时，子目录将重新填充适当的信息。
