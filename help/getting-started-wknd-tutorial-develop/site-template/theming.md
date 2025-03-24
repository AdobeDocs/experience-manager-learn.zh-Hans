---
title: 主题化工作流 | AEM快速站点创建
description: 了解如何更新Adobe Experience Manager站点的主题源以应用品牌特定的样式。 了解如何使用代理服务器查看CSS和Javascript更新的实时预览。 本教程还将介绍如何使用AEM Cloud Manager的前端管道将主题更新部署到Adobe站点。
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
workflow-type: tm+mt
source-wordcount: '458'
ht-degree: 1%

---

# 主题化工作流 {#theming}

在本章中，我们更新了Adobe Experience Manager Site的主题源以应用品牌特定的样式。 我们了解如何使用代理服务器在针对实时站点进行编码时查看CSS和Javascript更新的预览。 本教程还将介绍如何使用AEM Cloud Manager的前端管道将主题更新部署到Adobe站点。

最终，我们的网站将更新为包含符合WKND品牌的样式。

## 先决条件 {#prerequisites}

这是一个多部分教程，并假定已完成[页面模板](./page-templates.md)章节中概述的步骤。

## 目标

1. 了解如何下载和修改站点的主题源。
1. 了解针对实时网站的代码如何实现实时预览。
1. 了解使用Adobe Cloud Manager的前端管道将编译的CSS和JavaScript更新作为主题的一部分交付的端到端工作流。

## 更新主题 {#theme-update}

接下来，对主题源进行更改，使站点与WKND品牌的外观相匹配。

>[!VIDEO](https://video.tv.adobe.com/v/332918?quality=12&learn=on)

视频的高级步骤：

1. 在AEM中创建用于代理开发服务器的本地用户。
1. 从AEM下载主题源并使用本地IDE（如VSCode）打开。
1. 修改主题源，并使用代理开发服务器实时预览CSS和JavaScript更改。
1. 更新主题源，使杂志文章与WKND品牌样式和模型匹配。

### 解决方案文件

下载[WKND示例主题](assets/theming/WKND-THEME-src-1.1.zip)的已完成样式

## 使用前端管道部署主题 {#deploy-theme}

使用Cloud Manager的前端管道将主题更新部署到AEM环境。

>[!VIDEO](https://video.tv.adobe.com/v/338722?quality=12&learn=on)

视频的高级步骤：

1. 在Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/managing-code/cloud-manager-repositories.html)中创建新的Git [存储库
1. 将主题源项目添加到Cloud Manager Git存储库：

   ```shell
   $ cd <PATH_TO_THEME_SOURCES_FOLDER>
   $ git init -b main
   $ git add .
   $ git commit -m "initial commit"
   $ git remote add origin <CLOUD_MANAGER_GIT_REPOSITORY_URL>
   ```

1. 在Cloud Manager中配置[前端管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html)以部署前端代码。
1. 运行前端管道将更新部署到目标AEM环境。

### 示例存储库

提供了几个可用作参考的GitHub存储库示例：

* [aem-site-template-standard](https://github.com/adobe/aem-site-template-standard)
* [aem-site-template-basic-theme-e2e](https://github.com/adobe/aem-site-template-basic-theme-e2e) — 用作“实际”项目的示例。

## 恭喜！ {#congratulations}

恭喜，您刚刚更新了一个主题并将其部署到AEM！

### 后续步骤 {#next-steps}

通过使用[AEM项目原型](../project-archetype/overview.md)创建站点，更深入地了解AEM开发并了解更多底层技术。
