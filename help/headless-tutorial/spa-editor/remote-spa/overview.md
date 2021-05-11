---
title: SPA Editor和远程SPA入门 — 概述
description: 欢迎观看面向希望使用AEM SPA Editor通过可编辑的AEM内容来扩充现有远程SPA的开发人员的多部分教程。
topic: 无外设、SPA、开发
feature: SPA编辑器，核心组件， API，开发
role: Developer, Architect
level: Beginner
kt: 7630
thumbnail: kt-7630.jpg
translation-type: tm+mt
source-git-commit: ec692af56afa83330097bb9d8db0d2f2f4fde1c1
workflow-type: tm+mt
source-wordcount: '694'
ht-degree: 1%

---


# 概述

欢迎观看面向希望使用SPA AEM Editor将现有基于React（或Next.js）的远程SPA内容与可编辑的AEM内容相结合的开发人员的多部分教程。

本教程构建于[WKND GraphQL App](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html)之上，后者是一个React应用程序，通过AEM GraphQL API读取AEM内容片段内容，但不提供SPA内容的任何上下文内容创作。

## 关于教程

本教程旨在说明如何更新远程SPA或在AEM上下文外运行的SPA以使用和交付在AEM中创作的内容。

教程中的大多数活动侧重于JavaScript开发，但围绕AEM讨论的关键方面。 这些方面包括定义在AEM中创作和存储内容的位置，以及将SPA路由映射到AEM页面。

本教程旨在将&#x200B;**AEM用作Cloud Service**，由两个项目组成：

1. __AEM Project__&#x200B;包含必须部署到AEM的配置和内容。
1. __WKND__ Appproject是要与AEM SPA Editor集成的SPA

## 最新代码

+ 本教程的代码位于`feature/spa-editor`分支的[GitHub](https://github.com/adobe/aem-guides-wknd-graphql)上。

## 前提条件

本教程要求：

+ [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v14+](https://nodejs.org/en/)
+ [npm v7+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all.0.3.0.zip或更高版本](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql源代码](https://github.com/adobe/aem-guides-wknd-graphql)

本教程假定：

+ [Microsoft® Visual Studio代](https://visualstudio.microsoft.com/) 码(IDE)
+ `~/Code/wknd-app`的工作目录
+ 在`http://localhost:4502`上作为作者服务运行AEM SDK
+ 使用密码为`admin`的本地`admin`帐户运行AEM SDK
+ 在`http://localhost:3000`上运行SPA

>[!NOTE]
>
> **需要设置本地开发环境的帮助？** 请参阅以 [下指南，使用AEM作为Cloud Service SDK设置本地开发环境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。


## 快速设置

“快速设置”可在15分钟内使用WKND应用程序SPA和AEM SPA编辑器启动并运行。 此加速设置将直接带您进入教程的结束状态，让您了解如何在AEM SPA Editor中创作SPA。

+ [了解快速设置](./quick-setup.md)

## 1.配置AEM for SPA Editor

AEM配置是将SPA与AEM SPA Editor集成所必需的。 通过AEM项目管理和部署这些配置。 在本章中，了解哪些配置是必需的以及如何定义它们。

+ [了解如何为SPA Editor配置AEM](./aem-configure.md)

## 2.BootstrapSPA

要使AEM SPA编辑器将SPA集成到it的创作上下文中，必须为SPA添加一些附加内容。

+ [了解如何引导SPA for AEM SPA Editor](./spa-bootstrap.md)

## 3.可编辑的固定组件

首先，探索向SPA中添加可编辑的“固定组件”。 这说明了开发人员如何将特定的可编辑组件放入SPA中。 虽然作者可以更改组件的内容，但他们无法删除组件或更改其位置、位置或大小。

+ [了解可编辑的固定组件](./spa-fixed-component.md)

## 4.可编辑的容器组件

接下来，浏览向SPA中添加一个可编辑的“容器组件”。 这说明了开发人员如何将容器组件放入SPA中。 容器组件允许作者将允许的组件放入组件中，并调整组件的布局。

+ [了解可编辑的容器组件](./spa-container-component.md)

## 5.动态路由和可编辑组件

最后，将前几章阐述的概念用于动态路线；根据路由参数显示不同内容的路由。 这说明了如何使用AEM SPA Editor在以编程方式驱动和派生的路由上创作内容。

+ [了解动态路由和可编辑组件](./spa-dynamic-routes.md)

## 其他资源

+ [在AEM Docs中编辑外部SPA](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/hybrid/editing-external-spa.html)
+ [AEM WCM组件 — React Core实施](https://www.npmjs.com/package/@adobe/aem-core-components-react-base)
+ [AEM WCM组件 — Spa编辑器 — React Core实施](https://www.npmjs.com/package/@adobe/aem-core-components-react-spa)
