---
title: 页面模板
description: 了解如何创建和修改页面模板。 了解页面模板与页面之间的关系。 了解如何配置页面模板的策略，以便为内容提供精细的治理和品牌一致性。  基于Adobe XD的模型创建了一个结构良好的杂志文章模板。
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
workflow-type: tm+mt
source-wordcount: '628'
ht-degree: 0%

---

# 页面模板 {#page-templates}

在本章中，我们将探讨页面模板与页面之间的关系。 我们将根据[AdobeXD](https://www.adobe.com/products/xd.html)的一些模型构建一个无样式的杂志文章模板。 在构建模板的过程中，将涵盖核心组件和高级策略配置。

## 先决条件 {#prerequisites}

这是一个多部分教程，并假定已完成[创作内容和发布更改](./author-content-publish.md)章节中概述的步骤。

## 目标

1. 了解页面模板的详细信息，以及如何使用策略对页面内容实施精细控制。
1. 了解如何链接模板和页面。
1. 创建新模板并创作页面。

## 您将构建的内容 {#what-you-will-build}

在本教程的这一可选部分中，您将构建一个新的“杂志文章页面”模板，该模板可用于创建新的杂志文章并与通用结构保持一致。 该模板基于设计和在AdobeXD中生成的UI套件。 本章仅侧重于构建模板的结构或框架。 未实施任何样式，但模板和页面可正常使用。

## 创建杂志文章页面模板

创建页面时，必须选择一个模板，该模板用作创建新页面的基础。 模板定义生成页面的结构、初始内容和允许的组件。

[页面模板](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/templates.html?lang=zh-hans)有3个主要区域：

1. **结构** — 定义模板中的组件。 内容作者无法编辑这些内容。
1. **初始内容** — 定义模板开始的组件，这些组件可由内容作者编辑和/或删除
1. **策略** — 定义有关组件的行为方式以及作者可用的选项的配置。

接下来，在AEM中创建一个与模型结构匹配的新模板。 这将发生在AEM的本地实例中。 按照以下视频中的步骤进行操作：

>[!VIDEO](https://video.tv.adobe.com/v/332915?quality=12&learn=on)

您可以使用以下缩略图来标识您的模板（或上传您自己的模板！）

![文章页面模板缩略图](./assets/page-templates/article-page-template-thumbnail.png)


### 解决方案包

可以通过包管理器下载并安装已完成的杂志模板](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.1.zip)的[解决方案。

## 使用体验片段更新页眉和页脚 {#experience-fragments}

创建全局内容（如页眉或页脚）时的常见做法是使用[体验片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html)。 体验片段，允许用户组合多个组件以创建单个可引用的组件。 体验片段具有支持多站点管理和[本地化](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure)的优势。

站点模板生成了页眉和页脚。 接下来，更新体验片段以匹配模型。 按照以下视频中的步骤进行操作：

>[!VIDEO](https://video.tv.adobe.com/v/332916?quality=12&learn=on)

下面是视频的高级步骤：

1. 下载示例内容包&#x200B;**[WKND-Starter-Assets-Skate-Article-1.2.zip](assets/page-templates/WKND-Starter-Assets-Skate-Article-1.2.zip)**。
1. 使用包管理器上载并安装内容包。
1. 更新页眉和页脚体验片段以使用WKND徽标

## 创建杂志文章页面

接下来，使用“杂志文章页面”模板创建一个新页面。 创作页面的内容以匹配站点模型。 按照以下视频中的步骤进行操作：

>[!VIDEO](https://video.tv.adobe.com/v/332917?quality=12&learn=on)

使用[提供的文本](./assets/page-templates/la-skateparks-copy.txt)填充文章正文。

## 恭喜！ {#congratulations}

恭喜，您刚刚使用Adobe Experience Manager Sites创建了新模板和页面。

### 后续步骤 {#next-steps}

此时，杂志文章页面和站点与WKND的品牌样式不匹配。 按照[主题](theming.md)教程了解更新用于将全局样式应用于站点的CSS和Javascript前端代码的最佳实践。

### 解决方案包

本章的解决方案包可供下载： [WKND-Magazine-Template-SOLUTION-1.0.zip](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip)。
