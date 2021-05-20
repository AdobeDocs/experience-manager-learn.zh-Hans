---
title: SPA Editor和远程SPA快速入门 — 概述
description: 欢迎参加面向希望使用AEM SPA编辑器将现有SPA与可编辑AEM内容相补充的开发人员的多部分教程。
topic: 无头、SPA、开发
feature: SPA编辑器，核心组件， API，开发
role: Developer, Architect
level: Beginner
kt: 7630
thumbnail: 333272.jpeg
source-git-commit: cede0c97e0f322fe5d20d5c4f685ed10b90af1d4
workflow-type: tm+mt
source-wordcount: '694'
ht-degree: 1%

---


# 概述

欢迎参加多部分教程，面向希望使用AEM SPA编辑器将现有的基于React的（或Next.js）远程SPA内容与可编辑的AEM内容进行扩充的开发人员。

本教程以[WKND GraphQL应用程序](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html)为基础，该应用程序是一个React应用程序，用于通过AEM GraphQL API使用AEM内容片段内容，但不提供SPA内容的任何上下文内部创作。

>[!VIDEO](https://video.tv.adobe.com/v/333272/?quality=12&learn=on)

## 关于教程

本教程旨在说明如何更新远程SPA或在AEM上下文外运行的SPA，以使用和交付在AEM中创作的内容。

本教程中的大多数活动重点介绍JavaScript开发，但涉及的关键方面涉及AEM。 这些方面包括定义在AEM中创作和存储内容的位置，以及将SPA路由映射到AEM页面。

本教程旨在将&#x200B;**AEM作为Cloud Service**&#x200B;使用，并且包含两个项目：

1. __AEM Project__&#x200B;包含必须部署到AEM的配置和内容。
1. __WKND__ Appproject是要与AEM SPA编辑器集成的SPA

## 最新代码

+ 可以在`feature/spa-editor`分支的[GitHub](https://github.com/adobe/aem-guides-wknd-graphql)上找到本教程的代码。

## 前提条件

本教程需要满足以下条件：

+ [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v14+](https://nodejs.org/en/)
+ [npm v7+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all.0.3.0.zip或更高版本](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql源代码](https://github.com/adobe/aem-guides-wknd-graphql)

本教程假定：

+ [Microsoft® Visual Studio代](https://visualstudio.microsoft.com/) 码作为IDE
+ `~/Code/wknd-app`的工作目录
+ 在`http://localhost:4502`上作为创作服务运行AEM SDK
+ 使用本地`admin`帐户运行密码为`admin`的AEM SDK
+ 在`http://localhost:3000`上运行SPA

>[!NOTE]
>
> **需要有关设置本地开发环境的帮助？** 请参阅以 [下指南，以使用AEM as a Cloud ServiceSDK设置本地开发环境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。


## 快速设置

快速设置可在15分钟内使用WKND应用程序SPA和AEM SPA编辑器启动并运行。 此加速设置将直接转到教程的结束状态，从而允许您探索在AEM SPA编辑器中创作SPA。

+ [了解快速设置](./quick-setup.md)

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

+ [在AEM文档中编辑外部SPA](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/hybrid/editing-external-spa.html)
+ [AEM WCM组件 — React Core实施](https://www.npmjs.com/package/@adobe/aem-core-components-react-base)
+ [AEM WCM组件 — Spa编辑器 — React Core实施](https://www.npmjs.com/package/@adobe/aem-core-components-react-spa)
