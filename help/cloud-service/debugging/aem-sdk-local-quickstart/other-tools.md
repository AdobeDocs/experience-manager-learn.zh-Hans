---
title: 用于调试AEM SDK的其他工具
description: 其他各种工具可以帮助调试AEM SDK的本地快速启动。
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5251
translation-type: tm+mt
source-git-commit: a3d3612713decefb5c7e1cf5b2d4d21afff0a2f5
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 2%

---


# 用于调试AEM SDK的其他工具

其他各种工具可以帮助在AEM SDK的本地快速启动上调试应用程序。

## CRXDE Lite

![CRXDE Lite](./assets/other-tools/crxde-lite.png)

CRXDE Lite是一个基于Web的界面，用于与AEM数据存储库JCR进行交互。 CRXDE Lite提供对JCR的完全可见性，包括节点、属性、属性值和权限。

CRXDE Lite位于：

+ “工具”>“常规”>“CRXDE Lite”
+ 或直接在[http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)上

## 说明查询

![说明查询](./assets/other-tools/explain-query.png)

解释AEM SDK本地快速启动中基于查询Web的工具，它提供关于AEM如何解释和执行查询的重要见解，以及一个宝贵的工具，可确保查询以AEM的性能方式执行。

说明查询位于：

+ “工具”>“诊断”>“查询性能”>“说明查询”选项卡
+ [http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) >“说明”查询选项卡

## QueryBuilder调试器

![QueryBuilder调试器](./assets/other-tools/query-debugger.png)

QueryBuilder调试器是基于Web的工具，可帮助您使用AEM [QueryBuilder](https://docs.adobe.com/content/help/en/experience-manager-65/developing/platform/query-builder/querybuilder-api.html)语法调试和了解搜索查询。

QueryBuilder调试器位于：

+ [http://localhost:4502/libs/cq/search/content/querydebug.html](http://localhost:4502/libs/cq/search/content/querydebug.html)

## Sling Log Tracer和AEM Chrome插件

![Sling Log Tracer和AEM Chrome插件](./assets/other-tools/log-tracer.png)

[Sling Log Tracer随AEM](https://sling.apache.org/documentation/bundles/log-tracers.html) SDK的本地快速启动一起提供，它允许对HTTP请求进行深入跟踪，从而揭示每个请求的深入调试信息。必须配置[Log Tracer OSGi配置](https://sling.apache.org/documentation/bundles/log-tracers.html#configuration-1)才能启用此功能。

[Google Chrome Web浏览器](https://www.google.com/chrome/)的开放源[AEM Chrome插件](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US)与Log Tracer集成，直接在Chrome的Dev Tools中显示调试信息。

_AEM Chrome插件是一个开放源代码工具，Adobe不支持它。_

