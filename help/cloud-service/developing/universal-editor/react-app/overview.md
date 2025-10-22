---
title: 使用通用编辑器进行 React 应用程序编辑
description: 了解如何使用通用编辑器编辑示例 React 应用程序的内容。
short-description: 了解如何使用通用编辑器编辑示例 React 应用程序的内容。相关内容存储在 AEM 中的内容片段中，并使用 GraphQL API 进行获取。
version: Experience Manager as a Cloud Service
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
source-git-commit: f0b1b906e1ef04b53eca940f191e65d62a2e0bab
workflow-type: tm+mt
source-wordcount: '444'
ht-degree: 100%

---

# 使用通用编辑器进行 React 应用程序编辑

了解如何使用通用编辑器编辑示例 React 应用程序的内容。相关内容存储在 AEM 中的内容片段中，并使用 GraphQL API 进行获取。

本教程将引导你完成以下操作：搭建本地开发环境、对 React 应用程序进行内容编辑适配，并通过通用编辑器编辑内容。

## 你将学到的内容

本教程涵盖以下主题：

- 通用编辑器简介
- 设置本地开发环境
   - **AEM SDK**：它使用 GraphQL API 为 React 应用程序提供内容片段中存储的内容。
   - **React 应用程序**：一个简单的用户界面，用于展示来自 AEM 的内容。
   - **通用编辑器服务**：_通用编辑器服务的一个本地副本_，用于将通用编辑器与 AEM SDK 进行绑定。
- 如何对 React 应用进行适配，以使用通用编辑器编辑内容
- 了解如何使用通用编辑器编辑 React 应用程序的内容


## 通用编辑器概述

通用编辑器为内容作者和开发人员（前端和后端）提供支持，下面让我们来回顾一下通用编辑器的一些主要优势：

- 旨在以所见即所得 (WYSIWYG) 模式编辑 Headless 和 Headful 内容。
- 提供跨不同前端技术（如 React、Angular、Vue 等）的一致内容编辑体验。因此，内容作者可以编辑内容，而不必担心底层前端技术。
- 在前端应用中启用通用编辑器只需极少量的适配工作。从而最大限度地提高开发人员的工作效率，让他们能够专注于构建体验。
- 实现内容创作者、前端开发人员和后端开发人员三者职责的明确分离，使每个角色都能专注于自身核心任务。


## 示例 React 应用程序

本教程使用 [**WKND Teams**](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial#react-app---basic-tutorial---teampersons) 作为示例 React 应用程序。**WKND Teams** React 应用程序显示团队成员及其详细信息列表。

团队详细信息（例如标题、描述和团队成员）在 AEM 中存储为&#x200B;_团队_&#x200B;内容片段。同样，姓名、个人简介和个人资料图片等人员详细信息在 AEM 中存储为&#x200B;_人员_&#x200B;内容片段。

React 应用程序的内容由 AEM 通过 GraphQL API 提供，用户界面则由两个 React 组件 `Teams` 和 `Person` 构建。

我们提供相应的教程，以帮助您学习如何构建 **WKND Teams** React 应用程序。您可以在[此处](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview)找到该教程。

## 后续步骤

了解如何[设置本地开发环境](./local-development-setup.md)。
