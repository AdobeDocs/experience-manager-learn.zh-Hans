---
title: AEM SPA Editor 和 React 快速入门
description: 创建您的首个 React 单页面应用程序 (SPA)，该应用程序可在带 WKND SPA 的 Adobe Experience Manager (AEM) 中编辑。了解如何结合使用 React JS 框架和 AEM 的 SPA 编辑器来创建 SPA。此多节教程演练了为虚构的生活方式品牌 WKND 实施 React 应用程序的过程。本教程涉及创建 SPA 的全过程以及与 AEM 的集成。
version: Experience Manager as a Cloud Service
jira: KT-5912
thumbnail: 5912-spa-react.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 38802296-8988-4300-a04a-fcbbe98ac810
last-substantial-update: 2022-08-25T00:00:00Z
duration: 71
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: ht
source-wordcount: '417'
ht-degree: 100%

---

# 在 AEM 中创建您的第一个 React SPA {#overview}

{{spa-editor-deprecation}}

欢迎来到本系列教程，本教程专为刚接触 Adobe Experience Manager (AEM) 中 **SPA 编辑器**&#x200B;功能的新手开发者设计。本教程将逐步介绍如何为虚构的生活方式品牌 WKND 实施一个 React 应用程序。该 React 应用程序的开发和设计旨在与 AEM 的 SPA 编辑器一起部署，该编辑器会将 React 组件映射到 AEM 组件。部署到 AEM 的完整 SPA 可以使用 AEM 的传统内联编辑工具进行动态创作。

![最终实施的 SPA](assets/wknd-spa-implementation.png)

*WKND SPA 实施*

## 关于

本教程旨在配合 **AEM as a Cloud Service** 使用，且向后兼容 **AEM 6.5.4+** 和 **AEM 6.4.8+**。

## 最新代码

所有教程代码均可在 [GitHub](https://github.com/adobe/aem-guides-wknd-spa) 上找到。

[最新的代码库](https://github.com/adobe/aem-guides-wknd-spa/releases)已作为可下载的 AEM 包提供。

## 先决条件

在开始本教程之前，您需要具备以下内容：

* HTML、CSS 和 JavaScript 的基础知识
* 基本熟悉 [React](https://reactjs.org/tutorial/tutorial.html)

*虽然不是必需的，但对[开发传统 AEM Sites 组件](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=zh-Hans)有基本的了解是有益的。*

## 本地开发环境 {#local-dev-environment}

完成本教程需要本地开发环境。屏幕截图和视频是使用 AEM as a Cloud Service SDK 在 Mac OS 环境下捕获的，其中 [Visual Studio Code](https://code.visualstudio.com/) 作为 IDE。除非另有说明，否则命令和代码应与本地操作系统无关。

### 所需软件

* [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=zh-Hans), [AEM 6.5.4+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=zh-Hans#aem-65) 或 [AEM 6.4.8+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=zh-Hans#aem-64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/)（3.3.9 或更新版本）
* [Node.js](https://nodejs.org/en/) 和 [npm](https://www.npmjs.com/)

>[!NOTE]
>
> **刚接触 AEM as a Cloud Service？**&#x200B;请参阅[以下使用 AEM as a Cloud Service SDK 搭建本地开发环境的指南](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-hans)。
>
> **刚接触 AEM 6.5？** 请参考[以下搭建本地开发环境的指南](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=zh-hans)。

## 后续步骤 {#next-steps}

你还在等什么？！请导航至[创建项目](create-project.md)章节，开始学习如何使用 AEM 项目原型生成支持 SPA 编辑器的项目。
