---
title: AEM Sites入门 — Project Archetype
description: AEM Sites入门 — Project Archetype。 WKND教程是为初次接触Adobe Experience Manager的开发人员设计的多部分教程。 本教程将介绍一个虚构生活方式品牌WKND的AEM站点的实施。 本教程涵盖基本主题，如项目设置、主原型、核心组件、可编辑模板、客户端库和组件开发。
sub-product: 站点
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
index: y
feature: 核心组件、页面编辑器、可编辑模板、AEM项目原型
topic: 内容管理，开发
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: fb6c56dfc85fbcb36a68210f068fd496849c352e
workflow-type: tm+mt
source-wordcount: '502'
ht-degree: 7%

---


# AEM Sites入门 — Project Archetype {#project-archetype}

欢迎使用为初次接触Adobe Experience Manager(AEM)的开发人员设计的多部分教程。 本教程将介绍如何为WKND这个虚构生活方式品牌实施AEM站点。

本教程开始使用[AEM Project Archetype](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html)生成新项目。

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

## Github {#github}

项目的所有代码都可以在AEM指南回购协议中的Github上找到：

**[GitHub:WKND Sites项目](https://github.com/adobe/aem-guides-wknd)**

此外，本教程的每个部分在GitHub中都有自己的分支。 用户只需签出与上一部分对应的分支，即可随时开始教程。

## 后续步骤{#next-steps}

你在等什么！开始教程，方法是导航到[“项目设置”](project-setup.md)一章，并学习如何使用AEM Project Archetype生成新的Adobe Experience Manager项目。
