---
title: AEM Headless快速入门 — Content Services
description: 一个端到端教程，它演示了如何使用 AEM Headless 构建和展示内容。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 5aa32791-861a-48e3-913c-36028373b788
duration: 311
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '327'
ht-degree: 4%

---

# AEM Headless快速入门 — Content Services

AEM Content Services利用传统AEM Pages来构建Headless REST API端点，并且AEM组件定义或引用要在这些端点上公开的内容。

AEM Content Services允许使用与在AEM Sites中创作网页相同的内容抽象概念，来定义这些HTTP API的内容和架构。 通过使用AEM Pages和AEM组件，营销人员能够快速编写和更新可以为任何应用程序提供支持的灵活JSON API。

## Content Services教程

一个端到端教程，它演示了如何在Headless CMS场景中使用AEM构建和公开内容并由本机移动设备应用程序使用。

>[!VIDEO](https://video.tv.adobe.com/v/28315?quality=12&learn=on)

本教程将探讨如何使用AEM Content Services增强移动设备应用程序显示事件信息（音乐、性能、艺术等）的体验 由WKND团队策划。

本教程将涵盖以下主题：

* 使用内容片段创建表示事件的内容
* 使用AEM Sites的模板和将事件数据公开为JSON的页面定义AEM Content Services端点
* 探索如何使用AEM WCM核心组件使营销人员能够创作JSON端点
* 使用移动应用程序中的AEM Content Services JSON
   * Android的使用是因为它具有跨平台模拟器，本教程的所有用户(Windows、macOS和Linux)都可以使用它来运行本机应用程序。

## GitHub项目

源代码和内容包位于 [AEM指南 — WKND Mobile GitHub项目](https://github.com/adobe/aem-guides-wknd-mobile).

如果您发现教程或代码有问题，请留言 [GitHub问题](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## AEM GraphQL与AEM内容服务

|                                | AEM GRAPHQL API | AEM内容服务 |
|--------------------------------|:-----------------|:---------------------|
| 架构定义 | 结构化内容片段模型 | AEM组件 |
| 内容 | 内容片段 | AEM组件 |
| 内容发现 | 按GraphQL查询 | 按AEM页 |
| 投放格式 | GRAPHQL JSON | AEM组件导出程序JSON |
