---
title: 了解AEM中的Sling模型导出程序
description: Apache Sling模型1.3.0引入了Sling模型导出器，这是一种将Sling模型对象导出或序列化为自定义抽象的优雅方法。 本文并列了使用Sling模型填充HTL脚本的传统用例，以及利用Sling模型导出器框架将Sling模型序列化为JSON。
version: 6.3, 6.4, 6.5
sub-product: 基础，内容服务
feature: API
topics: development, content-delivery, headless
activity: understand
audience: developer, architect
doc-type: article
topic: 开发
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '573'
ht-degree: 0%

---


# 了解[!DNL Sling Model Exporter]

Apache [!DNL Sling Models] 1.3.0引入了[!DNL Sling Model Exporter]，这是一种将[!DNL Sling Model]对象导出或序列化为自定义抽象的优雅方法。 本文并列了使用[!DNL Sling Models]填充HTL脚本的传统用例，以及利用[!DNL Sling Model Exporter]框架将[!DNL Sling Model]序列化为JSON。

## 传统Sling模型HTTP请求流程

[!DNL Sling Models]的传统用例是为资源或请求提供业务抽象，为HTL脚本（或以前的JSP）提供访问业务函数的接口。

常见的模式是开发表示AEM组件或页面的[!DNL Sling Models]，并使用[!DNL Sling Model]对象向HTL脚本提供数据，最终结果为浏览器中显示的HTML。

### Sling模型HTTP请求流程

![Sling模型请求流](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET] 在AEM中请求资源。

   示例: `HTTP GET /content/my-resource.html`

1. 根据请求资源的`sling:resourceType`，将解析相应的脚本。

1. 脚本会将请求或资源调整为所需的[!DNL Sling Model]。

1. 脚本使用[!DNL Sling Model]对象生成HTML呈现版本。

1. 脚本生成的HTML将在HTTP响应中返回。

此传统模式在生成HTML时非常有效，因为通过HTL可以轻松利用[!DNL Sling Model]。 创建更结构化的数据（如JSON或XML）是一项更为繁琐的工作，因为HTL并不自然地适合这些格式的定义。

## [!DNL Sling Model Exporter] HTTP请求流程

Apache [!DNL Sling Model Exporter]附带Sling提供的Jackson导出程序，该导出程序会自动将“ordinal” [!DNL Sling Model]对象序列化为JSON。 虽然Jackson导出程序可进行相当的配置，但其核心会检查[!DNL Sling Model]对象，并使用任何“getter”方法作为JSON键生成JSON，而getter返回值作为JSON值生成。

通过[!DNL Sling Models]的直接序列化，用户可以使用使用传统的[!DNL Sling Model]请求流创建的HTML响应来为普通Web请求提供服务（请参阅上文），同时还可以显示Web服务或JavaScript应用程序可使用的JSON呈现版本。

![Sling模型导出程序HTTP请求流程](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*此流描述使用提供的Jackson导出程序生成JSON输出的流。自定义导出程序的使用遵循相同的流程，但其输出格式。*

1. 对AEM中的资源发出HTTPGET请求，该资源的选择器和扩展已在[!DNL Sling Model]的导出程序中注册。

   示例: `HTTP GET /content/my-resource.model.json`

1. Sling将所请求资源的`sling:resourceType`选择器和扩展解析为一个动态生成的Sling导出程序Servlet，该Servlet已映射到带有导出程序的[!DNL Sling Model]。
1. 解析的Sling导出程序Servlet针对从请求或资源适应的[!DNL Sling Model]对象（由Sling模型自适应表确定）调用[!DNL Sling Model Exporter]。
1. 导出程序根据导出程序选项和导出程序特定的Sling模型批注序列化[!DNL Sling Model]，并将结果返回给Sling导出程序Servlet。
1. Sling导出程序Servlet在HTTP响应中返回[!DNL Sling Model]的JSON呈现版本。

>[!NOTE]
>
>虽然Apache Sling项目提供将[!DNL Sling Models]序列化为JSON的Jackson导出程序，但导出程序框架也支持自定义导出程序。 例如，项目可以实施将[!DNL Sling Model]序列化为XML的自定义导出程序。

>[!NOTE]
>
>[!DNL Sling Model Exporter] *序列化* [!DNL Sling Models]不仅如此，还可以将它们导出为Java对象。 导出到其他Java对象在HTTP请求流中不起作用，因此不会显示在上图中。

## 辅助材料

* [ [!DNL Sling Model Exporter] ApacheFramework文档](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
