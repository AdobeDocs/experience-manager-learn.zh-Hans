---
title: 開始使用AEM Headless - GraphQL
description: 瞭解Experience ManagerGraphQL API及其功能。
feature: Content Fragments, GraphQL API, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 0056971f-2f89-43b3-bb6f-dd16c2a50370
thumbnail: 328618.jpg
last-substantial-update: 2022-07-20T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 11%

---

# AEM Headless 快速入门 – GraphQL {#getting-started-with-aem-headless}

內容片段的AEM GraphQL API支援Headless CMS案例，其中外部使用者端應用程式會使用AEM中管理的內容呈現體驗。

現代化的內容傳送API是Javascript型前端應用程式效率和效能的關鍵。 使用REST API會帶來挑戰：

* 一次擷取一個物件的大量請求
* 通常「超量傳遞」內容，這表示應用程式收到的內容多於其需求

為克服這些挑戰，GraphQL提供查詢式API，讓使用者端只要查詢AEM所需的內容，並使用單一API呼叫進行接收。

>[!VIDEO](https://video.tv.adobe.com/v/328618?quality=12&learn=on)

這部影片會概略介紹在AEM中實作的GraphQL API。 AEM中的GraphQL API主要是為了將AEM內容片段的傳送給下游應用程式，作為Headless部署的一部分。

>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_overview"
>title="AEM Headless 快速入门 – GraphQL"
>abstract="了解如何使用 GraphQL 交付内容片段。"
>additional-url="https://video.tv.adobe.com/v/328618" text="AEM 中的 GraphQL 概述"

## AEM Headless GraphQL影片系列

透過內容片段和AEM GraphQL API及開發工具的深入逐步解說，瞭解AEM GraphQL功能。

* [AEM Headless GraphQL影片系列](./video-series/modeling-basics.md)

## AEM Headless GraphQL實作教學課程

透過AEM GraphQL API建置使用內容片段的React應用程式，探索AEM GraphQL功能。

* [AEM Headless GraphQL實作教學課程](./multi-step/overview.md)

## AEM GraphQL與AEM Content Services

|  | AEM GRAPHQL API | AEM內容服務 |
|--------------------------------|:-----------------|:---------------------|
| 結構描述定義 | 結構化內容片段模型 | AEM Components |
| 内容 | 内容片段 | AEM Components |
| 內容探索 | 依GraphQL查詢 | 依AEM頁面 |
| 傳遞格式 | GRAPHQL JSON | AEM ComponentExporter JSON |
