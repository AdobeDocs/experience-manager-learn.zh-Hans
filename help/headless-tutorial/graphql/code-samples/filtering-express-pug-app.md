---
title: 筛选Express应用程序
description: 一个简单的Express应用程序，用于筛选使用内容片段建模的WKND冒险。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11812
thumbnail: KT-11812.jpg
index: false
hide: true
hidefromtoc: true
recommendations: noCatalog, noDisplay
exl-id: b64f33ab-cd18-4cbc-a57e-baf505f1442a
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 0%

---

# 筛选Express应用程序

探索AEM Headless GraphQL API使用过滤数据的能力 [Express](https://expressjs.com/) 和 [Pug](https://pugjs.org/) 应用程序。 此Express应用程序创建一个可按活动类型筛选的WKND冒险列表。

此代码演示了使用Adobe的 [适用于NodeJS的AEM Headless客户端](https://github.com/adobe/aem-headless-client-nodejs#aem-headless-client-for-nodejs) 使用基于Node.js的JavaScript调用持久GraphQL查询。 此应用程序使用 `wknd-shared/adventures-all` 持久查询以收集所有冒险，并派生可用活动类型的列表。 当用户选择活动类型时，选定的类型将传递给 `wknd-shared/adventures-by-activity` 持久查询并仅检索指定活动类型的那些冒险的冒险详细信息。 冒险详细信息可通过以下方式从AEM中检索： `wknd-shared/adventures-by-slug` 持久查询。

此代码：

+ 连接到AEM Publish服务，不需要身份验证
+ 使用WKND的持久查询： `wknd-shared/adventures-all`， `wknd-shared/adventures-by-activity`、和 `wknd-shared/adventures-by-slug`
