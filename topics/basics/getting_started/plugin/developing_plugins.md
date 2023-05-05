<!-- Copyright 2000-2023 JetBrains s.r.o. and contributors. Use of this source code is governed by the Apache 2.0 license. -->

# 开发一款插件

<link-summary>使用 Gradle 和 Gradle IntelliJ 插件开发 IntelliJ 平台插件。</link-summary>

可以使用 [IntelliJ IDEA Community Edition](https://www.jetbrains.com/idea/download/)或 [IntelliJ IDEA Ultimate](https://www.jetbrains.com/idea/download/)作为开发插件的IDE。
两者都包括完整的插件开发工具集。
强烈建议始终使用最新版本，因为捆绑的 <control>Plugin DevKit</control> 支持新功能的插件开发工具。

要更加熟悉IntelliJ IDEA，请参考 [IntelliJ IDEA Web帮助](https://www.jetbrains.com/idea/help/)。

在实际开发之前，请确保了解实现最佳 [](plugin_user_experience.md)所需的所有要求。

> 在某些情况下，可能不需要实现一个真正的IntelliJ平台插件，因为存在 [替代解决方案](plugin_alternatives.md)。
>

## Gradle IntelliJ插件

构建IntelliJ平台插件的推荐解决方案是 [](tools_gradle_intellij_plugin.md)。
IntelliJ IDEA Ultimate和Community版本捆绑了必要的插件来支持基于Gradle的插件开发： _Gradle_ 和 _Plugin DevKit_。
要验证这些插件已安装并启用，请参见有关 [管理插件](https://www.jetbrains.com/help/idea/managing-plugins.html)的帮助部分。

Gradle IntelliJ插件管理插件项目的依赖关系-包括基本IDE和其他 [插件依赖项](plugin_dependencies.md)。
它提供任务来运行带有您的插件的IDE，并将您的插件打包和 [发布](publishing_plugin.md#publishing-plugin-with-gradle) 到 [JetBrains Marketplace](https://plugins.jetbrains.com)。
为了确保插件不受平台主要版本发布之间可能发生的 [API更改](api_changes_list.md)的影响，您可以快速针对其他IDE和发布验证您的插件。

创建基于Gradle的新IntelliJ平台插件项目有两种主要方式：
- 在 [New Project Wizard](https://www.jetbrains.com/help/idea/new-project-wizard.html)中提供的专用生成器-创建一个带有所有必需文件的最小插件项目
- 在GitHub上提供的 [](plugin_github_template.md)-除了所需的项目文件外，还包括GitHub Actions CI工作流程的配置。

本文档部分描述使用 <control>New Project</control> 向导生成的插件结构，但使用 _IntelliJ Platform Plugin模板_ 生成的项目涵盖了所有已描述的文件和目录。
有关此方法的优点和如何使用它的说明，请参见 [](plugin_github_template.md) 部分。

> 旧的DevKit项目模型和工作流程仍然支持现有项目，并建议用于 [创建主题插件](developing_themes.md)。
> 请参阅 [如何将DevKit插件迁移到Gradle](migrating_plugin_devkit_to_gradle.md)。
>

> 针对使用Scala实现的插件可用专用的 [SBT插件](https://github.com/JetBrains/sbt-idea-plugin)。
>

## 插件开发工作流程

* [创建基于Gradle的插件项目](creating_plugin_project.md)
* [配置Gradle IntelliJ插件](configuring_plugin_project.md)
  * [添加Kotlin支持](using_kotlin.md) (可选)
* [发布插件](publishing_plugin.md)
