---
title: AEM Sites 快速入门 - 项目原型
description: AEM Sites 快速入门 - 项目原型。WKND 教程是专为不熟悉 Adobe Experience Manager 的开发人员设计的多段式教程。此教程介绍了虚拟生活方式品牌 WKND 的 AEM 网站的实施。此教程涵盖了项目设置、maven 原型、核心组件、可编辑模板、客户端库和组件开发等基本主题。
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
index: y
doc-type: Tutorial
exl-id: 90d14734-f644-4a45-9361-1e47a5b52fff
recommendations: disable
duration: 74
source-git-commit: dbf63f30ccfd06e4f4d7883c2f7bc4ac78245364
workflow-type: ht
source-wordcount: '414'
ht-degree: 100%

---

# AEM Sites 快速入门 - 项目原型 {#project-archetype}

{{traditional-aem}}

欢迎参加专为不熟悉 Adobe Experience Manager (AEM) 的开发人员设计的多段式教程。本教程介绍了虚拟生活方式品牌 WKND 的 AEM 网站的实施。

本教程从使用 [AEM 项目原型](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=zh-Hans)生成一个新项目开始。

本教程需要与 **AEM as a Cloud Service** 一起使用，且向后兼容 **AEM 6.5.14+**。站点会通过以下项目实施：

* [Maven AEM 项目原型](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=zh-Hans)
* [核心组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hans)
* [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/getting-started.html?lang=zh-Hans)
* [Sling 模型](https://sling.apache.org/documentation/bundles/models.html)
* [可编辑模板](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html?lang=zh-Hans)
* [样式系统](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html?lang=zh-Hans)

*预计完成本教程的每个部分需要 1-2 小时。*

## 本地开发环境 {#local-dev-environment}

完成本教程需要本地开发环境。屏幕快照和视频使用 AEM as a Cloud Service SDK 在 [Visual Studio Code](https://code.visualstudio.com/) 作为 IDE 的 macOS 环境中捕获。除非另有说明，否则命令和代码应与本地操作系统无关。

### 所需软件

需要在本地安装以下软件：

* [本地 AEM **作者**&#x200B;实例](https://experience.adobe.com/#/downloads) (Cloud Service SDK 或 6.5.14+)
* [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/)（3.3.9 或更新版本）
* [Node.js](https://nodejs.org/en/)（LTS - 长期支持）
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Visual Studio Code](https://code.visualstudio.com/) 或同类 IDE
   * [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) - 此工具在整个教程中使用

>[!NOTE]
>
> **刚接触 AEM as a Cloud Service？**&#x200B;请参阅[以下使用 AEM as a Cloud Service SDK 搭建本地开发环境的指南](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-hans)。
>
> **刚接触 AEM 6.5？** 请参考[以下搭建本地开发环境的指南](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=zh-hans)。

## GitHub {#github}

本教程的代码可以在 GitHub 上的 AEM 指南存储库中找到：

**[GitHub：WKND Sites 项目](https://github.com/adobe/aem-guides-wknd)**

此外，本教程的每个部分在 GitHub 上都有自己的分支。用户可以通过签出与上一个部分对应的分支，随时开始学习本教程。

## 后续步骤 {#next-steps}

您在等什么？导航至[项目设置](project-setup.md)一章开始学习本教程，了解如何使用 AEM 项目原型生成新的 Adobe Experience Manager 项目。
