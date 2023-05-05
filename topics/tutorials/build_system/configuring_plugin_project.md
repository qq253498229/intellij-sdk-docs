# 配置 Gradle IntelliJ 插件

<!-- Copyright 2000-2023 JetBrains s.r.o. and contributors. Use of this source code is governed by the Apache 2.0 license. -->

<link-summary>配置必要的 Gradle IntelliJ 插件属性和任务。</link-summary>

本节介绍了实现常见功能的 Gradle 插件属性的指导性导览。
有关更高级的选项，请参阅完整的 [Gradle IntelliJ Plugin](tools_gradle_intellij_plugin.md) 参考文档。

## 保持最新版本

Gradle IntelliJ Plugin 和 [Gradle](https://gradle.org/install/) 构建系统一直在不断发展，每个新版本都会带来重要的错误修复、新功能和改进，使开发更加高效。
强烈建议将 Gradle 和 Gradle IntelliJ Plugin 都保持更新到最新版本。

> 当前的 Gradle IntelliJ Plugin 版本是 [![GitHub Release](https://img.shields.io/github/release/jetbrains/gradle-intellij-plugin.svg?style=flat-square){type="joined"}](https://github.com/jetbrains/gradle-intellij-plugin/releases)
>
{style="note"}

## 目标平台和依赖项

默认情况下，Gradle 插件将根据 IntelliJ IDEA Community Edition 的最新 EAP 快照定义的 IntelliJ 平台构建插件项目。

如果本地计算机上没有匹配指定 IntelliJ 平台的版本，则 Gradle 插件会下载正确的版本和类型。
然后，IntelliJ IDEA 对构建及任何关联的源代码和 JetBrains Java 运行时进行索引。

### IntelliJ 平台配置

显式设置 [`intellij.version`](tools_gradle_intellij_plugin.md#intellij-extension-version) 和 [`intellij.type`](tools_gradle_intellij_plugin.md#intellij-extension-type) 属性告诉 Gradle 插件使用该 IntelliJ 平台配置来创建插件项目。

> 有关如何开发与多个基于 IntelliJ 的 IDE 兼容的插件的信息，请参见 [为多个产品开发](dev_alternate_products.md) 页面。
>

所有可用的平台版本都可以在 [](intellij_artifacts.md) 中浏览。

如果存储库中没有选定的平台版本，或者希望使用目标 IDE 的本地安装作为所需的 IntelliJ 平台类型和版本，请使用 [`intellij.localPath`](tools_gradle_intellij_plugin.md#intellij-extension-localpath) 指向该安装位置。
如果设置了 `intellij.localPath` 属性，请不要设置 `intellij.version` 和 `intellij.type` 属性，因为这可能会导致未定义的行为。

### 插件依赖项

IntelliJ 平台插件项目可以依赖内置或第三方插件。
在这种情况下，项目应该以与用于构建插件项目的 IntelliJ 平台版本匹配的那些插件的版本进行构建。
Gradle 插件将获取 [`intellij.plugins`](tools_gradle_intellij_plugin.md#intellij-extension-plugins) 定义的列表中的任何插件。
有关指定插件和版本的信息，请参见 Gradle 插件 [IntelliJ Extension](tools_gradle_intellij_plugin.md#configuration-intellij-extension)。

请注意，此属性描述了依赖关系，以便 Gradle 插件可以获取所需的工件。
必须在 [Plugin Configuration](plugin_configuration_file.md) (<path>plugin.xml</path>) 文件中添加运行时依赖项，如 [Plugin Dependencies](plugin_dependencies.md#3-dependency-declaration-in-pluginxml) 中所述。

## 运行 IDE 任务

默认情况下，Gradle 插件将使用与构建插件时相同的 IntelliJ 平台版本作为 IDE 开发实例。
对于此用例，默认情况下也使用相应的 JetBrains 运行时，因此不需要进行进一步配置。

### 运行对不同版本和类型的基于 IntelliJ 平台的 IDE

用于 [开发实例](ide_development_instance.md)的 IntelliJ 平台 IDE 可以与用于构建插件项目的 IDE 不同。
设置 [`runIde.ideDir`](tools_gradle_intellij_plugin.md#tasks-runide-idedir) 属性将定义要用于开发实例的 IDE。
在使用 [替代的基于 IntelliJ 平台的 IDE](intellij_platform.md#ides-based-on-the-intellij-platform)运行 或调试插件时，通常会使用此属性。

### 针对不同版本的 JetBrains 运行时运行

每个 IntelliJ 平台版本都有对应的 [JetBrains 运行时](ide_development_instance.md#using-a-jetbrains-runtime-for-the-development-instance)版本。
通过指定 [`runIde.jbrVersion`](tools_gradle_intellij_plugin.md#tasks-runide-jbrversion) 属性，可以使用不同版本的运行时，描述应由 IDE 开发实例使用的 JetBrains 运行时版本。
Gradle 插件将根据需要获取指定的 JetBrains 运行时。

## 修补插件配置文件

插件项目的 <path>plugin.xml</path> 文件具有在构建时从 [`patchPluginXml`](tools_gradle_intellij_plugin.md#tasks-patchpluginxml) 任务的属性值 “修补” 的元素值。
Patching DSL 中尽可能多的属性将被替换为插件项目的 <path>plugin.xml</path> 文件中相应元素的值：
* 如果定义了 `patchPluginXml` 属性的默认值，则 _不管 `patchPluginXml` 任务是否出现在 Gradle 构建脚本中_，该属性值都将在 <path>plugin.xml</path> 中进行修补。
  * 例如，[`patchPluginXml.sinceBuild`](tools_gradle_intellij_plugin.md#tasks-patchpluginxml-sincebuild) 和 [`patchPluginXml.untilBuild`](tools_gradle_intellij_plugin.md#tasks-patchpluginxml-untilbuild) 属性的默认值是基于 [`intellij.version`](tools_gradle_intellij_plugin.md#intellij-extension-version) 的声明（或默认）值定义的。
    因此，默认情况下，`patchPluginXml.sinceBuild` 和 `patchPluginXml.untilBuild` 将被替换为 <path>plugin.xml</path> 文件中 [`<idea-version>`](plugin_configuration_file.md#idea-plugin__idea-version) 元素的 `since-build` 和 `until-build` 属性。
* 如果显式地定义了 [`patchPluginXml`](tools_gradle_intellij_plugin.md#tasks-patchpluginxml) 任务的属性值，则该属性值将在 <path>plugin.xml</path> 中进行替换。
  * 如果 `patchPluginXml.sinceBuild` 和 `patchPluginXml.untilBuild` 属性都显式设置，则两者都将在 <path>plugin.xml</path> 中进行替换。
  * 如果一个属性被显式设置（例如 `patchPluginXml.sinceBuild`），而另一个属性未被设置（例如 `patchPluginXml.untilBuild` 具有默认值），则这两个属性都将按其各自的（显式和默认）值进行修补。
* 要想 **不替换** `<idea-version>` 元素的 `since-build` 和 `until-build` 属性，请将 [`intellij.updateSinceUntilBuild`](tools_gradle_intellij_plugin.md#intellij-extension-updatesinceuntilbuild) 设置为 `false`，并且不提供 `patchPluginXml.sinceBuild` 和 `patchPluginXml.untilBuild` 值。

避免混淆的最佳实践是用注释替换 Gradle 插件将修补的 <path>plugin.xml</path> 中的元素。
这样，这些参数的值就不会在源代码中出现两次。
Gradle 插件将在修补过程的一部分添加必要的元素。
对于包含诸如 [`patchPluginXml.changeNotes`](tools_gradle_intellij_plugin.md#tasks-patchpluginxml-changenotes) 和 [`patchPluginXml.pluginDescription`](tools_gradle_intellij_plugin.md#tasks-patchpluginxml-plugindescription) 等描述的 [`patchPluginXml`](tools_gradle_intellij_plugin.md#tasks-patchpluginxml) 属性，使用 HTML 元素时不需要 `CDATA` 块。

> 为了维护和生成最新的更改日志，请尝试使用 [Gradle Changelog Plugin](https://github.com/JetBrains/gradle-changelog-plugin)。
>

正如在 [](creating_plugin_project.md#components-of-a-wizard-generated-gradle-intellij-platform-plugin)中讨论的那样，Gradle 属性 `project.version`、`project.group` 和 `rootProject.name` 都是根据向导输入生成的。
但是，[](tools_gradle_intellij_plugin.md) 不会合并和替换这些 Gradle 属性以获取 <path>plugin.xml</path> 文件中默认的 [`<id>`](plugin_configuration_file.md#idea-plugin__id) 和 [`<name>`](plugin_configuration_file.md#idea-plugin__name) 元素。

最佳实践是保持 `project.version` 的当前状态。
默认情况下，如果您在 Gradle 构建脚本中修改了 `project.version`，则 Gradle 插件将自动更新 <path>plugin.xml</path> 文件中的 [`<version>`](plugin_configuration_file.md#idea-plugin__version) 值。
这种做法使所有版本声明保持同步。

## 验证插件

Gradle 插件提供了一些任务，允许运行完整性和兼容性测试：
* [`verifyPluginConfiguration`](tools_gradle_intellij_plugin.md#tasks-verifypluginconfiguration) - 验证在插件项目中配置的 SDK、目标平台、API 等版本是否正确，
* [`verifyPlugin`](tools_gradle_intellij_plugin.md#tasks-verifyplugin) - 验证 <path>plugin.xml</path> 描述符的完整性和内容以及插件存档结构的内容，
* [`runPluginVerifier`](tools_gradle_intellij_plugin.md#tasks-runpluginverifier) - 运行 [IntelliJ Plugin Verifier](https://github.com/JetBrains/intellij-plugin-verifier) 工具，检查与指定 IntelliJ IDE 构建的二进制兼容性。

插件验证器集成任务允许配置您的插件将被检查的确切 IDE 版本。
有关更多信息，请参见 [](verifying_plugin_compatibility.md#plugin-verifier)。

## 发布插件

在使用 [`publishPlugin`](tools_gradle_intellij_plugin.md#tasks-publishplugin) 任务之前，请查看 [](publishing_plugin.md) 页面。
该文档解释了不同的 Gradle 插件上传方法，而不会暴露帐户凭据。
