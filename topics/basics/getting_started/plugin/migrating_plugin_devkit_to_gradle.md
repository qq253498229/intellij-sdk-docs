# 将 DevKit 插件迁移到 Gradle

<!-- Copyright 2000-2022 JetBrains s.r.o. and contributors. Use of this source code is governed by the Apache 2.0 license. -->

<link-summary>将现有基于 DevKit 的插件迁移到 Gradle 设置。</link-summary>

> 请参阅 [Revamping Plugins #3 – Migrating from DevKit to the Gradle build system](https://blog.jetbrains.com/platform/2021/12/migrating-from-devkit-to-the-gradle-build-system/) 博客文章，以获得逐步指南。

将使用旧的 DevKit 方法创建的插件（可用于 [creating themes](creating_theme_project.md)）转换为基于 Gradle 的插件项目可以使用 <control>New Project</control> 向导创建围绕现有的基于 DevKit 的项目的基于 Gradle 的项目：
* 确保包含基于 DevKit 的 IntelliJ 平台插件项目的目录可以在必要时完全恢复。
* 删除基于 DevKit 的项目的所有工件：
    * <path>.idea</path> 目录
    * <path>[modulename].iml</path> 文件
    * <path>out</path> 目录
* 按 Gradle [源集格式](https://docs.gradle.org/current/userguide/java_plugin.html#sec:java_project_layout)在项目目录中排列现有源文件。
* 像创建新的基于 Gradle 的插件项目一样使用 <control>[New Project](creating_plugin_project.md#create-ide-plugin)</control> 向导。在 <control>New Project</control> 页面上选择 <control>IDE Plugin</control> generator 并设置以下值：
    * <control>Group</control> 为初始源集中现有包的名称。
    * <control>Artifact</control> 为现有插件的名称。
    * <control>Name</control> 为包含现有插件的目录的名称，例如，如果插件项目基本目录为 <path>/Users/john/Projects/old_plugin</path>，则应为 <path>old_plugin</path>。
    * <control>Location</control> 为插件的父目录的名称，例如，如果插件项目基本目录为 <path>/Users/john/Projects/old_plugin</path>，则它应该是 <path>/Users/john/Projects</path>。
* 单击 <control>Finish</control> 创建新的基于 Gradle 的插件。
* 根据需要使用 Gradle [源集](https://www.jetbrains.com/help/idea/gradle.html#gradle_source_sets) [添加更多模块](https://www.jetbrains.com/help/idea/gradle.html#gradle_add_module)。
