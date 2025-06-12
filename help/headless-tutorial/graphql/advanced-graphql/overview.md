---
title: AEM Headless 的高级概念 - GraphQL
description: 端到端教程，展示 Adobe Experience Manager (AEM) GraphQL API 的高级概念。
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: daae6145-5267-4958-9abe-f6b7f469f803
duration: 441
source-git-commit: bd0f42fa37b7bbe19bf0d7fc65801198e64cbcd9
workflow-type: ht
source-wordcount: '1052'
ht-degree: 100%

---

# AEM Headless 的高级概念

本端到端教程是[基础教程](../multi-step/overview.md)的延续，后者介绍了 Adobe Experience Manager (AEM) Headless 和 GraphQL 的基础知识。本高级教程深入阐述了使用内容片段模型、内容片段以及 AEM GraphQL 持久化查询的各个方面，包括如何在客户端应用程序中使用 GraphQL 持久化查询。

## 先决条件

完成 [AEM as a Cloud Service 的快速设置](../quick-setup/cloud-service.md)，以配置您的 AEM as a Cloud Service 环境。

强烈建议您在继续学习本高级教程之前，先完成之前的[基础教程](../multi-step/overview.md)和[视频系列](../video-series/modeling-basics.md)教程。虽然您可以使用本地 AEM 环境完成本教程，但本教程仅涵盖 AEM as a Cloud Service 的工作流程。

>[!CAUTION]
>
>如果您无法访问 AEM as a Cloud Service 环境，您可以[使用本地 SDK 完成 AEM Headless 的快速设置](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/quick-setup/local-sdk.html?lang=zh-Hans)。然而，值得注意的是，某些产品的 UI 页面（如内容片段导航）与此不同。



## 目标

本教程涵盖以下主题：

* 使用验证规则和更高级的数据类型（如选项卡占位符、嵌套片段引用、JSON 对象以及日期和时间数据类型）来创建内容片段模型。
* 在处理嵌套内容和片段引用时创作内容片段，并配置文件夹策略以实现内容片段创作的治理
* 使用带有变量和指令的 GraphQL 查询来探索 AEM GraphQL API 的功能。
* 在 AEM 中持久化带参数的 GraphQL 查询，并学习如何在持久化查询中使用 cache-control 参数。
* 使用 AEM Headless JavaScript SDK，将对持久化查询的请求集成到示例 WKND GraphQL React 应用程序中。

## AEM Headless 高级概念概述

以下视频概述了本教程涵盖的主要概念。本教程包括使用更高级的数据类型定义内容片段模型、嵌套内容片段，以及在 AEM 中持久化 GraphQL 查询。

