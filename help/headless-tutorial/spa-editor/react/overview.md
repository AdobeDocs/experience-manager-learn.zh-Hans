---
title: AEM SPA Editor 和 React 快速入门
description: 创建您的首个 React 单页面应用程序 (SPA)，该应用程序可在带 WKND SPA 的 Adobe Experience Manager (AEM) 中编辑。了解如何结合使用 React JS 框架和 AEM 的 SPA 编辑器来创建 SPA。此多节教程演练了为虚构的生活方式品牌 WKND 实施 React 应用程序的过程。此教程涵盖了 SPA 的端到端创建以及与 AEM 的集成。
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5912
thumbnail: 5912-spa-react.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
exl-id: 38802296-8988-4300-a04a-fcbbe98ac810
last-substantial-update: 2022-08-25T00:00:00Z
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# 在AEM中创建您的第一个React SPA {#overview}

欢迎参加为初次使用 **SPA编辑器** 功能(Adobe Experience Manager(AEM))。 本教程将指导您实施React应用程序，以打造虚构的生活方式品牌WKND。 React应用程序是开发并设计为与AEM SPA Editor一起部署的，React Editor可将React组件映射到AEM组件。 部署到AEM的已完成SPA可以使用AEM的传统在线编辑工具进行动态创作。

![已实施最终SPA](assets/wknd-spa-implementation.png)

*WKND SPA实施*

## 关于

本教程旨在与 **AEMas a Cloud Service** 并且向后兼容 **AEM 6.5.4+** 和 **AEM 6.4.8+**.

## 最新代码

所有教程代码都可以在 [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

的 [最新代码库](https://github.com/adobe/aem-guides-wknd-spa/releases) 可作为可下载的AEM包使用。

## 前提条件

在启动本教程之前，您需要满足以下条件：

* HTML、CSS和JavaScript的基本知识
* 对 [React](https://reactjs.org/tutorial/tutorial.html)

*虽然不需要，但对 [开发传统AEM Sites组件](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=zh-Hans).*

## 本地开发环境 {#local-dev-environment}

要完成本教程，需要本地开发环境。 屏幕截图和视频是使用在Mac OS环境中运行的AEMas a Cloud ServiceSDK捕获的，该SDK [Visual Studio代码](https://code.visualstudio.com/) 作为IDE。 除非另有说明，否则命令和代码应独立于本地操作系统。

### 所需软件

* [AEMas a Cloud ServiceSDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html), [AEM 6.5.4+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-65) 或 [AEM 6.4.8+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [阿帕奇·马文](https://maven.apache.org/) （3.3.9或更高版本）
* [Node.js](https://nodejs.org/en/) 和 [npm](https://www.npmjs.com/)

>[!NOTE]
>
> **是AEMas a Cloud Service的新用户？** 查看 [以下使用AEMas a Cloud Service SDK设置本地开发环境的指南](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-Hans).
>
> **AEM 6.5的新增功能？** 查看 [设置本地开发环境的以下指南](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=zh-Hans).

## 后续步骤 {#next-steps}

你在等什么?!通过导航到 [创建项目](create-project.md) 章节和了解如何使用SPA项目原型生成启用了AEM的项目。
