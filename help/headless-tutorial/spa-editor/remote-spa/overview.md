---
title: SPA Editor和远程SPA快速入门 — 概述
description: 欢迎参加面向希望使用AEM SPA编辑器将现有SPA与可编辑AEM内容相补充的开发人员的多部分教程。
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7630
thumbnail: 333272.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
exl-id: c5f933eb-c409-41dc-bb6a-6b2220dfbb47
source-git-commit: 0fff8b53e3dffb835e070444b55a72f0b0cc3d14
workflow-type: tm+mt
source-wordcount: '614'
ht-degree: 4%

---

# 概述

欢迎参加多部分教程，面向希望使用AEM SPA编辑器将现有的基于React的（或Next.js）远程SPA内容与可编辑的AEM内容进行扩充的开发人员。

本教程以 [WKND GraphQL应用程序](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html)，一种在AEM GraphQL API上使用AEM内容片段内容的React应用程序，但不提供SPA内容的任何上下文内创作。

>[!VIDEO](https://video.tv.adobe.com/v/333272?quality=12&learn=on)

## 关于教程

本教程旨在说明如何更新远程SPA或在AEM上下文外运行的SPA，以使用和交付在AEM中创作的内容。

本教程中的大多数活动重点介绍JavaScript开发，但涉及的关键方面涉及AEM。 这些方面包括定义在AEM中创作和存储内容的位置，以及将SPA路由映射到AEM页面。

本教程旨在与 **AEMas a Cloud Service** 由两个项目组成：

1. 的 __AEM项目__ 包含必须部署到AEM的配置和内容。
1. __WKND应用程序__ 项目是要与AEM SPA Editor集成的SPA

## 最新代码

+ 本教程代码的起点可在 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/remote-spa-tutorial) 在 `remote-spa-tutorial` 文件夹。

## 前提条件

本教程需要满足以下条件：

+ [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v18](https://nodejs.org/en/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all-2.1.0.zip或更高版本](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql源代码](https://github.com/adobe/aem-guides-wknd-graphql/tree/main)

本教程假定：

+ [Microsoft® Visual Studio代码](https://visualstudio.microsoft.com/) 作为IDE
+ 工作目录 `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial`
+ 在上运行AEM SDK作为创作服务 `http://localhost:4502`
+ 使用本地运行AEM SDK `admin` 密码帐户 `admin`
+ 在上运行SPA `http://localhost:3000`

>[!NOTE]
>
> **需要有关设置本地开发环境的帮助？** 查看 [以下使用AEMas a Cloud Service SDK设置本地开发环境的指南](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-Hans).

## 1.为AEM编辑器配置SPA

需要AEM配置才能将SPA与AEM SPA编辑器集成。 这些配置通过AEM项目进行管理和部署。 在本章中，了解需要哪些配置以及如何定义这些配置。

+ [了解如何配置AEM for SPA Editor](./aem-configure.md)

## 2.BootstrapSPA

AEM SPA Editor要将SPA集成到其创作上下文中，必须在SPA中添加一些内容。

+ [了解如何引导SPA for AEM SPA Editor](./spa-bootstrap.md)

## 3.可编辑的固定组件

首先，探索向SPA中添加可编辑的“固定组件”。 这说明了开发人员如何在SPA中放置特定的可编辑组件。 虽然作者可以更改组件的内容，但无法删除组件或更改其位置、位置或大小。

+ [了解可编辑的固定组件](./spa-fixed-component.md)

## 4.可编辑的容器组件

接下来，探索向SPA中添加一个可编辑的“容器组件”。 这说明了开发人员如何将容器组件放入SPA。 容器组件允许作者将允许的组件放置在其中，并调整组件的布局。

+ [了解可编辑的容器组件](./spa-container-component.md)

## 5.动态路由和可编辑的组件

最后，将前几章中阐述的概念用于动态路由；根据路由参数显示不同内容的路由。 这说明了如何使用AEM SPA Editor在以编程方式驱动和派生的路由上创作内容。

+ [了解动态路由和可编辑的组件](./spa-dynamic-routes.md)

## 其他资源

+ [AEM SPA React可编辑的组件](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