>[!VIDEO](https://video.tv.adobe.com/v/340035?quality=12&learn=on)

>[!CAUTION]
>
>本视频（在 2:25 处）提到通过 Package Manager 安装 GraphiQL 查询编辑器，以便探索 GraphQL 查询。然而，在较新的 AEM as a Cloud Service 版本中，内置了 **GraphiQL Explorer**，因此无需单独安装软件包。请参阅[使用 GraphiQL IDE](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/graphiql-ide.html?lang=zh-Hans) 以了解更多信息。


## 项目设置

WKND Site 项目包含所有必要的配置，因此您可以在完成[快速设置](../quick-setup/cloud-service.md)后立即开始本教程。本节仅重点介绍创建您自己的 AEM Headless 项目时可参考的一些重要步骤。


### 审查现有配置

在 AEM 中启动任何新项目的第一步是创建其配置，作为工作区并创建 GraphQL API 端点。要查看或创建配置，请导航至&#x200B;**工具** > **常规** > **配置浏览器**。

![导航至配置浏览器](assets/overview/create-configuration.png)

请注意，本教程已创建了 `WKND Shared` 网站配置。要为您自己的项目创建配置，请点击右上角的&#x200B;**创建**，并在随后出现的“创建配置”模态对话框中填写表单。

![查看 WKND 共享配置](assets/overview/review-wknd-shared-configuration.png)

### 查看 GraphQL API 端点

接下来，您必须配置 API 端点以发送 GraphQL 查询。要查看现有端点或创建一个端点，请导航至&#x200B;**工具** > **常规** > **GraphQL**。

![配置端点](assets/overview/endpoints.png)

请注意，`WKND Shared Endpoint` 已经创建。要为您的项目创建端点，请在右上角选择&#x200B;**创建**，并按照工作流程进行操作。

![审查 WKND 共享端点](assets/overview/review-wknd-shared-endpoint.png)

>[!NOTE]
>
> 保存端点后，您将看到一个关于访问安全控制台的模态对话框，如果您想配置对端点的访问权限，可以通过该对话框调整安全设置。然而，安全权限本身并不在本教程的讨论范围内。有关更多信息，请参阅 [AEM 文档](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html?lang=zh-Hans)。

### 查看 WKND 内容结构和语言根文件夹

明确的内容结构是AEM Headless 实施成功的关键。它有助于提高内容的可扩展性、可用性和权限管理。

语言根文件夹是指以 ISO 语言代码（如 EN 或 FR）作为名称的文件夹。AEM 翻译管理系统利用这些文件夹来定义内容的主要语言以及用于内容翻译的语言。

前往&#x200B;**导航** > **资源** > **文件**。

![导航到“文件”](assets/overview/files.png)

导航到 **WKND 共享**&#x200B;文件夹。注意标题为“English”且名称为“EN”的文件夹。此文件夹是 WKND Site 项目的语言根文件夹。

![英文文件夹](assets/overview/english.png)

对于您自己的项目，请在配置中创建一个语言根文件夹。有关更多详细信息，请参阅[创建文件夹](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md#create-folders)部分。

### 为嵌套文件夹分配配置

最后，您必须将项目的配置分配到语言根文件夹。此分配允许根据项目配置中定义的内容片段模型来创建内容片段。

要将语言根文件夹分配给配置，请选择该文件夹，然后在顶部导航栏中选择&#x200B;**属性**。

![选择属性](assets/overview/properties.png)

接下来，导航至&#x200B;**云服务**&#x200B;选项卡，并在&#x200B;**云配置**&#x200B;字段中选择文件夹图标。

![云配置](assets/overview/cloud-conf.png)

在出现的模态对话框中，选择您之前创建的配置，以将语言根文件夹分配给它。

### 最佳实践

在 AEM 中创建自己的项目时，以下是一些最佳实践：

* 在构建文件夹层次结构时，应充分考虑本地化和翻译因素。换句话说，语言文件夹应嵌套在配置文件夹中，这样便于翻译这些配置文件夹中的内容。
* 文件夹层次结构应保持扁平且直观。请避免在后续操作中移动或重命名文件夹和片段，尤其是在发布供实时使用之后，因为这会改变路径，从而可能影响片段引用和 GraphQL 查询。

## 入门包和解决方案包

有两个 AEM **包**&#x200B;可用，可通过[包管理器](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md#sample-content)进行安装

* [Advanced-GraphQL-Tutorial-Starter-Package-1.1.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Starter-Package-1.1.zip) 在教程的后续部分中使用，其中包含示例图像和文件夹。
* [高级 GraphQL 教程解决方案包 1.2.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip) 包含第 1 至 4 章的完整解决方案，包括新的内容片段模型、内容片段和持久化 GraphQL 查询。对于那些想直接跳到[客户端应用程序集成](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md)章节的人员来说，这将非常有用。


[React 应用程序 - 高级教程 - WKND Adventures](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/advanced-tutorial/README.md) 项目可供查看和探索示例应用程序。此示例应用程序通过调用持久化的 GraphQL 查询从 AEM 检索内容，并以沉浸式体验呈现。

## 快速入门

要开始学习本高级教程，请按照以下步骤进行：

1. 使用 [AEM as a Cloud Service](../quick-setup/cloud-service.md) 来搭建开发环境。
1. 请从[创建内容片段模型](/help/headless-tutorial/graphql/advanced-graphql/create-content-fragment-models.md)的教程章节开始。
