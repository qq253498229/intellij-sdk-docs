<!-- Copyright 2000-2023 JetBrains s.r.o. and contributors. Use of this source code is governed by the Apache 2.0 license. -->

# 插件类型

<link-summary>不同类型插件的概述和示例。</link-summary>

基于IntelliJ平台的产品可以通过添加插件进行修改和定制。
所有可下载插件都可从 [JetBrains Marketplace](https://plugins.jetbrains.com/)获取。

最常见的插件类型包括：

* 自定义语言支持
* 框架集成
* 工具集成
* 用户界面附加组件
* 主题

> 在某些情况下，可能不需要实现一个真正的IntelliJ平台插件，因为存在 [替代解决方案](plugin_alternatives.md)。
>

## 自定义语言支持

自定义语言支持提供了与特定编程语言一起工作的基本功能，包括：

* 文件类型识别
* 词法分析
* 语法高亮
* 格式化
* 代码洞察和代码完成
* 检查和快速修复
* 意图操作

插件还可以增强现有的（捆绑的）自定义语言，例如提供额外的检查、意图或任何其他功能。

请参考 [自定义语言支持教程](custom_language_support_tutorial.md) 以了解更多信息。

## 框架集成

框架集成包括改进的代码洞察功能，这些功能通常针对给定的框架，并且可以直接从IDE中使用特定于框架的功能。
有时它还包括用于自定义语法或DSL的语言支持元素。

* 具体的代码洞察
* 直接访问特定于框架的功能

请参考 [Struts 2插件](%gh-ij-plugins%/struts2) 作为框架集成的示例。
更多参考插件可以在 [JetBrains Marketplace](https://plugins.jetbrains.com/search?orderBy=update%20date&shouldHaveSource=true&tags=Framework) 上找到。

## 工具集成

工具集成使得可以在不切换上下文的情况下直接从IDE中操作第三方工具和组件，这包括：

* 实现其他操作
* 相关的UI组件
* 访问外部资源

请参考 [Gerrit集成](https://plugins.jetbrains.com/plugin/7272)插件作为一个示例。

## 用户界面附加组件

此类插件对IDE的标准用户界面应用各种更改。
一些新增的组件是交互式的，并提供新功能，而其他组件仅限于视觉修改。
[随机背景](https://plugins.jetbrains.com/plugin/9692-random-background) 插件可以作为一个示例。

## 主题

[Themes](themes_getting_started.md) 使设计师能够自定义内置IDE UI元素的外观。
