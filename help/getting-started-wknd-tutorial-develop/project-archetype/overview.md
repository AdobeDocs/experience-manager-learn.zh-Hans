---
title: AEM Sites — 项目原型快速入门
description: AEM Sites — 项目原型快速入门。 WKND教程是一个多部分教程，专为不熟悉Adobe Experience Manager的开发人员设计。 本教程介绍了虚拟生活方式品牌WKND的AEM站点的实施。 本教程涵盖了项目设置、maven原型、核心组件、可编辑模板、客户端库和组件开发等基本主题。
version: 6.5, Cloud Service
type: Tutorial
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
index: y
exl-id: 90d14734-f644-4a45-9361-1e47a5b52fff
recommendations: disable
source-git-commit: bbdb045edf5f2c68eec5094e55c1688e725378dc
workflow-type: tm+mt
source-wordcount: '476'
ht-degree: 20%

---

# AEM Sites — 项目原型快速入门 {#project-archetype}

欢迎使用专为不熟悉Adobe Experience Manager (AEM)的开发人员设计的多部分教程。 本教程介绍了虚拟生活方式品牌 WKND 的 AEM 站点的实现。 

本教程首先使用 [AEM项目原型](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html) 以生成新项目。

本教程设计用于 **AEMas a Cloud Service** 向后兼容 **AEM 6.5.14+**. 通过以下方式实施站点：

* [Maven AEM 项目原型](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html)
* [核心组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hans)
* [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/getting-started.html)
* [Sling 模型](https://sling.apache.org/documentation/bundles/models.html)
* [可编辑模板](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html?lang=zh-Hans)
* [样式系统](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)

*预计需要1-2小时来完成教程的每个部分。*

## 本地开发环境 {#local-dev-environment}

需要本地开发环境来完成本教程。 使用在macOS环境中运行的AEMas a Cloud ServiceSDK捕获屏幕截图和视频，并具有 [Visual Studio Code](https://code.visualstudio.com/) 作为IDE。 除非另有说明，否则命令和代码应独立于本地操作系统。

### 所需的软件

以下内容应安装在本地：

* [本地AEM **作者** 实例](https://experience.adobe.com/#/downloads) (Cloud ServiceSDK或6.5.14及更高版本)
* [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) （3.3.9或更高版本）
* [Node.js](https://nodejs.org/en/) （LTS — 长期支持）
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Visual Studio Code](https://code.visualstudio.com/) 或等效的IDE
   * [VSCode AEM同步](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)  — 整个教程中使用的工具

>[!NOTE]
>
> **不熟悉AEMas a Cloud Service？** 查看 [以下指南介绍了如何使用AEMas a Cloud ServiceSDK设置本地开发环境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-Hans).
>
> **AEM 6.5的新手？** 查看 [以下指南介绍了如何设置本地开发环境](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=zh-Hans).

## GitHub {#github}

本教程中的代码可以在GitHub上的AEM Guide存储库中找到：

**[GitHub： WKND站点项目](https://github.com/adobe/aem-guides-wknd)**

此外，本教程的每个部分在GitHub中都有其自己的分支。 用户只需检出与上一个部件对应的分支，即可随时开始教程。

## 后续步骤 {#next-steps}

你在等什么？ 通过导航到 [项目设置](project-setup.md) 章节并了解如何使用AEM项目原型生成新的Adobe Experience Manager项目。
