---
title: SPA编辑器和远程SPA快速入门 — 概述
description: 欢迎阅读多部分教程，该教程面向那些希望使用AEM SPA编辑器用可编辑的SPA内容充实现有远程AEM的开发人员。
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7630
thumbnail: 333272.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
doc-type: Tutorial
exl-id: c5f933eb-c409-41dc-bb6a-6b2220dfbb47
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '614'
ht-degree: 4%

---

# 概述

{{edge-delivery-services}}

对于希望使用AEM SPA编辑器将现有基于React（或Next.js）的SPA内容扩充为可编辑AEM内容的开发人员，欢迎使用多部分教程。

本教程基于 [WKND GraphQL应用程序](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html)，这是一个React应用程序，它通过AEM GraphQL API使用AEM内容片段内容，但不提供任何SPA内容的上下文创作。

>[!VIDEO](https://video.tv.adobe.com/v/333272?quality=12&learn=on)

## 关于教程

本教程旨在说明如何更新远程SPA或在AEM上下文之外运行的SPA，以便使用和交付在AEM中创作的内容。

本教程中的大多数活动都重点介绍JavaScript开发，但其中涵盖了围绕AEM的关键方面。 这些方面包括定义在AEM中创作和存储内容的位置，并将SPA路由映射到AEM Pages。

本教程专门设计用于 **AEMas a Cloud Service** 并且由两个项目组成：

1. 此 __AEM项目__ 包含必须部署到AEM的配置和内容。
1. __WKND应用程序__ 项目是要与SPA SPA编辑器集成的AEM

## 最新代码

+ 本教程代码的起始点可在上找到 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/remote-spa-tutorial) 在 `remote-spa-tutorial` 文件夹。

## 前提条件

本教程要求您满足以下条件：

+ [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v18](https://nodejs.org/en/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all-2.1.0.zip或更高版本](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql源代码](https://github.com/adobe/aem-guides-wknd-graphql/tree/main)

本教程假定：

+ [Microsoft® Visual Studio代码](https://visualstudio.microsoft.com/) 作为IDE
+ 的工作目录 `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial`
+ 在上将AEM SDK作为创作服务运行 `http://localhost:4502`
+ 使用本地运行AEM SDK `admin` 具有密码的帐户 `admin`
+ 运行SPA `http://localhost:3000`

>[!NOTE]
>
> **在设置本地开发环境时需要帮助？** 查看 [以下指南介绍了如何使用AEMas a Cloud ServiceSDK设置本地开发环境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-Hans).

## 1.为SPA编辑器配置AEM

需要使用AEM配置将SPA与AEM SPA编辑器集成。 这些配置通过AEM项目进行管理和部署。 在本章中，了解所需的配置以及如何定义它们。

+ [了解如何为SPA编辑器配置AEM](./aem-configure.md)

## 2.BootstrapSPA

要使AEM SPA Editor将SPA集成到其创作上下文中，必须对SPA添加一些内容。

+ [了解如何引导SPA for AEM SPA编辑器](./spa-bootstrap.md)

## 3.可编辑的固定组件

首先，浏览向SPA添加可编辑的“固定组件”。 这说明了开发人员如何在SPA中放置特定的可编辑组件。 虽然作者可以更改组件的内容，但他们不能移除组件或更改其放置、位置或大小。

+ [了解可编辑的固定组件](./spa-fixed-component.md)

## 4.可编辑的容器组件

接下来，探索如何向SPA添加可编辑的“容器组件”。 这说明了开发人员如何在SPA中放置容器组件。 容器组件允许作者将允许的组件放入容器组件中，并调整组件的布局。

+ [了解可编辑的容器组件](./spa-container-component.md)

## 5.动态路由和可编辑组件

最后，将前面几章中介绍的概念用于动态路由；根据路由的参数显示不同内容的路由。 这说明了如何使用AEM SPA Editor创作以编程方式驱动和派生的路由的内容。

+ [了解动态路由和可编辑组件](./spa-dynamic-routes.md)

## 其他资源

+ [AEM SPA React可编辑组件](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
