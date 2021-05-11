---
title: 用于调试AEM SDK的其他工具
description: 其他各种工具可以帮助调试AEM SDK的本地快速启动。
feature: Developer Tools
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5251
topic: 开发
role: Developer
level: Beginner, Intermediate
translation-type: tm+mt
source-git-commit: 5fcc7eec120debf1a8ac08716154599467e66759
workflow-type: tm+mt
source-wordcount: '222'
ht-degree: 4%

---


# 用于调试AEM SDK的其他工具

其他各种工具可帮助在AEM SDK的本地快速启动上调试应用程序。

## CRXDE Lite

![CRXDE Lite](./assets/other-tools/crxde-lite.png)

CRXDE Lite是一个基于Web的界面，用于与AEM JCR数据存储库进行交互。 CRXDE Lite提供对JCR的完全可见性，包括节点、属性、属性值和权限。

CRXDE Lite位于：

+ “工具”>“常规”>“CRXDE Lite”
+ 或直接在[http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)

## 说明查询

![说明查询](./assets/other-tools/explain-query.png)

解释AEM SDK的本地快速入门中基于查询 Web的工具，它提供了有关AEM如何解释和执行查询的关键见解，以及确保AEM以高性能方式执行查询的宝贵工具。

说明查询位于：

+ “工具”>“诊断”>“查询性能”>“说明查询”选项卡
+ [http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) >说明查询选项卡

## QueryBuilder调试器

![QueryBuilder调试器](./assets/other-tools/query-debugger.png)

QueryBuilder调试器是基于Web的工具，可帮助您使用AEM [QueryBuilder](https://docs.adobe.com/content/help/en/experience-manager-65/developing/platform/query-builder/querybuilder-api.html)语法调试和了解搜索查询。

QueryBuilder调试器位于：

+ [http://localhost:4502/libs/cq/search/content/querydebug.html](http://localhost:4502/libs/cq/search/content/querydebug.html)

