---
title: 使用jQuery和Handlebars进行筛选
description: 使用jQuery和Handlebars的JavaScript实施，可筛选要显示的WKND冒险。.
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11135
thumbnail: KT-11135.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 75ffd84a-62b1-480f-b05f-3664f54bb171
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '146'
ht-degree: 0%

---

# 使用jQuery和Handlebars进行筛选

探索AEM Headless GraphQL API通过使用的JavaScript应用程序筛选数据的能力 [jQuery](https://jquery.com/) 和 [Handlebars](https://handlebarsjs.com/). 此应用程序创建一个可按活动类型筛选的WKND冒险列表。

此代码演示使用Adobe的 [适用于JavaScript的AEM Headless客户端](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) 以调用持久的GraphQL查询。 此应用程序使用 `wknd-shared/adventures-all` 持久查询以收集所有冒险并派生可用活动类型的列表。 当用户选择活动类型时，选定的类型将传递给 `wknd-shared/adventures-by-activity` 持久查询并仅检索指定活动类型的冒险的冒险详细信息。

此代码：

+ 连接到AEM Publish服务，无需身份验证
+ 使用WKND的持久查询： `wknd-shared/adventures-all` 和 `wknd-shared/adventures-by-activity`
