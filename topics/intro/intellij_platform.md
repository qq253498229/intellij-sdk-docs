<!-- Copyright 2000-2023 JetBrains s.r.o. and contributors. Use of this source code is governed by the Apache 2.0 license. -->

# IntelliJ 平台

<link-summary>介绍 IntelliJ 平台、基于它的插件和 IDE。</link-summary>

IntelliJ 平台本身不是产品，而是为构建 IDE 提供平台。
它用于为 JetBrains 产品提供支持，例如 [IntelliJ IDEA](https://www.jetbrains.com/idea/)。
它也是开源的，第三方可以使用它来构建 IDE，例如 Google 的 [Android Studio](https://developer.android.com/studio/index.html)。

IntelliJ 平台提供了这些 IDE 需要的所有基础设施，以提供丰富的语言工具支持。
它是一个基于组件的、跨平台的 JVM 应用程序宿主，具有高级用户界面工具包，可用于创建工具窗口、树形视图和列表（支持快速搜索），以及弹出菜单和对话框。

IntelliJ 平台具有全文本编辑器，其中包含 [语法高亮](analyzing.md)、代码折叠、代码补全和其他丰富的文本 [编辑功能](editing.md)的抽象实现。
同时还包括图像编辑器。

此外，它还包括开放的 API 以构建标准 IDE 功能，如项目模型和构建系统。
它还提供了一个丰富的调试体验基础设施，具有与语言无关的高级断点支持、调用堆栈、观察窗口和表达式评估等功能。

但 IntelliJ 平台真正的强大之处在于程序结构接口（PSI）。
它是一组用于解析文件、构建代码的丰富语法和语义模型以及从这些数据构建索引的功能。
PSI 支持许多功能，从快速导航到文件、类型和符号，到代码完成窗口和查找用途的内容、代码检查和代码重写，用于快速修复或重构，以及许多其他功能。

IntelliJ 平台包括许多语言的解析器和 PSI 模型，其可扩展性意味着可以添加对其他语言的支持。

## 插件

基于 IntelliJ 平台构建的产品是可扩展的应用程序，平台负责创建组件并将依赖项注入类中。
IntelliJ 平台完全支持插件，并且 JetBrains 托管 [JetBrains Marketplace](https://plugins.jetbrains.com)，可用于分发支持一个或多个产品的插件。
还可以使用 [](custom_plugin_repository.md)来分发插件。

插件可以以多种方式扩展平台，从添加简单的菜单项到添加对完整语言、构建系统和调试器的支持。
许多现有的 IntelliJ 平台功能都是实现为插件，可以根据最终产品的需要包含或排除这些插件。
有关更多详细信息，请参见 [](plugins_quick_start.md)。

> 在某些情况下，可能不需要实现实际的 IntelliJ 平台插件，因为存在 [替代解决方案](plugin_alternatives.md)。
>

## 开源

IntelliJ 平台是开源的，采用 [Apache 许可证](%gh-ic%/LICENSE.txt)，并 [托管在 GitHub 上](https://github.com/JetBrains/intellij-community)。

虽然本指南将 IntelliJ 平台视为一个单独的实体，但并不存在 "IntelliJ Platform" 的 GitHub 存储库。
相反，该平台被认为与 IntelliJ IDEA Community Edition 几乎完全重叠，后者是 IntelliJ IDEA Ultimate 的免费开源版本（上面链接的 GitHub 存储库是 [JetBrains/intellij-community](https://github.com/JetBrains/intellij-community) 存储库）。
请注意：从 2021.1 版开始，一些捆绑在 IntelliJ IDEA Community Edition 中的插件不是开源的。

IntelliJ 平台的版本由相应的 IntelliJ IDEA Community Edition 发布版本的版本确定。
例如，要构建针对 IntelliJ IDEA (2019.1.1) 的插件，使用 #191.6707.61 构建号标记意味着从 `intellij-community` 存储库获取正确的 Intellij 平台文件。
有关构建号与版本编号对应关系的更多信息，请参见 [](build_number_ranges.md)。

通常，基于 IntelliJ 平台的 IDE 将 `intellij-community` 存储库作为 Git 子模块包含，并提供配置来描述由 `intellij-community` 和哪些自定义插件组成产品。
这就是 IDEA Ultimate 团队的工作方式，他们为自定义插件和 IntelliJ 平台本身贡献代码。

## 基于 IntelliJ 平台的 IDE

许多 JetBrains IDE 都基于 IntelliJ 平台。
IntelliJ IDEA Ultimate 是 IntelliJ IDEA Community Edition 的超集，但包括闭源插件（[请参见此功能对比](https://www.jetbrains.com/idea/features/editions_comparison_matrix.html)）。
同样，其他产品如 WebStorm 和 DataGrip 基于 IntelliJ IDEA Community Edition，但包含不同的插件集合，排除其他默认插件。
这使得插件可以针对多个产品进行定位，因为每个产品都将包括来自 IntelliJ IDEA Community Edition 存储库的基本功能和一组插件。

<include from="snippets.md" element-id="jetbrainsProductOpenSourceLicense"/>

以下 IDE 基于 IntelliJ 平台：
* JetBrains IDEs:
  * [AppCode](https://www.jetbrains.com/objc/)
  * [CLion](https://www.jetbrains.com/clion/)
  * [DataGrip](https://www.jetbrains.com/datagrip/)
  * [GoLand](https://www.jetbrains.com/go/)
  * [IntelliJ IDEA](https://www.jetbrains.com/idea/)
  * [MPS](https://www.jetbrains.com/mps/)
  * [PhpStorm](https://www.jetbrains.com/phpstorm/)
  * [PyCharm](https://www.jetbrains.com/pycharm/)
  * [Rider](#rider)
  * [RubyMine](https://www.jetbrains.com/ruby/)
  * [WebStorm](https://www.jetbrains.com/webstorm/)
* [Android Studio](https://developer.android.com/studio/index.html) IDE from Google
* [Comma](https://commaide.com/) IDE for Raku (以前称为 Perl 6)
* [Jmix Studio](https://www.jmix.io/tools/)

请参阅 *第 VIII 部分——产品特定*，获取 IDE 的特定细节。

### Rider

JetBrains [Rider](https://www.jetbrains.com/rider/) 使用 IntelliJ 平台与其他基于 IntelliJ 的 IDE 不同。
它使用 IntelliJ 平台为 C# 和 .NET IDE 提供用户界面，具有标准的 IntelliJ 编辑器、工具窗口、调试体验等功能。
它还集成到标准的查找用途和搜索 UI 中，并使用代码完成、语法高亮等功能。

但是，Rider 并不会为 C# 文件创建完整的 [PSI](psi.md)（句法和语义）模型。
相反，它重用 [ReSharper](https://www.jetbrains.com/resharper/) 提供语言功能。
所有的 C# PSI 模型、检查、代码重写（如快速修复和重构）都在进程外运行，在 ReSharper 的命令行版本中。
这意味着为 Rider 创建插件涉及两个部分——一个插件位于 IntelliJ "前端" 显示用户界面，另一个插件位于 ReSharper "后端" 分析和处理 C# PSI。

幸运的是，许多插件可以直接使用 ReSharper 后端工作。
Rider 负责显示检查和代码完成的结果，许多插件可以实现而无需需要 IntelliJ UI 组件。
有关更多详细信息，请参见 [](rider.md)。
