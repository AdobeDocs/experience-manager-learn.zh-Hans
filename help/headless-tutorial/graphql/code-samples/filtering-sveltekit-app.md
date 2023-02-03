---
title: 简单SvelteKit应用程序
description: 一个简单的SvelteKit应用程序，可显示使用内容片段建模的WKND冒险。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11811
thumbnail: KT-11811.jpg
index: false
hide: true
recommendations: noCatalog, noDisplay
hidefromtoc: true
source-git-commit: a0a1c7e5d3dd74454b9b8ab787ce7447e73ee098
workflow-type: tm+mt
source-wordcount: '120'
ht-degree: 0%

---


# 筛选SvelteKit应用程序

探索AEM无头GraphQL API使用 [SvelteKit](https://kit.svelte.dev/) 应用程序。 此SvelteKit应用程序会创建WKND冒险列表，可选择该列表以显示冒险的详细信息。

此代码可演示如何使用Adobe [AEM Headless Client for JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) 从SvelteKit调用持久化GraphQL查询。 此应用程序使用 `wknd-shared/adventures-all` 保留查询以收集所有历险，并派生可用活动类型列表。 探险详情可通过 `wknd-shared/adventures-by-slug` 保留查询。

此代码：

+ 连接到AEM发布服务，且不需要身份验证
+ 使用WKND的持久查询： `wknd-shared/adventures-all` 和 `wknd-shared/adventures-by-slug`
