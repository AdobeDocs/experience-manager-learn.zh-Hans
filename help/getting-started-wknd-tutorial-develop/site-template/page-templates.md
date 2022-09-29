---
title: 页面模板
description: 了解如何创建和修改页面模板。 了解页面模板与页面之间的关系。 了解如何配置页面模板的策略，以便为内容提供精细的管理和品牌一致性。  基于Adobe XD的模型，创建了结构良好的杂志文章模板。
sub-product: sites
version: Cloud Service
type: Tutorial
topic: Content Management
feature: Core Components, Editable Templates, Page Editor
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
exl-id: 261ec68f-36f4-474f-a6e4-7a2f9cea691b
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '652'
ht-degree: 1%

---

# 页面模板 {#page-templates}

在本章中，我们将探讨页面模板与页面之间的关系。 我们将根据 [AdobeXD](https://www.adobe.com/products/xd.html). 在构建模板过程中，将介绍核心组件和高级策略配置。

## 前提条件 {#prerequisites}

这是一个多部分教程，我们假定在 [创作内容和发布更改](./author-content-publish.md) 章节已完成。

## 目标

1. 了解页面模板的详细信息以及如何使用策略来强制对页面内容进行精细控制。
1. 了解模板和页面的关联方式。
1. 创建新模板并创作页面。

## 将构建的内容 {#what-you-will-build}

在本教程的这一部分中，您将构建新的杂志文章页面模板，该模板可用于创建新的杂志文章，并与通用结构保持一致。 模板基于AdobeXD中生成的设计和UI Kit。 本章仅侧重于构建模板的结构或框架。 未实施样式，但模板和页面可正常工作。

## 创建杂志文章页面模板

创建页面时，必须选择模板，以用作创建新页面的基础。 模板可定义生成页面的结构、初始内容和允许的组件。

有3个主要区域 [页面模板](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/templates.html):

1. **结构**  — 定义属于模板一部分的组件。 内容作者无法编辑这些内容。
1. **初始内容**  — 定义模板开头的组件，内容作者可以编辑和/或删除这些组件
1. **策略**  — 定义有关组件行为方式以及作者将拥有哪些选项的配置。

接下来，在AEM中创建与模型结构匹配的新模板。 这将在AEM的本地实例中发生。 按照以下视频中的步骤操作：

>[!VIDEO](https://video.tv.adobe.com/v/332915/?quality=12&learn=on)

您可以使用以下缩略图来标识模板（或上传您自己的模板！）

![文章页面模板缩略图](./assets/page-templates/article-page-template-thumbnail.png)


### 解决方案包

已完成 [杂志模板解决方案](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.1.zip) 可以通过包管理器下载和安装。

## 使用体验片段更新页眉和页脚 {#experience-fragments}

创建全局内容（如页眉或页脚）时，通常使用 [体验片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html). 体验片段，允许用户合并多个组件以创建一个可引用的组件。 体验片段具有支持多站点管理和 [本地化](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure).

网站模板会生成页眉和页脚。 接下来，更新体验片段以匹配模型。 按照以下视频中的步骤操作：

>[!VIDEO](https://video.tv.adobe.com/v/332916/?quality=12&learn=on)

以下视频的高级步骤：

1. 下载示例内容包 **[WKND-Starter-Assets-Skate-Article-1.2.zip](assets/page-templates/WKND-Starter-Assets-Skate-Article-1.2.zip)**.
1. 使用包管理器上载并安装内容包。
1. 更新页眉和页脚体验片段以使用WKND徽标

## 创建杂志文章页面

接下来，使用杂志文章页面模板创建新页面。 创作页面内容以匹配网站模型。 按照以下视频中的步骤操作：

>[!VIDEO](https://video.tv.adobe.com/v/332917/?quality=12&learn=on)

使用 [提供的文本](./assets/page-templates/la-skateparks-copy.txt) 填充文章正文。

## 恭喜！ {#congratulations}

恭喜，您刚刚使用Adobe Experience Manager Sites创建了新模板和页面。

### 后续步骤 {#next-steps}

此时，杂志文章页面和网站与WKND的品牌样式不匹配。 关注 [天明](theming.md) 本教程介绍了更新CSS和Javascript前端代码的最佳实践，这些代码用于将全局样式应用到网站。

### 解决方案包

可下载本章的解决方案包： [WKND-Magazine-Template-SOLUTION-1.0.zip](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip).
