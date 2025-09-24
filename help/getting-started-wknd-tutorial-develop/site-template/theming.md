---
title: 主题化工作流 | AEM 快速网站创建
description: 了解如何更新 Adobe Experience Manager 网站的主题源，以应用品牌特有的样式。了解如何使用代理服务器查看 CSS 和 Javascript 更新的实时预览。本教程还将介绍如何使用 Adobe Cloud Manager 的前端管道将主题更新部署到 AEM 网站。
version: Experience Manager as a Cloud Service
feature: Core Components
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-7498
thumbnail: KT-7498.jpg
doc-type: Tutorial
exl-id: 98946462-1536-45f9-94e2-9bc5d41902d4
recommendations: noDisplay, noCatalog
duration: 1275
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '458'
ht-degree: 100%

---

# 主题化工作流 {#theming}

在本章中，我们将更新 Adobe Experience Manager 网站的主题源，以应用品牌特有的样式。我们将了解在针对实时网站编码时，如何使用代理服务器查看 CSS 和 Javascript 更新的预览。本教程还将介绍如何使用 Adobe Cloud Manager 的前端管道将主题更新部署到 AEM 网站。

最后，更新我们的网站，以包含与 WKND 品牌相匹配的样式。

## 先决条件 {#prerequisites}

这是一个多段式教程，并且假设您已完成[页面模板](./page-templates.md)一章中概述的步骤。

## 目标

1. 了解如何下载和更改网站的主题源。
1. 了解如何针对实时网站编码以进行实时预览。
1. 了解使用 Adobe Cloud Manager 的前端管道将编译的 CSS 和 JavaScript 更新作为主题的一部分进行交付的端到端工作流。

## 更新主题 {#theme-update}

接下来，我们对主题源进行更改，使网站与 WKND 品牌的外观相匹配。

>[!VIDEO](https://video.tv.adobe.com/v/3453630?quality=12&learn=on&captions=chi_hans)

视频的高级概述步骤：

1. 在 AEM 中创建一个本地用户，与代理开发服务器一起使用。
1. 从 AEM 下载主题源，然后使用本地 IDE（如 VSCode）打开。
1. 更改主题源，使用一个代理开发服务器实时预览 CSS 和 JavaScript 的变化。
1. 更新主题源，使杂志文章与 WKND 品牌化样式和模型相匹配。

### 解决方案文件

下载 [WKND 主题示例](assets/theming/WKND-THEME-src-1.1.zip)的已完成的样式

## 使用前端管道部署一个主题 {#deploy-theme}

使用 Cloud Manager 的前端管道将主题更新部署到 AEM 环境。

>[!VIDEO](https://video.tv.adobe.com/v/338722?quality=12&learn=on)

视频的高级概述步骤：

1. [在 Cloud Manager 中创建一个新的 git 存储库](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/managing-code/cloud-manager-repositories.html?lang=zh-Hans)
1. 将您的主题源项目添加到 Cloud Manager git 存储库中：

   ```shell
   $ cd <PATH_TO_THEME_SOURCES_FOLDER>
   $ git init -b main
   $ git add .
   $ git commit -m "initial commit"
   $ git remote add origin <CLOUD_MANAGER_GIT_REPOSITORY_URL>
   ```

1. 在 Cloud Manager 中配置一个[前端管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?lang=zh-Hans)，以部署前端代码。
1. 运行前端管道，将更新部署到目标 AEM 环境。

### 存储库示例

有几个 GitHub 存储库示例可以作为参考：

* [aem-site-template-standard](https://github.com/adobe/aem-site-template-standard)
* [aem-site-template-basic-theme-e2e](https://github.com/adobe/aem-site-template-basic-theme-e2e) - 用作“真实世界”项目的示例。

## 恭喜！ {#congratulations}

祝贺您更新了一个主题并将其部署到 AEM！

### 后续步骤 {#next-steps}

通过使用 [AEM 项目原型](../project-archetype/overview.md)创建一个网站，深入了解 AEM 开发并详细了解底层技术。
