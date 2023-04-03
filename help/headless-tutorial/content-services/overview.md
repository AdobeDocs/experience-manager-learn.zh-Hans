---
title: AEM Headless快速入门 — 内容服务
description: 一个端到端教程，它演示了如何使用 AEM Headless 构建和展示内容。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 5aa32791-861a-48e3-913c-36028373b788
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 6%

---

# AEM Headless 快速入门 - Content Services

AEM内容服务利用传统的AEM页面来撰写无头REST API端点，并且AEM组件定义或引用要在这些端点上公开的内容。

AEM Content Services允许使用与在AEM Sites中创作网页相同的内容抽象概念，来定义这些HTTP API的内容和模式。 使用AEM页面和AEM组件，营销人员能够快速编写和更新可支持任何应用程序的灵活JSON API。

## 内容服务教程

一个端到端教程，其中演示了如何在无头CMS场景中使用AEM构建和显示由本机移动设备应用程序使用的内容。

>[!VIDEO](https://video.tv.adobe.com/v/28315?quality=12&learn=on)

本教程探讨如何使用AEM Content Services来提升显示事件信息（音乐、性能、艺术等）的移动设备应用程序的体验 由WKND团队策划。

本教程将涵盖以下主题：

* 使用内容片段创建表示事件的内容
* 使用AEM Sites的模板和将事件数据显示为JSON的页面定义AEM Content Services端点
* 了解如何使用AEM WCM核心组件来使营销人员能够创作JSON端点
* 从移动设备应用程序中使用AEM Content Services JSON
   * 使用Android是因为它有一个跨平台模拟器，本教程的所有用户(Windows、macOS和Linux)都可以使用该模拟器来运行本机应用程序。

## GitHub项目

源代码和内容包位于 [AEM指南 — WKND Mobile GitHub项目](https://github.com/adobe/aem-guides-wknd-mobile).

如果您发现本教程或代码存在问题，请保留 [GitHub问题](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## AEM GraphQL与AEM Content Services

|  | AEM GraphQL API | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| 架构定义 | 结构化内容片段模型 | AEM组件 |
| 内容 | 内容片段 | AEM组件 |
| 内容发现 | 按GraphQL查询 | 按AEM页面 |
| 投放格式 | GraphQL JSON | AEM ComponentExporter JSON |
