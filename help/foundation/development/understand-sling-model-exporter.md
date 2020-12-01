---
title: 了解AEM中的Sling Model Exporter
description: Apache Sling Models 1.3.0引入了Sling Model Exporter，这是一种将Sling Model对象导出或序列化为自定义抽象的优雅方法。 本文将使用Sling Models填充HTL脚本的传统用例与利用Sling Model Exporter框架将Sling Model序列化为JSON并列。
version: 6.3, 6.4, 6.5
sub-product: 基础，内容服务
feature: sling-models, sling-model-exporter
topics: development, content-delivery, headless
activity: understand
audience: developer, architect
doc-type: article
translation-type: tm+mt
source-git-commit: 63295cbc353a796959ba2e98e3e21c188f596372
workflow-type: tm+mt
source-wordcount: '571'
ht-degree: 0%

---


# 了解[!DNL Sling Model Exporter]

Apache [!DNL Sling Models] 1.3.0引入了[!DNL Sling Model Exporter]，这是一种将[!DNL Sling Model]对象导出或序列化为自定义抽象的优雅方法。 本文将使用[!DNL Sling Models]填充HTL脚本的传统用例与利用[!DNL Sling Model Exporter]框架将[!DNL Sling Model]序列化为JSON并列。

## 传统的Sling模型HTTP请求流

[!DNL Sling Models]的传统用例是为资源或请求提供业务抽象，为HTL脚本（或以前的JSP）提供访问业务函数的接口。

常见的模式正在开发代表AEM组件或页面的[!DNL Sling Models]，并使用[!DNL Sling Model]对象向HTL脚本提供数据，其最终结果是浏览器中显示的HTML。

### Sling Model HTTP请求流

![吊索模型请求流](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET] 请求AEM中的资源。

   示例: `HTTP GET /content/my-resource.html`

1. 根据请求资源的`sling:resourceType`，解析相应的脚本。

1. 脚本将请求或资源调整为所需的[!DNL Sling Model]。

1. 脚本使用[!DNL Sling Model]对象生成HTML再现。

1. 脚本生成的HTML在HTTP响应中返回。

这种传统模式在生成HTML的上下文中工作良好，因为[!DNL Sling Model]可以通过HTL轻松利用。 创建更多结构化数据（如JSON或XML）更加繁琐，因为HTL不会自然而然地定义这些格式。

## [!DNL Sling Model Exporter] HTTP请求流

Apache [!DNL Sling Model Exporter]附带Sling提供的Jackson Exporter，可自动将“普通”[!DNL Sling Model]对象序列化为JSON。 Jackson Exporter虽然可以进行相当配置，但其核心检查[!DNL Sling Model]对象，并使用任何“getter”方法作为JSON键生成JSON，而getter返回值作为JSON值生成。

[!DNL Sling Models]的直接序列化允许他们通过使用传统的[!DNL Sling Model]请求流创建的HTML响应服务普通Web请求（请参阅上文），同时还提供Web服务或JavaScript应用程序可能使用的JSON再现。

![Sling Model Exporter HTTP请求流](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*此流描述使用提供的Jackson Exporter生成JSON输出的流。使用自定义导出程序遵循相同的流程，但输出格式相同。*

1. 对AEM中的资源发出HTTPGET请求，选择器和扩展已在[!DNL Sling Model]的导出器中注册。

   示例: `HTTP GET /content/my-resource.model.json`

1. Sling将解析所请求资源的`sling:resourceType`选择器和扩展，并将其解析为动态生成的Sling Exporter Servlet，该Sling Exporter Servlet将映射到带Exporter的[!DNL Sling Model]。
1. 解析的Sling Exporter Servlet针对根据请求或资源调整的[!DNL Sling Model]对象调用[!DNL Sling Model Exporter]（由Sling Models适配器确定）。
1. 导出器根据导出器选项和导出器特定的Sling模型注释序列化[!DNL Sling Model]，并将结果返回给Sling Exporter Servlet。
1. Sling Exporter Servlet在HTTP响应中返回[!DNL Sling Model]的JSON再现。

>[!NOTE]
>
>虽然Apache Sling项目提供将[!DNL Sling Models]序列化为JSON的Jackson Exporter，但Exporter框架还支持自定义导出器。 例如，项目可以实现将[!DNL Sling Model]序列化为XML的自定义导出器。

>[!NOTE]
>
>[!DNL Sling Model Exporter] *序列化* [!DNL Sling Models]还可以将它们导出为Java对象。 导出到其他Java对象在HTTP请求流中不起作用，因此不在上图中显示。

## 支持材料

* [ [!DNL Sling Model Exporter] ApacheFramework文档](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
