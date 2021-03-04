---
title: AEM Sites - WKND 教程快速入门
description: AEM Sites - WKND 教程快速入门. WKND教程是为初次接触Adobe Experience Manager的开发人员设计的多部分教程。 本教程将介绍一个虚构生活方式品牌WKND的AEM站点的实施。 本教程涵盖基本主题，如项目设置、主原型、核心组件、可编辑模板、客户端库和组件开发。
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
feature: 核心组件、页面编辑器、可编辑模板、AEM项目原型
topic: 内容管理，开发
role: 开发人员
level: 初学者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '907'
ht-degree: 7%

---


# AEM Sites - WKND 教程快速入门 {#introduction}

欢迎使用为初次接触Adobe Experience Manager(AEM)的开发人员设计的多部分教程。 本教程将介绍如何为WKND这个虚构生活方式品牌实施AEM站点。 本教程涵盖基本主题，如项目设置、核心组件、可编辑模板、客户端库以及使用Adobe Experience Manager Sites进行组件开发。

## 概述 {#wknd-tutorial-overview}

此多部分教程的目标是教给开发人员如何使用Adobe Experience Manager(AEM)中的最新标准和技术实施网站。 完成本教程后，开发人员应了解该平台的基本基础并了解AEM中的常见设计模式。

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

本教程设计为将&#x200B;**AEM用作Cloud Service**，并向后兼容&#x200B;**AEM 6.5.5.0+**&#x200B;和&#x200B;**AEM 6.4.8.1+**。 该站点的实施方式如下：

* [Maven AEM Project Archetype](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/developing/archetype/overview.html)
* [核心组件](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/introduction.html)
* [HTL](https://docs.adobe.com/content/help/en/experience-manager-htl/using/getting-started/getting-started.html)
* Sling 模型
* [可编辑的模板](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)
* [样式系统](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)

*估计需要1-2个小时来完成教程的每个部分。*

## 本地开发环境{#local-dev-environment}

完成本教程需要一个本地开发环境。 屏幕截图和视频是使用AEM作为Cloud ServiceSDK捕获的，Mac OS环境上运行，IDE为[Visual Studio代码](https://code.visualstudio.com/)。 除非另有说明，否则命令和代码应独立于本地操作系统。

### 所需软件

应在本地安装以下内容：

* 本地AEM **Author**&#x200B;实例(Cloud Service SDK、6.5.5+或6.4.8.1+)
* [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) （3.3.9或更高版本）
* [Node.js](https://nodejs.org/en/) （LTS — 长期支持）
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Visual Studio代](https://code.visualstudio.com/) 码或等效IDE
   * [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)  — 在整个教程中使用的工具

>[!NOTE]
>
> **初次使用AEM作为Cloud Service?** 请参阅以 [下指南，使用AEM作为Cloud Service SDK设置本地开发环境](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。
>
> **初次使用AEM 6.5?** 请参阅以 [下指南以设置本地开发环境](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)。

## 关于教程{#about-tutorial}

WKND是一个虚构的在线杂志和博客，关注几个国际城市的夜生活、活动和事件。

### Adobe XD UI Kit

为使本教程更接近真实场景，Adobe才华横溢的UX设计人员使用[Adobe XD](https://www.adobe.com/products/xd.html)为站点创建了模型。 在本教程中，各种设计将实现到一个完全可创作的AEM站点中。 特别感谢&#x200B;**Lorenzo Buosi**&#x200B;和&#x200B;**Kilian Amendola**&#x200B;为WKND网站设计了美观的设计。

下载XD UI套件：

* [AEM核心组件UI套件](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [WKND UI套件](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

名称WKND很合适，因为我们希望开发人员完成&#x200B;***weekend***&#x200B;的更好部分，以完成教程。

### Github {#github}

项目的所有代码都可以在AEM指南回购协议中的Github上找到：

**[GitHub:WKND Sites项目](https://github.com/adobe/aem-guides-wknd)**

此外，本教程的每个部分在GitHub中都有自己的分支。 用户只需签出与上一部分对应的分支，即可随时开始教程。

>[!NOTE]
>
> 如果您使用本教程的早期版本，您仍可以在GitHub上找到[解决方案包](https://github.com/adobe/aem-guides-wknd/releases/tag/archetype-18.1)和[代码](https://github.com/adobe/aem-guides-wknd/tree/archetype-18.1)。

## 引用站点 {#reference-site}

WKND站点的完成版本也可作为参考：[https://wknd.site/](https://wknd.site/)

本教程涵盖AEM开发人员所需的主要开发技能，但&#x200B;*不*&#x200B;将构建整个站点的端到端。 完成的参考站点是探索和查看更多开箱即用功能的另一个极好的资源。

要在跳入教程之前测试最新代码，请从GitHub](https://github.com/adobe/aem-guides-wknd/releases/latest)**下载并安装**[&#x200B;最新版本。

### 由Adobe Stock提供支持

WKND参考网站中的许多图像来自[Adobe Stock](https://stock.adobe.com/)，并且是第三方材料，如[https://www.adobe.com/legal/terms.html](https://www.adobe.com/cn/legal/terms.html)的“演示资产附加条款”中所定义。 如果您希望将Adobe Stock图像用于除查看此演示网站外的其他用途（例如在网站上或营销材料中显示它），则可以在Adobe Stock上购买许可。

借助Adobe Stock，您可以访问超过1.4亿个高品质免版税图像，包括照片、图形、视频和模板，从而快速启动您的创意项目。

## 后续步骤{#next-steps}

你在等什么！开始教程，方法是导航到[“项目设置”](project-setup.md)一章，并学习如何使用AEM Project Archetype生成新的Adobe Experience Manager项目。
