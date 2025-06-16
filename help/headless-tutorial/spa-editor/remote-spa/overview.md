---
title: SPA 编辑器和远程 SPA 快速入门 - 概述
description: 欢迎来到本系列教程！本教程面向希望使用 AEM SPA 编辑器为现有远程 SPA 添加可编辑 AEM 内容的开发人员。
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7630
thumbnail: 333272.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
doc-type: Tutorial
exl-id: c5f933eb-c409-41dc-bb6a-6b2220dfbb47
duration: 294
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: tm+mt
source-wordcount: '571'
ht-degree: 100%

---

# 概述

{{edge-delivery-services}}

欢迎来到本系列教程！本教程面向希望使用 AEM SPA 编辑器为现有远程 SPA 添加可编辑 AEM 内容的开发人员。

本教程基于 [WKND GraphQL 应用程序](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html?lang=zh-Hans)，这是一个通过 AEM 的 GraphQL API 使用 AEM 内容片段的 React 应用程序，但该应用程序不提供任何 SPA 内容的上下文创作功能。

>[!VIDEO](https://video.tv.adobe.com/v/333272?quality=12&learn=on)

## 关于本教程

本教程旨在说明如何更新远程 SPA（或运行在 AEM 环境之外的 SPA），以便使用和投放在 AEM 中创作的内容。

本教程中的大多数活动都聚焦于 JavaScript 开发，但也涵盖了有关 AEM 的关键方面。这些方面包括定义在 AEM 中创作和存储内容的位置，以及将 SPA 路由映射到 AEM 页面。

本教程将会使用 **AEM as a Cloud Service**，并由两个项目组成：

1. __AEM 项目__&#x200B;包含必须部署到 AEM 的配置和内容。
1. __WKND 应用程序__&#x200B;项目是一个 SPA，它会与 AEM 的 SPA 编辑器集成

## 最新代码

+ 本教程代码的起始点可以在 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/remote-spa-tutorial) 的 `remote-spa-tutorial` 文件夹中找到。

## 先决条件

本教程需要以下内容：

+ [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=zh-Hans)
+ [Node.js v18](https://nodejs.org/en/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all-2.1.0.zip 或更高版本](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql 源代码](https://github.com/adobe/aem-guides-wknd-graphql/tree/main)

本教程假设：

+ 使用 [Microsoft® Visual Studio Code](https://visualstudio.microsoft.com/) 作为 IDE
+ 工作目录为 `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial`
+ 在 `http://localhost:4502` 上以 Author 服务运行 AEM SDK
+ 使用本地 `admin` 账户（密码为 `admin`）运行 AEM SDK
+ 在 `http://localhost:3000` 上运行 SPA

>[!NOTE]
>
> **需要帮助设置本地开发环境吗？**&#x200B;请参阅[以下使用 AEM as a Cloud Service SDK 搭建本地开发环境的指南](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-Hans)。

## &#x200B;1. 为 SPA 编辑器配置 AEM

要将 SPA 与 AEM SPA 编辑器集成，需要 AEM 配置。这些配置通过 AEM 项目进行管理和部署。在本章中，您将了解哪些配置是必需的，以及如何定义它们。

+ [了解如何为 SPA 编辑器配置 AEM。](./aem-configure.md)

## 2 - 引导启动 SPA

为了让 AEM SPA 编辑器将 SPA 集成到其创作环境中，必须对 SPA 进行一些添加。

+ [学习如何为 AEM SPA 编辑器引导启动 SPA](./spa-bootstrap.md)

## 3.可编辑固定组件

首先，探索在 SPA 中添加一个可编辑的“固定组件”。这说明了开发人员如何在 SPA 中放置特定的可编辑组件。虽然作者可以更改组件的内容，但他们不能删除组件或更改其位置、定位或大小。

+ [了解可编辑的固定组件](./spa-fixed-component.md)

## &#x200B;4. 可编辑的容器组件

接下来，探索如何在 SPA 中添加一个可编辑的“容器组件”。这将演示开发人员如何在 SPA 中放置容器组件。容器组件允许作者在其中放置允许的组件，并调整组件的布局。

+ [了解可编辑容器组件](./spa-container-component.md)

## &#x200B;5. 动态路由和可编辑组件

最后，将前几章中解释的概念应用于动态路由；即根据路由参数显示不同内容的路由。这说明了如何使用 AEM SPA 编辑器在以编程方式驱动和派生的路由上创作内容。

+ [了解动态路由和可编辑组件](./spa-dynamic-routes.md)

## 其他资源

+ [AEM SPA React 可编辑组件](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
