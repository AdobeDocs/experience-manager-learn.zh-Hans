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
duration: 29
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 0%

---

# 筛选Express应用程序

探索AEM Headless GraphQL API使用[Express](https://expressjs.com/)和[Pug](https://pugjs.org/)应用程序筛选数据的能力。 此Express应用程序创建一个可按活动类型筛选的WKND冒险列表。

此代码演示了使用Adobe的[AEM Headless Client for NodeJS](https://github.com/adobe/aem-headless-client-nodejs#aem-headless-client-for-nodejs)使用基于Node.js的JavaScript调用持久GraphQL查询。 此应用程序使用`wknd-shared/adventures-all`持久查询来收集所有冒险，并派生可用活动类型的列表。 当用户选择活动类型时，选定的类型将传递到`wknd-shared/adventures-by-activity`持久查询，并仅检索指定活动类型的那些冒险的冒险详细信息。 冒险详细信息是通过`wknd-shared/adventures-by-slug`持久查询从AEM中检索的。

此代码：

+ 连接到AEM Publish服务，无需身份验证
+ 使用WKND的持久查询： `wknd-shared/adventures-all`、`wknd-shared/adventures-by-activity`和`wknd-shared/adventures-by-slug`
