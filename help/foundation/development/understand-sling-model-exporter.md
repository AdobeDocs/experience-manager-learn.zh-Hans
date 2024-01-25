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
duration: 150
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '568'
ht-degree: 0%

---

# 了解 [!DNL Sling Model Exporter]

Apache [!DNL Sling Models] 1.3.0引入 [!DNL Sling Model Exporter]，导出或序列化的简单方式 [!DNL Sling Model] 将对象转换为自定义抽象概念。 本文比较了使用的传统用例 [!DNL Sling Models] 要填充HTL脚本，请使用 [!DNL Sling Model Exporter] 序列化框架 [!DNL Sling Model] 转换为JSON。

## 传统Sling模型HTTP请求流

的传统用例 [!DNL Sling Models] 是为资源或请求提供业务抽象，从而提供HTL脚本（或以前的JSP）用于访问业务功能的界面。

常见模式正在形成 [!DNL Sling Models] 表示AEM组件或页面，并使用 [!DNL Sling Model] 对象向HTL脚本提供数据，并在浏览器中显示HTML的结束结果。

### Sling模型HTTP请求流

![Sling模型请求流程](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET] 在AEM中请求资源。

   示例： `HTTP GET /content/my-resource.html`

1. 基于请求资源的 `sling:resourceType`，则会解析相应的脚本。

1. 该脚本可根据需要调整请求或资源 [!DNL Sling Model].

1. 脚本使用 [!DNL Sling Model] 对象以生成HTML演绎版。

1. 脚本生成的HTML将在HTTP响应中返回。

这种传统模式在生成HTML的环境中非常有效，例如 [!DNL Sling Model] 可通过HTL轻松利用。 创建更多结构化数据（如JSON或XML）是一项繁琐得多的工作，因为HTL本身并不自然适合这些格式的定义。

## [!DNL Sling Model Exporter] HTTP请求流程

Apache [!DNL Sling Model Exporter] 随附了Sling提供的杰克逊导出程序，可自动序列化 [!DNL Sling Model] 对象转换为JSON。 Jackson Exporter虽然可配置，但其核心可检查 [!DNL Sling Model] 对象，并使用任何“getter”方法作为JSON键生成JSON，并将getter返回值作为JSON值。

的直接序列化 [!DNL Sling Models] 允许他们使用传统方法创建的HTML响应为普通的Web请求提供服务 [!DNL Sling Model] 请求流（请参阅上文），但同时显示可由Web服务或JavaScript应用程序使用的JSON演绎版。

![Sling模型导出器HTTP请求流](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*此流程描述使用提供的Jackson导出程序生成JSON输出的流程。 使用自定义导出程序遵循相同的流程，但采用其输出格式。*

1. 在AEM中，会对具有选择器和扩展名且已注册到的资源发出HTTPGET请求 [!DNL Sling Model]的导出程序。

   示例： `HTTP GET /content/my-resource.model.json`

1. Sling解析所请求资源的 `sling:resourceType`，选择器以及动态生成的Sling导出器Servlet的扩展，该Servlet映射到 [!DNL Sling Model] 使用导出程序。
1. 解析的Sling导出程序Servlet调用 [!DNL Sling Model Exporter] 针对 [!DNL Sling Model] 从请求或资源改编的对象（由Sling模型自适应表确定）。
1. 导出程序将 [!DNL Sling Model] 基于导出程序选项和特定于导出程序的Sling模型注释，并将结果返回给Sling导出程序Servlet。
1. Sling导出程序Servlet返回 [!DNL Sling Model] 在HTTP响应中。

>[!NOTE]
>
>而Apache Sling项目提供了用于序列化的Jackson导出程序 [!DNL Sling Models] 对于JSON，导出程序框架还支持自定义导出程序。 例如，某个项目可以实施一个自定义导出程序，该程序序列化 [!DNL Sling Model] 转换为XML。

>[!NOTE]
>
>不仅如此 [!DNL Sling Model Exporter] *序列化* [!DNL Sling Models]，则还可以将它们导出为Java对象。 导出到其他Java对象在HTTP请求流中不起作用，因此未出现在上图中。

## 支持材料

* [Apache [!DNL Sling Model Exporter] 框架文档](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
