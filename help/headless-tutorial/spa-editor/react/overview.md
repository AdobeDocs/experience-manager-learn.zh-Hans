---
title: AEM SPA Editor 和 React 快速入门
description: 创建您的首个React单页应用程序(SPA)，该应用程序可在Adobe Experience Manager AEM和WKND SPA中编辑。 了解如何使用AEM SPA Editor的React JS框架创建SPA。 本多部分教程将演示如何实施React应用程序，以打造虚构的生活方式品牌WKND。 本教程涵盖SPA的端到端创建以及与AEM的集成。
sub-product: sites
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
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '466'
ht-degree: 3%

---

# 在AEM中创建您的第一个React SPA {#overview}

欢迎使用多部分教程，该教程专为不熟悉Adobe Experience Manager(AEM)中&#x200B;**SPA Editor**&#x200B;功能的开发人员设计。 本教程将指导您实施React应用程序，以打造虚构的生活方式品牌WKND。 React应用程序将开发并设计为与AEM SPA Editor一起部署，React Editor可将React组件映射到AEM组件。 部署到AEM的已完成SPA可以使用AEM的传统在线编辑工具进行动态创作。

![已实施最终SPA](assets/wknd-spa-implementation.png)

*WKND SPA实施*

## 关于

本教程旨在将&#x200B;**AEM用作Cloud Service**，并且向后兼容&#x200B;**AEM 6.5.4+**&#x200B;和&#x200B;**AEM 6.4.8+**。

## 最新代码

所有教程代码都可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa)上找到。

[最新代码库](https://github.com/adobe/aem-guides-wknd-spa/releases)可作为可下载的AEM包提供。

## 前提条件

在启动本教程之前，您需要满足以下条件：

* HTML、CSS和JavaScript的基本知识
* 对[React](https://reactjs.org/tutorial/tutorial.html)的基本熟悉

*虽然不需要，但对开发传统的AEM Sites组件有基 [本的了解](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html)。*

## 本地开发环境 {#local-dev-environment}

要完成本教程，需要本地开发环境。 屏幕截图和视频是使用AEM作为Cloud ServiceSDK捕获的，该SDK在Mac OS环境中运行，并且IDE为[Visual Studio代码](https://code.visualstudio.com/)。 除非另有说明，否则命令和代码应独立于本地操作系统。

### 所需软件

* [AEM as a Cloud ServiceSDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html)、 [AEM 6.5.4+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-65) 或 [AEM 6.4.8+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) （3.3.9或更高版本）
* [Node.](https://nodejs.org/en/) jsand  [npm](https://www.npmjs.com/)

>[!NOTE]
>
> **初次使用AEM as aCloud Service?** 请参阅以 [下指南，以使用AEM as a Cloud ServiceSDK设置本地开发环境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。
>
> **AEM 6.5的新增功能？** 请参阅以 [下指南以设置本地开发环境](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)。

## 后续步骤 {#next-steps}

你在等什么?!通过导航到[创建项目](create-project.md)章节来启动教程，并了解如何使用AEM项目原型生成启用了SPA Editor的项目。
