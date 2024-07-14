---
title: 筛选React应用程序
description: 一个简单的React应用程序，用于过滤使用内容片段建模的WKND冒险。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11132
thumbnail: KT-11132.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 1eb9487e-a82a-4d15-a776-cf004f2e3f01
duration: 26
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---

# 筛选React应用程序

探索AEM Headless GraphQL API使用[React](https://reactjs.org/)应用程序筛选数据的能力。 此React应用程序创建一个可按活动类型筛选的WKND Adventures列表。

此代码演示了使用Adobe的[AEM Headless Client for JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md)从React调用持久GraphQL查询。 此应用程序使用`wknd-shared/adventures-all`持久查询来收集所有冒险，并派生可用活动类型的列表。 当用户选择活动类型时，选定的类型将传递到`wknd-shared/adventures-by-activity`持久查询，并仅检索指定活动类型的那些冒险的冒险详细信息。

此代码：

+ 连接到AEM Publish服务，无需身份验证
+ 使用WKND的持久查询： `wknd-shared/adventures-all`和`wknd-shared/adventures-by-activity`
