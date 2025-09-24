---
title: 页面模板
description: 了解如何创建和更改页面模板。了解页面模板与页面之间的关系。了解如何配置页面模板的策略，以提供内容的细粒度治理和品牌一致性。基于 Adobe XD 中的模型创建一个结构良好的杂志文章模板。
version: Experience Manager as a Cloud Service
topic: Content Management
feature: Core Components, Editable Templates, Page Editor
role: Developer
level: Beginner
jira: KT-7498
thumbnail: KT-7498.jpg
doc-type: Tutorial
exl-id: 261ec68f-36f4-474f-a6e4-7a2f9cea691b
recommendations: noDisplay, noCatalog
duration: 1561
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '628'
ht-degree: 100%

---

# 页面模板 {#page-templates}

在本章中，我们将探讨页面模板与页面之间的关系。我们将根据 [AdobeXD](https://www.adobe.com/cn/products/xd.html) 中的一些模型构建一个无样式的杂志文章模板。构建模板的过程中涵盖了核心组件和高级策略配置。

## 先决条件 {#prerequisites}

这是一个多段式教程，并假设您已完成[作者内容和发布更改](./author-content-publish.md)一章中概述的步骤。

## 目标

1. 了解页面模板的详细信息以及如何使用策略来强制对页面内容的细粒度控制。
1. 了解模板与页面如何关联。
1. 创建一个新模板，然后创作一个页面。

## 您将构建什么 {#what-you-will-build}

在本教程的这一节，您将构建一个新的杂志文章页面模板，该模板可用于创建新的杂志文章，并采用一个常用结构。该模板基于 AdobeXD 中制作的设计和 UI 套件。本章仅重点关注构建模板的结构或框架。没有实施任何样式，但模板和页面可以正常工作。

## 创建杂志文章页面模板

创建页面时，您必须选择一个模板，以用作创建新页面的基础。模板定义了所生成页面的结构、初始内容以及允许使用的组件。

[页面模板](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/templates.html?lang=zh-hans)主要包含 3 个区域：

1. **结构** - 定义了作为模板一部分的各种组件。内容作者无法编辑这些组件。
1. **初始内容** - 定义了模板开始时使用的组件，内容作者可以编辑和/或删除这些组件
1. **策略** - 定义了关于组件行为方式以及作者将具有哪些选项的配置。

接下来，在 AEM 中创建一个与模型的结构相匹配的新模板。这在 AEM 的本地实例中完成。按照下面视频中的步骤操作：

>[!VIDEO](https://video.tv.adobe.com/v/332915?quality=12&learn=on)

您可以使用以下缩略图来识别您的模板（或上传您自己的模板！）

![文章页面模板缩略图](./assets/page-templates/article-page-template-thumbnail.png)


### 解决方案包

您可以通过包管理器下载并安装一个已经完成的[杂志模板解决方案](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.1.zip)。

## 通过体验片段更新页眉和页脚 {#experience-fragments}

创建页眉或页脚等全局内容时的一种常见做法是使用[体验片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=zh-Hans)。通过体验片段，用户可以组合多个组件来创建一个可引用的组件。体验片段的优势是支持多网站管理和[本地化](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=zh-hans#localized-site-structure)。

网站模板生成了页眉和页脚。接下来，更新体验片段以匹配模型。按照下面视频中的步骤操作：

>[!VIDEO](https://video.tv.adobe.com/v/332916?quality=12&learn=on)

下面视频的高级概述步骤：

1. 下载示例内容包 **[WKND-Starter-Assets-Skate-Article-1.2.zip](assets/page-templates/WKND-Starter-Assets-Skate-Article-1.2.zip)**。
1. 使用包管理器上传并安装内容包。
1. 更新页眉和页脚体验片段，以使用 WKND 徽标

## 创建杂志文章页面

接下来，使用杂志文章页面模板创建一个新页面。创作页面内容，以匹配网站模型。按照下面视频中的步骤操作：

>[!VIDEO](https://video.tv.adobe.com/v/332917?quality=12&learn=on)

使用[提供的文本](./assets/page-templates/la-skateparks-copy.txt)填充文章正文。

## 恭喜！ {#congratulations}

祝贺您使用 Adobe Experience Manager Sites 创建了一个新的模板和页面。

### 后续步骤 {#next-steps}

现在，杂志文章页面和网站并不符合 WKND 的品牌样式。学习[主题化](theming.md)教程，了解如何更新 CSS 和 Javascript 前端代码，将全局样式应用于网站的最佳做法。

### 解决方案包

本章的解决方案包可供下载：[WKND-Magazine-Template-SOLUTION-1.0.zip](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip)。
