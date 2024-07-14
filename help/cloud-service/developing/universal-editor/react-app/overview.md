---
title: 使用通用编辑器编辑React应用程序
description: 了解如何使用通用编辑器编辑示例React应用程序的内容。
version: Cloud Service
feature: Developer Tools, Headless
topic: Development, Content Management
role: Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 87
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15359
thumbnail: KT-15359.png
exl-id: 69ed610e-2eff-43b3-98f9-3dc40594e879
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 0%

---

# 使用通用编辑器编辑React应用程序

了解如何使用通用编辑器编辑示例React应用程序的内容。 这些内容存储在AEM的内容片段中，并使用GraphQL API获取。

本教程将指导您完成设置本地开发环境、检测React应用程序以编辑其内容以及使用通用编辑器编辑内容的过程。

## 您学到的内容

本教程涵盖以下主题：

- Universal Editor的简要概述
- 设置本地开发环境
   - **AEM SDK**：它使用GraphQL API为React应用程序提供内容片段中存储的内容。
   - **React应用程序**：显示AEM内容的简单用户界面。
   - **通用编辑器服务**：绑定通用编辑器和AEM SDK的通用编辑器服务&#x200B;_的_&#x200B;本地副本。
- 如何使用通用编辑器检测React应用程序以编辑内容
- 如何使用通用编辑器编辑React应用程序内容


## 通用编辑器概述

通用编辑器为内容作者和开发人员（前端和后端）提供支持，让我们回顾一下通用编辑器的一些主要优势：

- 构建用于在“所见即所得”(WYSIWYG)模式下编辑Headless和Headful内容。
- 跨不同的前端技术(如React、Angular、Vue等)提供一致的内容编辑体验。 因此，内容作者可以编辑内容，而无需担心底层前端技术。
- 在前端应用程序中启用通用编辑器需要非常少的工具。 因此，可以最大限度地提高开发人员的生产效率，并让他们能够专注于构建体验。
- 将关注分散到三个角色：内容作者、前端开发人员和后端开发人员，从而使每个角色都可以专注于其核心职责。


## 示例React应用程序

本教程使用&#x200B;[**WKND Teams**](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial#react-app---basic-tutorial---teampersons)作为示例React应用程序。 **WKND Teams** React应用程序显示团队成员及其详细信息列表。

标题、描述和团队成员等团队详细信息在AEM中存储为&#x200B;_团队_&#x200B;内容片段。 同样，人员详细信息（如姓名、个人简历和个人资料图片）在AEM中存储为&#x200B;_人员_&#x200B;内容片段。

React应用程序的内容由AEM使用GraphQL API提供，用户界面使用两个React组件构建： `Teams`和`Person`。

提供了相应的教程，以了解如何构建&#x200B;**WKND Teams** React应用程序。 您可以在[此处](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview)找到该教程。

## 后续步骤

了解如何[设置本地开发环境](./local-development-setup.md)。
