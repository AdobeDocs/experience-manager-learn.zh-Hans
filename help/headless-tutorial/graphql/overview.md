---
title: AEM无头入门 — GraphQL
description: 了解Experience ManagerGraphQL API及其功能。
feature: Content Fragments, GraphQL API, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 0056971f-2f89-43b3-bb6f-dd16c2a50370
source-git-commit: fb4a39a7b057ca39bc4cd4a7bce02216c3eb634c
workflow-type: tm+mt
source-wordcount: '246'
ht-degree: 1%

---

# AEM无头入门 — GraphQL

AEM GraphQL API（内容片段）支持无头CMS方案，其中外部客户端应用程序使用AEM中管理的内容渲染体验。

现代内容交付API是基于Javascript的前端应用程序效率和性能的关键。 使用REST API会带来以下挑战：

* 一次获取一个对象的请求数量很大
* 通常是“过度投放”内容，这意味着应用程序收到的内容超出了其需求

为了克服这些难题，GraphQL提供了一个基于查询的API，允许客户仅查询AEM所需的内容，并使用单个API调用接收。

>[!VIDEO](https://video.tv.adobe.com/v/328618/?quality=12&learn=on)

以下视频概述了在AEM中实施的GraphQL API。 AEM中的GraphQL API主要用于将AEM内容片段交付到作为无头部署一部分的下游应用程序。

## AEM无头GraphQL视频系列

通过深入演练内容片段以及AEM GraphQL API和开发工具，了解AEM GraphQL功能。

* [AEM无头GraphQL视频系列](./video-series/modeling-basics.md)

## AEM无头GraphQL动手教程

通过构建通过AEM GraphQL API使用内容片段的React应用程序来探索AEM GraphQL功能。

* [AEM无头GraphQL动手教程](./multi-step/overview.md)

## AEM GraphQL与AEM Content Services

|  | AEM GraphQL API | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| 架构定义 | 结构化内容片段模型 | AEM组件 |
| 内容 | 内容片段 | AEM组件 |
| 内容发现 | 按GraphQL查询 | 按AEM页面 |
| 投放格式 | GraphQL JSON | AEM ComponentExporter JSON |
