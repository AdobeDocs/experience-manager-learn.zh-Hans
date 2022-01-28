---
title: AEM无头的高级概念 — GraphQL
description: 一个端到端教程，其中演示了Adobe Experience Manager(AEM)GraphQL API的高级概念。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
source-git-commit: 83e16ea87847182139982ea2378d8ff9f079c968
workflow-type: tm+mt
source-wordcount: '926'
ht-degree: 0%

---

# AEM Headless的高级概念

本端到端教程将继续 [基本教程](../multi-step/overview.md) 涵盖Adobe Experience Manager(AEM)无头和GraphQL的基础知识。 该高级教程深入说明了如何使用内容片段模型、内容片段和AEM GraphQL API，包括在客户端应用程序中使用AEM GraphQL。

## 前提条件

完成 [快速设置AEMas a Cloud Service](../quick-setup/cloud-service.md) 来配置环境。

强烈建议您完成上一个 [基本教程](../multi-step/overview.md) 和 [视频系列](../video-series/modeling-basics.md) 教程，然后再继续使用此高级教程。 尽管您可以使用本地AEM环境完成教程，但本教程仅涵盖AEMas a Cloud Service的工作流程。

## 目标

本教程涵盖以下主题：

* 使用验证规则和更高级的数据类型（如选项卡占位符、嵌套片段引用、JSON对象以及日期和时间数据类型）创建内容片段模型。
* 使用嵌套内容和片段引用时创作内容片段，并为内容片段创作管理配置文件夹策略。
* 使用带有变量和指令的GraphQL查询来浏览AEM GraphQL API功能。
* 在AEM中使用参数保留GraphQL查询，并了解如何对保留的查询使用缓存控制参数。
* 使用AEM Headless JavaScript SDK将持久查询请求集成到示例WKND GraphQL React应用程序中。

## AEM Headless的高级概念概述

以下视频简要介绍本教程中涵盖的概念。 本教程包括使用更高级的数据类型定义内容片段模型、嵌套内容片段，以及在AEM中保留GraphQL查询。

>[!VIDEO](https://video.tv.adobe.com/v/340035/?quality=12&learn=on)

## 项目设置

WKND Site项目具有所有必需的配置，因此您可以在完成 [快速设置](../quick-setup/cloud-service.md). 本节仅重点介绍在创建您自己的AEM Headless项目时可以使用的一些重要步骤。

### 创建配置

在AEM中启动任何新项目的第一步是创建其配置（作为工作区）和创建GraphQL API端点。 要查看或创建配置，请导航至 **工具** > **常规** > **配置浏览器**.

![导航到配置浏览器](assets/overview/create-configuration.png)

请注意，已为教程创建WKND站点配置。 要为您自己的项目创建配置，请选择 **创建** 填写右上角的“创建配置”模式中显示的表单。

### 创建GraphQL API端点

接下来，必须配置API端点才能将GraphQL查询发送到。 要查看现有端点或创建一个端点，请导航到 **工具** > **资产** > **GraphQL**.

![配置端点](assets/overview/endpoints.png)

请注意，已创建全局端点和WKND端点。 要为项目创建端点，请选择 **创建** 中，然后按照工作流进行操作。

>[!NOTE]
>
> 保存端点后，您将看到有关访问安全控制台的模式窗口，如果要配置对端点的访问，可通过该模式窗口调整安全设置。 但是，安全权限本身不在本教程的涵盖范围内。 有关更多信息，请参阅 [AEM文档](https://experienceleague.adobe.com/docs/experience-manager-64/administering/security/security.html).

### 为项目创建语言根文件夹

语言根文件夹是使用ISO语言代码作为其名称（如EN或FR）的文件夹。 AEM翻译管理系统使用这些文件夹来定义内容的主要语言和内容翻译的语言。

转到 **导航** > **资产** > **文件**.

![导航到文件](assets/overview/files.png)

导航到 **WKND站点** 文件夹。 观察标题为“English”且名称为“EN”的文件夹。 此文件夹是WKND站点项目的语言根文件夹。

![英文文件夹](assets/overview/english.png)

对于您自己的项目，在配置中创建语言根文件夹。 请参阅 [创建文件夹](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md#create-folders) 以了解更多详细信息。

### 为嵌套文件夹分配配置

最后，必须将项目的配置分配给语言根文件夹。 此分配允许根据项目配置中定义的内容片段模型创建内容片段。

要将语言根文件夹分配给配置，请选择该文件夹，然后选择 **属性** 中。

![选择属性](assets/overview/properties.png)

接下来，导航到 **Cloud Services** 选项卡上，选择 **云配置** 字段。

![云配置](assets/overview/cloud-conf.png)

在显示的模式中，选择您之前创建的配置以将语言根文件夹分配给该配置。

### 最佳实践

在AEM中创建您自己的项目时，最佳做法如下：

* 文件夹层次结构应考虑本地化和翻译。 换言之，语言文件夹应嵌套在配置文件夹中，以便轻松翻译这些配置文件夹中的内容。
* 文件夹层次结构应保持平直和直观。 请避免稍后移动或重命名文件夹和片段，尤其是在发布以用于实时用途后，因为它会更改可能影响片段引用和GraphQL查询的路径。

## 入门和解决方案包

两个AEM **软件包** 可用，并可通过 [包管理器](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md#sample-content)

* [Advanced-GraphQL-Tutorial-Starter-Package-1.0.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Starter-Package-1.0.zip) 将在教程的后面部分使用，并包含示例图像和文件夹。
* [Advanced-GraphQL-Tutorial-Solution-Package-1.1.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.1.zip) 包含第1-4章的已完成解决方案，其中包括新的内容片段模型、内容片段和持久化图形QL查询。 对于要直接跳入 [客户端应用程序集成](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md) 章节。

有两个React JS项目可用于试用查询 [从无头客户端应用程序](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md).

* [aem-guides-wknd-headless-start-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-start-tutorial.zip)  — 在中完成的启动客户端应用程序 [第5章 — 客户端应用程序集成](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md).
* [aem-guides-wknd-headless-solution-tutorial-zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip)  — 已完成的客户端应用程序，使用 **持久保留** 查询。


## 入门

要开始使用此高级教程，请执行以下步骤：

1. 使用 [AEMas a Cloud Service](../quick-setup/cloud-service.md).
1. 将教程章节开始于 [创建内容片段模型](/help/headless-tutorial/graphql/advanced-graphql/create-content-fragment-models.md).
