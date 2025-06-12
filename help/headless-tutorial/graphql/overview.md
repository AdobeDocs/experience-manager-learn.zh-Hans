---
title: AEM Headless 快速入门 – GraphQL
description: 了解 Experience Manager GraphQL API 及其功能。
feature: Content Fragments, GraphQL API, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 0056971f-2f89-43b3-bb6f-dd16c2a50370
thumbnail: 328618.jpg
last-substantial-update: 2022-07-20T00:00:00Z
duration: 626
source-git-commit: bd0f42fa37b7bbe19bf0d7fc65801198e64cbcd9
workflow-type: ht
source-wordcount: '266'
ht-degree: 100%

---

# AEM Headless 快速入门 – GraphQL {#getting-started-with-aem-headless}

AEM 的内容片段 GraphQL API 支持 Headless CMS 场景，使外部客户端应用程序能够使用在 AEM 中管理的内容来渲染用户体验。

现代化的内容传递 API 是提升基于 JavaScript 的前端应用程序效率与性能的关键。使用 REST API 会带来一些挑战：

* 一次仅获取一个对象会导致大量请求的产生
* 经常出现“内容过载”问题，即应用程序接收到的内容超出其实际所需。

为克服这些挑战，GraphQL 提供了一种基于查询的 API，使客户端能够仅请求 AEM 提供所需内容，并通过单次 API 调用接收响应。

>[!VIDEO](https://video.tv.adobe.com/v/328618?quality=12&learn=on)

本视频概述了 AEM 中实施的 GraphQL API。AEM 中的 GraphQL API 主要用于将 AEM 内容片段传递到作为 Headless 部署一部分的下游应用程序。

>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_overview"
>title="AEM Headless 快速入门 – GraphQL"
>abstract="了解如何使用 GraphQL 投放内容片段。"
>additional-url="https://video.tv.adobe.com/v/328618" text="AEM 中的 GraphQL 概述"

## AEM Headless GraphQL 视频系列

通过深入介绍内容片段以及 AEM 的 GraphQL API 和开发工具，了解 AEM 的 GraphQL 功能。

* [AEM Headless GraphQL 视频系列](./video-series/modeling-basics.md)

## AEM Headless GraphQL 实践教程

通过构建一个通过 AEM 的 GraphQL API 使用内容片段的 React 应用程序来探索 AEM 的 GraphQL 功能。

* [AEM Headless GraphQL 实践教程](./multi-step/overview.md)

## AEM GraphQL 与 AEM 内容服务

|                                | AEM GraphQL API | AEM 内容服务 |
|--------------------------------|:-----------------|:---------------------|
| 架构定义 | 结构化内容片段模型 | AEM 组件 |
| 内容 | 内容片段 | AEM 组件 |
| 内容探索 | 通过 GraphQL 查询 | 通过 AEM 页面 |
| 传递格式 | GraphQL JSON | AEM ComponentExporter JSON |
