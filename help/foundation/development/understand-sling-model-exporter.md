---
title: 了解AEM中的Sling模型导出器
description: Apache Sling模型1.3.0引入了Sling模型导出器，这是一种将Sling模型对象导出或序列化为自定义抽象对象的简单方法。 本文比较了使用Sling模型填充HTL脚本的传统用例，并利用Sling模型导出器框架将Sling模型序列化为JSON。
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
doc-type: Article
topic: Development
role: Developer
level: Beginner
exl-id: 03cdf5d1-3253-44c9-ae1f-ec5d3c562427
duration: 133
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '568'
ht-degree: 0%

---

# 了解[!DNL Sling Model Exporter]

Apache [!DNL Sling Models] 1.3.0引入了[!DNL Sling Model Exporter]，这是一种将[!DNL Sling Model]对象导出或序列化为自定义抽象化对象的简便方法。 本文比较了使用[!DNL Sling Models]填充HTL脚本的传统用例，并利用[!DNL Sling Model Exporter]框架将[!DNL Sling Model]序列化为JSON。

## 传统Sling模型HTTP请求流

[!DNL Sling Models]的传统用例是为资源或请求提供业务抽象，这提供了HTL脚本（或以前的JSP）用于访问业务功能的接口。

常见模式是开发表示AEM组件或页面的[!DNL Sling Models]，并使用[!DNL Sling Model]对象向HTL脚本提供数据，最后在浏览器中显示HTML结果。

### Sling模型HTTP请求流

![Sling模型请求流](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. 在AEM中为资源发出[!DNL HTTP GET]请求。

   示例： `HTTP GET /content/my-resource.html`

1. 根据请求资源的`sling:resourceType`，解析相应的脚本。

1. 该脚本将请求或资源调整到所需的[!DNL Sling Model]。

1. 脚本使用[!DNL Sling Model]对象生成HTML呈现版本。

1. 脚本生成的HTML将在HTTP响应中返回。

此传统模式在生成HTML的上下文中运行良好，因为可以通过HTL轻松利用[!DNL Sling Model]。 创建更多结构化数据（如JSON或XML）是一项繁琐得多的工作，因为HTL本身并不自然适合这些格式的定义。

## [!DNL Sling Model Exporter] HTTP请求流

Apache [!DNL Sling Model Exporter]附带了一个Sling提供的Jackson导出程序，可自动将“普通”[!DNL Sling Model]对象序列化为JSON。 Jackson Exporter虽然可以配置，但其核心会检查[!DNL Sling Model]对象，并使用任何“getter”方法作为JSON键生成JSON，并使用getter返回值作为JSON值。

[!DNL Sling Models]的直接序列化允许它们使用使用传统[!DNL Sling Model]HTML流（请参阅上文）创建的请求响应为常规Web请求提供服务，但也会公开Web服务或JavaScript应用程序可以使用的JSON演绎版。

![Sling模型导出程序HTTP请求流](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*此流程描述使用提供的Jackson导出程序生成JSON输出的流程。 自定义导出程序的使用遵循相同的流程，但采用其输出格式。*

1. 在AEM中使用选择器和扩展名向[!DNL Sling Model]的导出程序注册的资源发出HTTPGET请求。

   示例： `HTTP GET /content/my-resource.model.json`

1. Sling将所请求资源的`sling:resourceType`、选择器和扩展解析为动态生成的Sling导出程序Servlet，后者通过导出程序映射到[!DNL Sling Model]。
1. 已解析的Sling导出器Servlet针对从请求或资源调整的[!DNL Sling Model]对象调用[!DNL Sling Model Exporter]（由Sling模型自适应表确定）。
1. 导出器根据导出器选项和特定于导出器的Sling模型注释来序列化[!DNL Sling Model]，并将结果返回到Sling导出器Servlet。
1. Sling导出程序Servlet在HTTP响应中返回[!DNL Sling Model]的JSON演绎版。

>[!NOTE]
>
>虽然Apache Sling项目提供了将[!DNL Sling Models]序列化为JSON的Jackson导出程序，但导出程序框架也支持自定义导出程序。 例如，项目可以实施将[!DNL Sling Model]序列化为XML的自定义导出程序。

>[!NOTE]
>
>不仅[!DNL Sling Model Exporter] *序列化* [!DNL Sling Models]，它还可以将它们导出为Java对象。 导出到其他Java对象在HTTP请求流中不起作用，因此未出现在上图中。

## 支持材料

* [Apache [!DNL Sling Model Exporter] Framework文档](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
