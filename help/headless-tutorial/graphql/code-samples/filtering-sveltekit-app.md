---
title: 简单SvelteKit应用程序
description: 一个简单的SvelteKit应用程序，可显示使用内容片段建模的WKND冒险。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11811
thumbnail: KT-11811.jpg
index: false
hide: true
recommendations: noCatalog, noDisplay
hidefromtoc: true
exl-id: 2e5bd50e-c0d7-4292-8097-e0a17f41a91a
duration: 27
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '113'
ht-degree: 0%

---

# 筛选SvelteKit应用程序

探索AEM Headless GraphQL API使用列出数据的能力 [SvelteKit](https://kit.svelte.dev/) 应用程序。 此SvelteKit应用程序创建WKND冒险列表，可以选择该列表来显示冒险的详细信息。

此代码演示了使用Adobe的 [适用于JavaScript的AEM Headless客户端](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) 从SvelteKit调用持久GraphQL查询。 此应用程序使用 `wknd-shared/adventures-all` 持久查询以收集所有冒险，并派生可用活动类型的列表。 如欲了解冒险详情，请访问 `wknd-shared/adventures-by-slug` 持久查询。

此代码：

+ 连接到AEM Publish服务，不需要身份验证
+ 使用WKND的持久查询： `wknd-shared/adventures-all` 和 `wknd-shared/adventures-by-slug`
