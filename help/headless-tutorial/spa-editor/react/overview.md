---
title: AEM SPA Editor 和 React 快速入门
description: 创建您的首个 React 单页面应用程序 (SPA)，该应用程序可在带 WKND SPA 的 Adobe Experience Manager (AEM) 中编辑。了解如何结合使用 React JS 框架和 AEM 的 SPA 编辑器来创建 SPA。此多节教程演练了为虚构的生活方式品牌 WKND 实施 React 应用程序的过程。本教程涉及创建 SPA 的全过程以及与 AEM 的集成。
version: Cloud Service
jira: KT-5912
thumbnail: 5912-spa-react.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 38802296-8988-4300-a04a-fcbbe98ac810
last-substantial-update: 2022-08-25T00:00:00Z
duration: 100
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '417'
ht-degree: 20%

---

# 在AEM中创建您的第一个React SPA {#overview}

{{edge-delivery-services}}

欢迎使用专为新加入的开发人员设计的多部分教程 **SPA编辑器** Adobe Experience Manager (AEM)中的功能。 本教程介绍了为虚构的生活方式品牌WKND实施React应用程序的过程。 React应用程序是使用AEM SPA Editor开发和设计的，该编辑器将React组件映射到AEM组件。 部署到AEM的已完成SPA可以使用传统的AEM内联编辑工具动态创作。

![已实施的最终SPA](assets/wknd-spa-implementation.png)

*WKND SPA实施*

## 关于

本教程专门设计用于 **AEMas a Cloud Service** 并且向后兼容 **AEM 6.5.4+** 和 **AEM 6.4.8+**.

## 最新代码

所有教程代码均可在上找到 [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

此 [最新的代码库](https://github.com/adobe/aem-guides-wknd-spa/releases) 作为可下载的AEM包提供。

## 前提条件

在开始本教程之前，您需要满足以下条件：

* HTML、CSS和JavaScript的基础知识
* 对的基本了解 [React](https://reactjs.org/tutorial/tutorial.html)

*虽然没有必要，但基本了解以下内容会很有帮助 [开发传统AEM Sites组件](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=zh-Hans).*

## 本地开发环境 {#local-dev-environment}

需要本地开发环境来完成本教程。 使用在Mac OS环境中运行的AEMas a Cloud ServiceSDK捕获屏幕截图和视频，环境为 [Visual Studio代码](https://code.visualstudio.com/) 作为IDE。 除非另有说明，否则命令和代码应独立于本地操作系统。

### 所需的软件

* [AEMAS A CLOUD SERVICESDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html)， [AEM 6.5.4+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-65) 或 [AEM 6.4.8+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) （3.3.9或更高版本）
* [Node.js](https://nodejs.org/en/) 和 [npm](https://www.npmjs.com/)

>[!NOTE]
>
> **还不熟悉AEMas a Cloud Service？** 查看 [以下指南介绍了如何使用AEMas a Cloud ServiceSDK设置本地开发环境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-Hans).
>
> **还不熟悉AEM 6.5？** 查看 [以下指南介绍了如何设置本地开发环境](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=zh-Hans).

## 后续步骤 {#next-steps}

你在等什么?!导航到 [创建项目](create-project.md) 章中，并了解如何使用SPA项目原型生成启用AEM编辑器的项目。
