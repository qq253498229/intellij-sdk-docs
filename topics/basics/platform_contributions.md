# 参与 IntelliJ 平台贡献

<!-- Copyright 2000-2023 JetBrains s.r.o. and other contributors. Use of this source code is governed by the Apache 2.0 license that can be found in the LICENSE file. -->

<tldr>如何参与和贡献 IntelliJ 平台开发。</tldr>

请务必阅读[行为准则](intellij-sdk-docs-original_CODE_OF_CONDUCT.md)。

## 参与社区

### 参与论坛和新闻组

有几个社区[论坛和新闻组](https://intellij-support.jetbrains.com/hc/en-us/community/topics)可以加入，讨论 IntelliJ 平台和 IDE。
这些论坛是用户和贡献者感兴趣的技术讨论、回答问题或解决新手潜在问题的绝佳资源。

### 链接到 IntelliJ 平台主页

任何开源项目的成功都取决于使用产品和为项目做出贡献的人数。
通过链接至 [https://www.jetbrains.com/opensource/idea](https://www.jetbrains.com/opensource/idea) ，您可以增加新用户或贡献者了解项目并加入社区的机会。

如果您像我们一样对 IntelliJ IDEA Community Edition 感到兴奋，可以通过链接来展示。
项目徽标和其他资产 [也可用](https://www.jetbrains.com/company/brand/logos/)。

### 推广 IntelliJ 平台和 IntelliJ IDEA Community Edition

使用你的博客、推特、Facebook 或提交文章到你最喜爱的本地杂志来帮助推广这个平台和 IDE。
如果你是不同开源社区的成员，为什么不在他们的讨论论坛或会议上提及 IntelliJ IDEA 呢？
如果您喜欢 IntelliJ IDEA，请大声说出来！
使用 IntelliJ IDEA 的开发者越多，就会发现越多的错误，编写越多的插件，项目变得更加可见，社区获得的好处也会越多！

## 帮助他人学习

### 撰写文档

我们一直在寻找有关 IntelliJ IDEA 功能以及 IntelliJ 平台文档的新文章。
您可以编写教程、操作指南、示例应用程序或分享您对 IntelliJ 平台的经验。
您可以将您的文档发布在网站或博客上，或者向 SDK 文档提交 [pull request](intellij-sdk-docs-original_CONTRIBUTING.md)。

### 制作演示视频

最近，演示视频已成为向其他开发者展示如何有效使用工具的一种非常流行的方式。
您可以记录一个有关某个特定功能或用例的演示视频，并与社区分享。

## 贡献代码

### 提交 Bug 报告

提交 Bug 报告的时间很短，对开发人员非常有帮助。
这是您可以做出的最简单的贡献之一。
当您发现 IDE 或平台存在问题时，请报告它。
确保提供关于您的环境（可从 <ui-path>About</ui-path> 菜单中获得）、重现问题的步骤以及问题的书面描述等信息。
您可以在我们的 [YouTrack 问题跟踪器](https://youtrack.jetbrains.com/issues/IDEA)中提交 Bug。
在提交问题之前，请搜索已经提交的描述相同问题的问题，并如果找到一个，可以投票支持它。

### 协助整理现有的 Bug 报告

多年来，用户已向 IntelliJ IDEA 问题跟踪器提交了数千个问题。
许多未解决的问题不再适用于最新版本的 IntelliJ IDEA，是重复的，或需要更多信息才能解决。
留下评论通知有关这些问题的状态，有助于团队使问题跟踪器干净且对每个人有用。

### 编写插件

为 IntelliJ IDEA 或任何其他基于 IntelliJ 平台的 IDE 添加额外功能的更大代码块，其中最好的方法之一就是编写插件。
您可以将插件提交到 [JetBrains Marketplace](https://plugins.jetbrains.com/)，使其对所有用户都可用。
编写插件时，您可以控制代码，并且不需要签署贡献协议。

### 提交补丁

如果您想改进 IntelliJ 平台的代码或 IntelliJ IDEA 的核心功能，则可以向 [GitHub 上的 IntelliJ IDEA Community Edition 存储库](https://github.com/JetBrains/intellij-community) 提交 pull request。
在准备更改时，请确保遵循 [](intellij_coding_guidelines.md)。
开发人员将审核您的贡献，并且如果符合质量标准并与其余代码匹配，则会通知您接受补丁。

正在寻找问题来解决吗？带有 [#patch_welcome](https://youtrack.jetbrains.com/issues/IDEA?q=%23patch_welcome%20%23unresolved) 标签的问题正在寻找外部贡献者。

或者，您可以将补丁附加到 [YouTrack Bug 数据库](https://youtrack.jetbrains.com/issues/IDEA)中的工单。
您可以使用附加的补丁提交新问题或将补丁附加到由其他用户提交的问题上。
在这种情况下，您还需要签署 [JetBrains Contributor 许可协议（CLA）](https://www.jetbrains.com/agreements/cla/)以完成您的贡献。

### 成为 Committer

长期提交高质量补丁的开发人员可以获得直接提交权限。
