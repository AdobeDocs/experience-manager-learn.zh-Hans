---
title: 筛选Express应用程序
description: 一个简单的Express应用程序，可过滤使用内容片段建模的WKND冒险。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11812
thumbnail: KT-11812.jpg
index: false
hide: true
hidefromtoc: true
recommendations: noCatalog, noDisplay
source-git-commit: ac2b3a766caea1013165aedd3478bf859212cc89
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 0%

---


# 筛选Express应用程序

探索AEM无头GraphQL API使用 [快速](https://expressjs.com/) 和 [普格](https://pugjs.org/) 应用程序。 此Express应用程序会创建一个可按活动类型过滤的WKND冒险列表。

此代码可演示如何使用Adobe [AEM Headless Client for NodeJS](https://github.com/adobe/aem-headless-client-nodejs#aem-headless-client-for-nodejs) 使用基于Node.js的JavaScript调用持久化GraphQL查询。 此应用程序使用 `wknd-shared/adventures-all` 保留查询以收集所有历险，并派生可用活动类型列表。 当用户选择活动类型时，所选类型将传递到 `wknd-shared/adventures-by-activity` 保留查询，并仅检索指定活动类型的历程的冒险详细信息。 探险详情可通过以下路径从AEM中检索 `wknd-shared/adventures-by-slug` 保留查询。

此代码：

+ 连接到AEM发布服务，且不需要身份验证
+ 使用WKND的持久查询： `wknd-shared/adventures-all`, `wknd-shared/adventures-by-activity`和 `wknd-shared/adventures-by-slug`
