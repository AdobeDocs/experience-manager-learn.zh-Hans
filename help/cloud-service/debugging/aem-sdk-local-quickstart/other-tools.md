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
topic: Development
role: Developer
level: Beginner, Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '339'
ht-degree: 3%

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

## Sling Log Tracer和AEM Chrome插件

![Sling Log Tracer和AEM Chrome插件](./assets/other-tools/log-tracer.png)

[Sling Log Tracer随AEM](https://sling.apache.org/documentation/bundles/log-tracers.html) SDK的本地快速启动一起提供，它允许对HTTP请求进行深入跟踪，并为每个请求提供详细的调试信息。必须配置[Log Tracer OSGi配置](https://sling.apache.org/documentation/bundles/log-tracers.html#configuration-1)才能启用此功能。

用于[Google Chrome Web浏览器](https://www.google.com/chrome/)的开放源[AEM Chrome插件](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US)与Log Tracer集成，直接在Chrome的开发工具中显示调试信息。

_AEM Chrome插件是一个开放源代码工具，Adobe不提供对它的支持。_

