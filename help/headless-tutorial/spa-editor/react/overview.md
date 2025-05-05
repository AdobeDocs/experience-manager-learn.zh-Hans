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
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '417'
ht-degree: 20%

---

# 在AEM中创建您的第一个React SPA {#overview}

{{edge-delivery-services}}

欢迎使用专为不熟悉Adobe Experience Manager (AEM)中的&#x200B;**SPA编辑器**&#x200B;功能的开发人员设计的多部分教程。 本教程介绍了为虚构的生活方式品牌WKND实施React应用程序的过程。 React应用程序是使用AEM的SPA Editor开发和设计的，该编辑器可将React组件映射到AEM组件。 部署到AEM的已完成SPA可以使用AEM的传统内联编辑工具动态创作。

![已实施的最终SPA](assets/wknd-spa-implementation.png)

*WKND SPA实施*

## 关于

此教程设计为使用&#x200B;**AEM as a Cloud Service**，并且向后兼容&#x200B;**AEM 6.5.4+**&#x200B;和&#x200B;**AEM 6.4.8+**。

## 最新代码

可在[GitHub](https://github.com/adobe/aem-guides-wknd-spa)上找到所有教程代码。

[最新代码库](https://github.com/adobe/aem-guides-wknd-spa/releases)可用作可下载的AEM包。

## 先决条件

在开始本教程之前，您需要满足以下条件：

* HTML、CSS和JavaScript的基础知识
* 对[React](https://reactjs.org/tutorial/tutorial.html)的基本了解

*虽然不是必需的，但对[开发传统AEM Sites组件](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=zh-Hans)有基本的了解是有益的。*

## 本地开发环境 {#local-dev-environment}

需要本地开发环境来完成本教程。 使用在Mac OS环境中运行的AEM as a Cloud Service SDK捕获屏幕截图和视频，并将[Visual Studio Code](https://code.visualstudio.com/)用作IDE。 除非另有说明，否则命令和代码应独立于本地操作系统。

### 所需的软件

* [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=zh-Hans)、[AEM 6.5.4+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=zh-Hans#aem-65)或[AEM 6.4.8+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=zh-Hans#aem-64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) （3.3.9或更高版本）
* [Node.js](https://nodejs.org/en/)和[npm](https://www.npmjs.com/)

>[!NOTE]
>
> **是AEM as a Cloud Service的新用户？**&#x200B;请查看以下[指南，了解如何使用AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-hans)设置本地开发环境。
>
> **是AEM 6.5的新手吗？**&#x200B;请查看以下[指南以设置本地开发环境](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=zh-hans)。

## 后续步骤 {#next-steps}

你在等什么?!导航到[创建项目](create-project.md)一章，开始阅读本教程，并了解如何使用AEM项目原型生成启用SPA Editor的项目。
