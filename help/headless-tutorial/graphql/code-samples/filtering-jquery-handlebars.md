---
title: 使用 jQuery 和 Handlebars 进行过滤
description: JavaScript实施，使用jQuery和Handlebars来筛选要显示的WKND Adventures。
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11135
thumbnail: KT-11135.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 75ffd84a-62b1-480f-b05f-3664f54bb171
duration: 26
source-git-commit: 30b98e82e78120bf9fb13c9d41780af4c07665d8
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 7%

---

# 使用 jQuery 和 Handlebars 进行过滤

探索AEM Headless GraphQL API通过使用了[jQuery](https://jquery.com/)和[Handlebars](https://handlebarsjs.com/)的JavaScript应用程序筛选数据的能力。 此应用程序创建一个可按活动类型筛选的WKND Adventures列表。

此代码演示了使用Adobe的[AEM Headless Client for JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md)调用持久GraphQL查询。 此应用程序使用`wknd-shared/adventures-all`持久查询来收集所有冒险，并派生可用活动类型的列表。 当用户选择活动类型时，选定的类型将传递到`wknd-shared/adventures-by-activity`持久查询，并仅检索指定活动类型的那些冒险的冒险详细信息。

此代码：

+ 连接到AEM Publish服务，无需身份验证
+ 使用WKND的持久查询： `wknd-shared/adventures-all`和`wknd-shared/adventures-by-activity`
