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
exl-id: 2e5bd50e-c0d7-4292-8097-e0a17f41a91a
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '120'
ht-degree: 0%

---

# 筛选SvelteKit应用程序

探索AEM Headless GraphQL API使用列出数据的能力 [SvelteKit](https://kit.svelte.dev/) 应用程序。 此SvelteKit应用程序创建WKND冒险列表，可以选择该列表以显示冒险的详细信息。

此代码演示使用Adobe的 [适用于JavaScript的AEM Headless客户端](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) 以从SvelteKit调用持久化GraphQL查询。 此应用程序使用 `wknd-shared/adventures-all` 持久查询以收集所有冒险并派生可用活动类型的列表。 冒险详情请通过以下网站查询： `wknd-shared/adventures-by-slug` 持久查询。

此代码：

+ 连接到AEM Publish服务，无需身份验证
+ 使用WKND的持久查询： `wknd-shared/adventures-all` 和 `wknd-shared/adventures-by-slug`
