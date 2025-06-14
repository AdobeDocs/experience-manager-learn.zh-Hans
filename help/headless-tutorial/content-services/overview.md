---
title: 入门指南：AEM Headless —— 内容服务
description: 一个端到端教程，演示如何使用 AEM Headless 构建并展示内容。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 5aa32791-861a-48e3-913c-36028373b788
duration: 311
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '327'
ht-degree: 100%

---

# 入门指南：AEM Headless —— 内容服务

AEM 的内容服务通过传统的 AEM 页面来构建 Headless REST API 端点，而 AEM 组件则会定义或引用需要在这些端点上展示的内容。

AEM 内容服务允许使用与 AEM Sites 中网页创作相同的内容抽象方式，来定义这些 HTTP API 所需的内容和架构。通过结合 AEM 页面与 AEM 组件，市场营销人员可以快速构建和更新灵活的 JSON API，以支持各种应用程序。

## 内容服务教程

一个端到端的教程，展示如何使用 AEM 构建并展示内容，并在 Headless CMS 场景中由原生移动应用程序使用。

>[!VIDEO](https://video.tv.adobe.com/v/33073?quality=12&learn=on&captions=chi_hans)

本教程将探讨如何利用 AEM 内容服务来增强移动应用程序的用户体验。该应用程序展示由 WKND 团队策划的活动信息（音乐、表演、艺术等）。

本教程将涵盖以下主题：

* 使用内容片段创建表示“活动”的内容
* 使用 AEM Sites 的模板和页面定义 AEM 内容服务端点，这些模板和页面以 JSON 格式展示事件数据
* 探索如何利用 AEM WCM 核心组件，帮助营销人员创作 JSON 端点
* 从移动应用中消费 AEM 内容服务 JSON
   * 本教程选择 Android 作为示例平台，是因为其拥有跨平台模拟器，适用于所有用户（包括 Windows、macOS 和 Linux 用户）运行原生应用程序。

## GitHub 项目

本教程所用的源代码和内容包可在 [AEM Guides - WKND Mobile GitHub 项目](https://github.com/adobe/aem-guides-wknd-mobile)中获取。

如果您在使用教程或代码时发现问题，欢迎[在 GitHub 提交问题](https://github.com/adobe/aem-guides-wknd-mobile/issues)。

## AEM GraphQL 与 AEM 内容服务

|                                | AEM GraphQL API | AEM 内容服务 |
|--------------------------------|:-----------------|:---------------------|
| 架构定义 | 结构化内容片段模型 | AEM 组件 |
| 内容 | 内容片段 | AEM 组件 |
| 内容探索 | 通过 GraphQL 查询 | 通过 AEM 页面 |
| 投放格式 | GraphQL JSON | AEM ComponentExporter JSON |
