<!-- Copyright 2000-2023 JetBrains s.r.o. and contributors. Use of this source code is governed by the Apache 2.0 license. -->

# IntelliJ 平台插件模板

<link-summary>IntelliJ 平台插件模板是一个 GitHub 模板，包含一个最小预配置的插件项目和 GitHub Actions CI 工作流程，涵盖了构建、测试和部署插件。</link-summary>

IntelliJ 平台插件模板是使用 [New Project Wizard](creating_plugin_project.md) 创建新的基于 Gradle 的 IntelliJ 平台插件的替代方案。

[IntelliJ 平台插件模板][gh:plugin-template]是一个 GitHub 仓库，提供了一个纯样板模板，使创建新的基于 Gradle 的插件项目更加容易。

该模板的主要目标是通过预配置项目脚手架和 CI、链接到适当的文档页面以及保持所有内容组织良好，加快插件开发的设置阶段，从而为新手和有经验的开发人员都节省时间。

GitHub 模板允许您从脚手架创建一个新的仓库，而无需手动复制和粘贴内容、克隆仓库或清除历史记录。
您只需要在 GitHub 项目页面上单击 <control>Use this template</control> 按钮（必须使用您的 GitHub 帐户登录）。
之后，GitHub Actions 工作流将被触发，以覆盖或删除任何特定于模板的配置，例如插件名称、当前的变更日志等。

完成此操作后，该项目即可被克隆到本地环境并在 [IntelliJ IDEA](https://www.jetbrains.com/idea/download) 中打开。

有关详细信息，请参阅 [IntelliJ 平台插件模板][gh:plugin-template]项目文档。

> _Busy Plugin Developer. Episode 0_ 网络研讨会的录像详细描述和展示了 [如何使用 IntelliJ 平台插件模板](https://youtu.be/-6D5-xEaYig?t=230)。
>
{style="note"}

[gh:plugin-template]: https://github.com/JetBrains/intellij-platform-plugin-template
