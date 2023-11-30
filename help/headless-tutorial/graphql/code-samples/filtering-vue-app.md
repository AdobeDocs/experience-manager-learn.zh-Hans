---
title: 正在筛选值应用程序
description: 一个简单的Vue应用程序，用于筛选使用内容片段建模的WKND冒险。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11366
thumbnail: KT-11366.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 8f96093a-4449-4249-9257-028e2ffd979b
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '137'
ht-degree: 0%

---

# 正在筛选值应用程序

探索AEM Headless GraphQL API使用过滤数据的能力 [值](https://vuejs.org/) 应用程序。 此React应用程序创建一个可按活动类型筛选的WKND Adventures列表。

此代码演示了使用Adobe的 [适用于JavaScript的AEM Headless客户端](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) 从Vue调用持久的GraphQL查询。 此应用程序使用 `wknd-shared/adventures-all` 持久查询以收集所有冒险，并派生可用活动类型的列表。 当用户选择活动类型时，选定的类型将传递给 `wknd-shared/adventures-by-activity` 持久查询并仅检索指定活动类型的那些冒险的冒险详细信息。

此代码：

+ 连接到AEM Publish服务，不需要身份验证
+ 使用WKND的持久查询： `wknd-shared/adventures-all` 和 `wknd-shared/adventures-by-activity`
