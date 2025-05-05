---
title: AEM Headless入门 — GraphQL
description: 了解Experience Manager GraphQL API及其功能。
feature: Content Fragments, GraphQL API, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 0056971f-2f89-43b3-bb6f-dd16c2a50370
thumbnail: 328618.jpg
last-substantial-update: 2022-07-20T00:00:00Z
duration: 626
source-git-commit: bd0f42fa37b7bbe19bf0d7fc65801198e64cbcd9
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 10%

---

# AEM Headless 快速入门 – GraphQL {#getting-started-with-aem-headless}

AEM的内容片段GraphQL API
支持无头CMS方案，其中外部客户端应用程序使用AEM中管理的内容呈现体验。

现代化的内容交付API是基于Javascript的前端应用程序效率和性能的关键。 使用REST API会带来挑战：

* 每次获取一个对象的请求数量很大
* 通常“超量提供”内容，这意味着应用程序收到的内容多于其需要的内容

为了克服这些挑战，GraphQL提供了基于查询的API，允许客户端仅在AEM中查询所需的内容，并使用单个API调用进行接收。

>[!VIDEO](https://video.tv.adobe.com/v/3452889?quality=12&learn=on&captions=chi_hans)

此视频概述了在AEM中实现的GraphQL API。 AEM中的GraphQL API主要用于将AEM内容片段的交付给下游应用程序，作为Headless部署的一部分。

>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_overview"
>title="AEM Headless 快速入门 – GraphQL"
>abstract="了解如何使用 GraphQL 交付内容片段。"
>additional-url="https://video.tv.adobe.com/v/3452889?captions=chi_hans" text="AEM 中的 GraphQL 概述"

## AEM Headless GraphQL视频系列

通过内容片段和AEM的GraphQL API及开发工具的深入演练，了解AEM的GraphQL功能。

* [AEM Headless GraphQL视频系列](./video-series/modeling-basics.md)

## AEM Headless GraphQL实践教程

通过通过AEM的GraphQL API构建一个使用内容片段的React应用程序，探索AEM的GraphQL功能。

* [AEM Headless GraphQL实践教程](./multi-step/overview.md)

## AEM GraphQL与AEM内容服务

|                                | AEM GRAPHQL API | AEM内容服务 |
|--------------------------------|:-----------------|:---------------------|
| 架构定义 | 结构化内容片段模型 | AEM组件 |
| 内容 | 内容片段 | AEM组件 |
| 内容发现 | 按GraphQL查询 | 按AEM页面 |
| 投放格式 | GRAPHQL JSON | AEM组件导出器JSON |
