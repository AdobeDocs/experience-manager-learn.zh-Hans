---
title: AEM Sites - WKND 教程快速入门
description: AEM Sites - WKND 教程快速入门. WKND教程是为Adobe Experience Manager新手的开发人员设计的多部分教程。 本教程将介绍一个AEM站点的实施过程，该站点用于一个虚构的生活方式品牌WKND。 本教程涵盖基本主题，如项目设置、主原型、核心组件、可编辑模板、客户端库和组件开发。
sub-product: 站点
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
KT: 4132
mini-toc-levels: 1
index: y
thumbnail: 30476.jpg
translation-type: tm+mt
source-git-commit: 69c1767098cc9da8ec0ae2bd83d25417d330f393
workflow-type: tm+mt
source-wordcount: '970'
ht-degree: 6%

---


# AEM Sites - WKND 教程快速入门 {#introduction}

欢迎学习为Adobe Experience Manager(AEM)新手开发人员设计的多部分教程。 本教程将介绍AEM站点的实施过程，该站点用于虚构的生活方式品牌WKND。 本教程涵盖基本主题，如项目设置、核心组件、可编辑模板、客户端库以及使用Adobe Experience Manager Sites进行组件开发。

## 概述 {#wknd-tutorial-overview}

此多部分教程的目标是教给开发人员如何使用Adobe Experience Manager(AEM)的最新标准和技术实施网站。 完成本教程后，开发人员应了解该平台的基本基础并了解AEM中的常见设计模式。

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

本教程设计为将&#x200B;**AEM用作Cloud Service**，并向后兼容&#x200B;**AEM 6.5+**&#x200B;和&#x200B;**AEM 6.4.2+**。 该站点通过以下方式实现：

* [Maven AEM Project Archetype](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/developing/archetype/overview.html)
* [核心组件](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/introduction.html)
* [HTL](https://docs.adobe.com/content/help/en/experience-manager-htl/using/getting-started/getting-started.html)
* Sling Models
* [可编辑的模板](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)
* [样式系统](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)

*估计1-2小时即可完成教程的每个部分。*

## 关于教程{#about-tutorial}

WKND是一家虚构的在线杂志和博客，主要关注几个国际城市的夜生活、活动和事件。

### Adobe XDUI套件

为了使本教程更接近真实场景，Adobe才华横溢的UX设计人员使用[Adobe XD](https://www.adobe.com/products/xd.html)为站点创建了模型。 在本教程中，将各种设计实现到一个完全可创作的AEM站点中。 特别感谢&#x200B;**Lorenzo Buosi**&#x200B;和&#x200B;**Kilian Amendola**&#x200B;为WKND站点创造了美观的设计。

下载XD UI套件：

* [AEM核心组件UI套件](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [WKND UI套件](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

名称WKND很合适，因为我们希望开发人员能够完成&#x200B;***weekend***&#x200B;的大部分工作，以完成教程。

### Github {#github}

项目的所有代码都可以在AEM Guide repo中的Github上找到：

**[GitHub:WKND Sites项目](https://github.com/adobe/aem-guides-wknd)**

此外，本教程的每个部分在GitHub中都有其自己的分支。 用户只需检出与前一部分对应的分支，即可随时开始教程。

>[!NOTE]
>
> 如果您使用本教程的先前版本，您仍可以在GitHub上找到[解决方案包](https://github.com/adobe/aem-guides-wknd/releases/tag/archetype-18.1)和[代码](https://github.com/adobe/aem-guides-wknd/tree/archetype-18.1)。

## 本地开发环境{#local-dev-environment}

必须具备本地开发环境才能完成本教程。 截屏和视频使用AEM作为在Mac OS环境上运行的Cloud ServiceSDK进行捕获。 除非另有说明，否则命令和代码应独立于本地操作系统。

**刚从AEM当Cloud Service?** 查看以 [下指南，使用AEM作为环境SDK设置本地开发Cloud Service](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。

**初次使用AEM 6.5?** 请参阅以 [下指南以设置本地开发环境](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)。

### 所需软件

应在本地安装以下内容：

* [AEM作为](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk) Cloud ServiceSDK [或](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/technical-requirements.html) AEM 6. [5或AEM 6.4 + SP2](https://helpx.adobe.com/cn/experience-manager/6-4/release-notes/sp-release-notes.html)
* [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html) (仅AEM 6.5+)
* [Apache Maven](https://maven.apache.org/) （3.3.9或更高版本）
* [Node.js v10+](https://nodejs.org/en/)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)

### 集成开发环境(IDE)

本教程将[Eclipse](https://www.eclipse.org/)与[AEM开发人员工具插件](https://eclipse.adobe.com/aem/dev-tools/)一起用作IDE，但可以使用任何支持Java和Maven项目的IDE。 本教程中对特定IDE功能的依赖性很低。

有关使用Eclipse或其他IDE（如[Visual Studio Code](https://code.visualstudio.com/)或[IntelliJ](https://www.jetbrains.com/idea/)、[）的详细步骤，请参阅以下指南](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)。

## 引用站点 {#reference-site}

WKND站点的完成版本也可作为参考：[https://wknd.site/](https://wknd.site/)

本教程涵盖AEM开发人员所需的主要开发技能，但&#x200B;*不*&#x200B;将构建整个站点端到端。 完成的参考站点是探索和了解更多AEM开箱即用功能的又一个极好的资源。

要在跳入教程之前测试最新代码，请从GitHub](https://github.com/adobe/aem-guides-wknd/releases/latest)**下载并安装**[&#x200B;最新版本。

### 由Adobe Stock提供支持

WKND参考网站中的许多图像来自[Adobe Stock](https://stock.adobe.com/)，是第三方材料，如[https://www.adobe.com/legal/terms.html](https://www.adobe.com/legal/terms.html)的演示资产附加条款中所定义。 如果您希望将Adobe Stock图像用于除查看此演示网站外的其他用途，例如在网站上或在营销材料中展示该图像，则可以在Adobe Stock购买许可。

借助Adobe Stock，您可以访问超过1.4亿个高品质免版税图像，包括照片、图形、视频和模板，快速开始您的创意项目。

## 后续步骤{#next-steps}

你在等什么！开始教程，方法是导航到[项目设置](project-setup.md)一章，并学习如何使用AEM项目原型生成新的Adobe Experience Manager项目。
