---
title: 过滤预操作应用程序
description: 一个简单的预操作应用程序，用于筛选使用内容片段建模的WKND冒险。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11389
thumbnail: KT-11389.jpg
index: false
hide: true
hidefromtoc: true
exl-id: d2b7e8ab-8bbc-495f-94f1-362ea47b3853
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '137'
ht-degree: 0%

---

# 过滤预操作应用程序

探索AEM Headless GraphQL API使用过滤数据的能力 [预处理](https://preactjs.com/) 应用程序。 此Preact应用程序创建一个可按活动类型筛选的WKND冒险列表。

此代码演示使用Adobe的 [适用于JavaScript的AEM Headless客户端](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) 以从React调用持久化GraphQL查询。 此应用程序使用 `wknd-shared/adventures-all` 持久查询以收集所有冒险并派生可用活动类型的列表。 当用户选择活动类型时，选定的类型将传递给 `wknd-shared/adventures-by-activity` 持久查询并仅检索指定活动类型的冒险的冒险详细信息。

此代码：

+ 连接到AEM Publish服务，无需身份验证
+ 使用WKND的持久查询： `wknd-shared/adventures-all` 和 `wknd-shared/adventures-by-activity`
