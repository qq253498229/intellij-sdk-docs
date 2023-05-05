# 所需经验

<!-- Copyright 2000-2022 JetBrains s.r.o. and contributors. Use of this source code is governed by the Apache 2.0 license. -->

<link-summary>所需技术知识。</link-summary>

IntelliJ平台是一种基于JVM的应用程序，主要使用Java和 [Kotlin](https://kotlinlang.org) 实现。
目前来看，无法使用非JVM语言开发IntelliJ平台的插件。

开发IntelliJ平台的插件需要掌握以下技术和概念：

- Java、Kotlin或其他JVM语言以及其标准和第三方库

- [Gradle](https://gradle.org/) 或类似的构建系统（例如Maven）

- [Swing](https://en.wikipedia.org/wiki/Swing_(Java))用于构建用户界面

- [Java并发模型](https://docs.oracle.com/javase/tutorial/essential/concurrency/index.html)

- 具有IntelliJ平台为基础的IDE（例如 [IntelliJ IDEA](https://www.jetbrains.com/idea/)）的经验

请记住，IntelliJ平台是一个庞大的项目，尽管我们正在尽最大努力涵盖尽可能多的主题，但不可能在文档中包含每个功能和用例。
有时，开发插件需要深入研究 [IntelliJ平台代码](https://github.com/JetBrains/intellij-community)，并分析 [其他插件中的示例实现](https://jb.gg/ipe)。

强烈建议在开始插件实现之前熟悉 [](explore_api.md) 部分。

> 在某些情况下，可能不需要实现一个真正的IntelliJ平台插件，因为存在 [替代解决方案](plugin_alternatives.md)。
>
{style="note"}
