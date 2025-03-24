---
title: 过滤前置应用程序
description: 一个简单的预操作应用程序，用于过滤使用内容片段建模的WKND冒险。
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11389
thumbnail: KT-11389.jpg
index: false
hide: true
hidefromtoc: true
exl-id: d2b7e8ab-8bbc-495f-94f1-362ea47b3853
duration: 26
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---

# 过滤前置应用程序

探索AEM Headless GraphQL API使用[Preact](https://preactjs.com/)应用程序筛选数据的能力。 此Preact应用程序创建一个可按活动类型筛选的WKND Adventures列表。

此代码演示了使用Adobe的[AEM Headless Client for JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md)从React调用持久化GraphQL查询。 此应用程序使用`wknd-shared/adventures-all`持久查询来收集所有冒险，并派生可用活动类型的列表。 当用户选择活动类型时，选定的类型将传递到`wknd-shared/adventures-by-activity`持久查询，并仅检索指定活动类型的那些冒险的冒险详细信息。

此代码：

+ 连接到AEM Publish服务，无需身份验证
+ 使用WKND的持久查询： `wknd-shared/adventures-all`和`wknd-shared/adventures-by-activity`
