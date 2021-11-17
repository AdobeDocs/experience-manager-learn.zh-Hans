---
title: 主题工作流 | AEM快速网站创建
description: 了解如何更新Adobe Experience Manager网站的主题源以应用特定于品牌的样式。 了解如何使用代理服务器查看CSS和Javascript更新的实时预览。 本教程还将介绍如何使用AEM Cloud Manager的前端管道将主题更新部署到Adobe站点。
sub-product: sites
version: Cloud Service
type: Tutorial
feature: Core Components
topic: Content Management, Development
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
exl-id: 98946462-1536-45f9-94e2-9bc5d41902d4
source-git-commit: 04096fe3c99cdcce2d43b2b29899c2bbe37ac056
workflow-type: tm+mt
source-wordcount: '515'
ht-degree: 0%

---

# 主题工作流 {#theming}

>[!CAUTION]
>
> 快速网站创建工具当前为技术预览。 它可用于测试和评估目的，并且除非与Adobe支持部门达成协议，否则不会用于生产。

在本章中，我们将更新Adobe Experience Manager网站的主题源，以应用特定于品牌的样式。 我们将了解在针对实时站点编码时，如何使用代理服务器查看CSS和Javascript更新的预览。 本教程还将介绍如何使用AEM Cloud Manager的前端管道将主题更新部署到Adobe站点。

最后，我们的网站将进行更新以包含与WKND品牌匹配的样式。

## 前提条件 {#prerequisites}

这是一个多部分教程，我们假定在 [页面模板](./page-templates.md) 章节已完成。

## 目标

1. 了解如何下载和修改网站的主题源。
1. 了解如何针对实时网站编码以进行实时预览。
1. 了解使用Adobe Cloud Manager的前端管道将编译的CSS和JavaScript更新作为主题一部分的端到端工作流。

## 更新主题 {#theme-update}

接下来，对主题源进行更改，以便网站与WKND品牌的外观相匹配。

>[!VIDEO](https://video.tv.adobe.com/v/332918/?quality=12&learn=on)

视频的高级步骤：

1. 在AEM中创建本地用户以与代理开发服务器一起使用。
1. 从AEM下载主题源，然后使用本地IDE（如VSCode）打开。
1. 修改主题源，并使用代理开发服务器实时预览CSS和JavaScript更改。
1. 更新主题源，以便杂志文章与WKND品牌风格和模型相匹配。

### 解决方案文件

下载的已完成样式 [WKND示例主题](assets/theming/WKND-THEME-src-1.1.zip)

## 使用前端管道部署主题 {#deploy-theme}

使用Cloud Manager的前端管道将主题更新部署到AEM环境。

>[!VIDEO](https://video.tv.adobe.com/v/338722/?quality=12&learn=on)

视频的高级步骤：

1. 创建新git [Cloud Manager中的存储库](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/managing-code/cloud-manager-repositories.html)
1. 将主题源项目添加到Cloud Manager git存储库：

   ```shell
   $ cd <PATH_TO_THEME_SOURCES_FOLDER>
   $ git init -b main
   $ git add .
   $ git commit -m "initial commit"
   $ git remote add origin <CLOUD_MANAGER_GIT_REPOSITORY_URL>
   ```

1. 配置 [前端管线](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html) 以部署前端代码。
1. 运行前端管道以部署目标AEM环境的更新。

### 示例repo

有几个示例GitHub repo可用作引用：

* [aem-site-template-standard](https://github.com/adobe/aem-site-template-standard)
* [aem-site-template-basic-theme-e2e](https://github.com/adobe/aem-site-template-basic-theme-e2e)  — 用作“真实世界”项目的示例。

## 恭喜！ {#congratulations}

恭喜，您刚刚创建并部署了一个主题到AEM!

### 后续步骤 {#next-steps}

深入了解AEM开发，并通过使用 [AEM项目原型](../project-archetype/overview.md).
