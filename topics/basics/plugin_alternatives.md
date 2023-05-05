# 实现插件的替代方案

<!-- Copyright 2000-2023 JetBrains s.r.o. and other contributors. Use of this source code is governed by the Apache 2.0 license that can be found in the LICENSE file. -->

<link-summary>避免构建“完整”插件的替代策略和工具。</link-summary>

在某些情况下，您可能不需要实现一个真正的IntelliJ平台插件，使用下面列出的替代方法可以在更短的时间内解决您的需求。
如果您需要特定于项目域、约定或实践的功能，您可以避免实现和发布插件所需的所有步骤，并将这些特性作为您的项目或IDE配置文件的一部分提供。

在开始IntelliJ平台插件开发之前，请定义您的要求并验证它们是否可以通过下面描述的任何替代方法来满足。
只有在所描述的解决方案在您的情况下不足以提供足够的价值，并且有大量开发人员可以从中受益时，才考虑实现实际的插件。

## 结构搜索和替换检查

[结构化搜索和替换（SSR）功能](https://www.jetbrains.com/help/idea/structural-search-and-replace.html)允许定义基于搜索代码片段结构的搜索模式，而不仅仅是基于文本信息，无论搜索的代码片段如何格式化或注释。
SSR模板可用于 [创建自定义检查](https://www.jetbrains.com/help/idea/creating-custom-inspections.html)，这可以是编程式 [代码检查](code_inspections.md)的替代方法。
根据要求，检查可以为与给定模板匹配的代码片段报告问题，但也可以提供快速修复，将报告的片段替换为配置的替换模板。
所有检查元数据，如名称、问题提示和描述，都是可配置的。
单个检查可以使用多个搜索和替换模板。

创建和配置SSR检查后，可以通过 [检查配置文件](https://www.jetbrains.com/help/idea/customizing-profiles.html)与其他团队成员共享。

只能为提供SSR支持的语言创建SSR检查。
要验证某种语言是否支持SSR，请在支持该语言的IDE中调用 <ui-path>编辑|查找|结构化搜索...</ui-path> 操作，并检查它是否出现在 <control>语言(Language)</control> 选择列表中。

> 请参阅 [I(J)nspector](https://ijnspector.wordpress.com/) 博客，了解实际的SSR模板示例。
>
{style="note"}

##  IDE脚本控制台

[IDE脚本控制台](https://www.jetbrains.com/help/idea/ide-scripting-console.html) 可用于自动化IDE的功能并提取所需信息，例如有关当前项目的信息。
脚本可以访问IntelliJ平台API，并默认情况下可以使用Kotlin、JavaScript或Groovy实现，但也可以使用符合 [JCR-223规范](https://www.jcp.org/en/jsr/detail?id=223) 的其他语言。

创建的脚本存储在 [IDE配置目录](https://www.jetbrains.com/help/idea/directories-used-by-the-ide-to-store-settings-caches-plugins-and-logs.html#config-directory) 中，不能作为项目文件或配置的一部分共享。

## Flora插件

[Flora](https://plugins.jetbrains.com/plugin/17669-flora-beta-) 插件允许开发项目特定的扩展，以Kotlin脚本 (<path>\*.kts</path>) 或JavaScript文件 (<path>\*.js</path>) 的格式。
Flora扩展可以像常规插件一样访问所有可用的IntelliJ平台API。

每个扩展由单个文件表示，并直接存储在项目的 <path>.plugins</path> 目录中。
通过将 <path>.plugins</path> 目录添加到版本控制系统，可以轻松地与其他团队成员共享扩展。
另外，在 <ui-path>Settings | Build, Execution, Deployment | Required Plugins</ui-path> 中添加Flora插件，并将此配置作为项目的一部分共享，可以轻松地向团队提供额外的IDE功能，而不需要任何手动设置。

## LivePlugin

[LivePlugin](https://plugins.jetbrains.com/plugin/7282-liveplugin) 允许在运行时扩展基于IntelliJ的IDE功能，无需重启IDE。
它添加了一个新的 <control>Plugins</control> 工具窗口，列出所有可用的扩展并允许管理它们。
扩展可以使用Kotlin或Groovy实现，并可以直接在IDE中进行编辑。
扩展可以使用所有IntelliJ平台API和额外的LivePlugin API，缩短常见用例。

创建的扩展存储在IDE级别上，并可以作为普通文件、GitHub gists或存储库与其他团队成员共享。
此外，如果它们存储在项目的 <path>.live-plugins</path> 目录中并且启用了LivePlugin的 <control>Run Project Specific Plugins(运行特定项目插件)</control> 选项，则该目录中的所有扩展将在打开项目时自动加载，并在关闭项目时卸载。

> 有关详细信息，请参阅 [描述](https://dmitrykandalov.com/liveplugin), [演示](https://www.youtube.com/watch?v=GcYa4lMRta0), 和 [扩展示例](https://github.com/dkandalov/live-plugin#more-examples) 。
>
{style="note"}

## PhpStorm高级元数据

[PhpStorm](https://www.jetbrains.com/phpstorm/) 支持描述方法和函数行为的特殊 [metadata files](https://www.jetbrains.com/help/phpstorm/ide-advanced-metadata.html)。
这些信息用于使用现有的IDE功能，如代码完成、导航、查找用法等。
元数据文件可以是 [项目文件的一部分](https://www.jetbrains.com/help/phpstorm/ide-advanced-metadata.html#create-metadata-files-inside-your-project)，这使得通过版本控制在团队成员之间共享它变得容易。
