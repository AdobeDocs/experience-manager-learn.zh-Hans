---
title: AEM Sites入门 — 项目原型
description: AEM Sites入门 — 项目原型。 WKND教程是一个多部分教程，专为不熟悉Adobe Experience Manager的开发人员而设计。 本教程将指导您实施一个AEM网站，以打造一个虚构的生活方式品牌WKND。 本教程涵盖基本主题，如项目设置、Maven原型、核心组件、可编辑模板、客户端库和组件开发。
sub-product: 站点
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: 核心组件、页面编辑器、可编辑模板、AEM项目原型
topic: 内容管理、开发
role: Developer
level: Beginner
mini-toc-levels: 1
index: y
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '488'
ht-degree: 3%

---


# AEM Sites入门 — 项目原型 {#project-archetype}

欢迎参加为初次使用Adobe Experience Manager(AEM)的开发人员而设计的多部分教程。 本教程将指导您实施AEM网站，以打造虚构的生活方式品牌WKND。

本教程首先使用[AEM项目原型](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html)生成新项目。

本教程旨在将&#x200B;**AEM用作Cloud Service**，并且向后兼容&#x200B;**AEM 6.5.5.0+**&#x200B;和&#x200B;**AEM 6.4.8.1+**。 站点的实施方式如下：

* [Maven AEM项目原型](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html)
* [核心组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hans)
* [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/getting-started/getting-started.html)
* Sling 模型
* [可编辑的模板](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)
* [样式系统](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)

*估计需要1-2小时才能完成教程的每个部分。*

## 本地开发环境 {#local-dev-environment}

要完成本教程，需要本地开发环境。 屏幕截图和视频是使用AEM作为Cloud ServiceSDK捕获的，该SDK在Mac OS环境中运行，并且IDE为[Visual Studio代码](https://code.visualstudio.com/)。 除非另有说明，否则命令和代码应独立于本地操作系统。

### 所需软件

应在本地安装以下内容：

* 本地AEM **Author**&#x200B;实例(Cloud ServiceSDK、6.5.5+或6.4.8.1+)
* [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) （3.3.9或更高版本）
* [Node.js](https://nodejs.org/en/) （LTS — 长期支持）
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Visual Studio代](https://code.visualstudio.com/) 码或等效IDE
   * [VSCode AEM同步](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)  — 在整个教程中使用的工具

>[!NOTE]
>
> **初次使用AEM as aCloud Service?** 请参阅以 [下指南，以使用AEM as a Cloud ServiceSDK设置本地开发环境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。
>
> **AEM 6.5的新增功能？** 请参阅以 [下指南以设置本地开发环境](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)。

## Github {#github}

项目的所有代码均可在AEM Guide存储库的Github上找到：

**[GitHub:WKND Sites项目](https://github.com/adobe/aem-guides-wknd)**

此外，本教程的每个部分在GitHub中都有其自己的分支。 用户只需签出与上一部分对应的分支，即可随时开始教程。

## 后续步骤 {#next-steps}

你在等什么?!通过导航到[项目设置](project-setup.md)章节来启动教程，并了解如何使用AEM项目原型生成新的Adobe Experience Manager项目。
