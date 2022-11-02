---
title: 过滤预警应用程序
description: 一个简单的预作应用程序，可过滤使用内容片段建模的WKND冒险。
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
source-git-commit: a21b78456354c18ad137e69a5d18258d652169b1
workflow-type: tm+mt
source-wordcount: '137'
ht-degree: 0%

---


# 过滤预警应用程序

探索AEM无头GraphQL API使用 [Preact](https://preactjs.com/) 应用程序。 此Preact应用程序会创建一个WKND冒险列表，该列表可按活动类型过滤。

此代码可演示如何使用Adobe [AEM Headless Client for JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) 从React中调用持久GraphQL查询。 此应用程序使用 `wknd-shared/adventures-all` 保留查询以收集所有历险，并派生可用活动类型列表。 当用户选择活动类型时，所选类型将传递到 `wknd-shared/adventures-by-activity` 保留查询，并仅检索指定活动类型的历程的冒险详细信息。

此代码：

+ 连接到AEM发布服务，且不需要身份验证
+ 使用WKND的持久查询： `wknd-shared/adventures-all` 和 `wknd-shared/adventures-by-activity`
