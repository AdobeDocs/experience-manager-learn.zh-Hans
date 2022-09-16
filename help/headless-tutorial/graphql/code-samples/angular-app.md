---
title: Angular应用程序
description: 一个简单的Angular应用程序，可显示使用内容片段建模的WKND冒险。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11133
thumbnail: KT-11133.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: 680ed62141b853daf104a827067ca6d5a209796d
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# Angular应用程序

使用简单的Angular应用程序浏览AEM无头GraphQL API。 此Angular应用程序连接到 [wknd.site](https://wknd.site)，会显示WKND冒险列表，并提供每个冒险的详细视图。

此代码：

+ 连接到 [wknd.site](https://wknd.site)的AEM发布服务，不需要身份验证
+ 使用持久查询： `wknd-shared/adventures-all` 和 `wknd-shared/adventures-by-slug`

