---
title: 页面模板
seo-title: AEM Sites入门 — 页面模板
description: 了解如何创建和修改页面模板。 了解页面模板与页面之间的关系。 了解如何配置页面模板的策略，以便为内容提供精细的管理和品牌一致性。  将根据Adobe XD的模型创建结构良好的杂志文章模板。
sub-product: 站点
version: Cloud Service
type: Tutorial
topic: 内容管理
feature: 核心组件、可编辑的模板、页面编辑器
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '767'
ht-degree: 3%

---


# 页面模板{#page-templates}

>[!CAUTION]
>
> 此处展示的快速网站创建功能将于2021年下半年发布。 相关文档可供预览。

在本章中，我们将探讨页面模板与页面之间的关系。 我们将根据[AdobeXD](https://www.adobe.com/products/xd.html)中的某些模型构建未设置样式的杂志文章模板。 在构建模板过程中，将介绍核心组件和高级策略配置。

## 前提条件 {#prerequisites}

这是一个多部分教程，假定已完成[创作内容和发布更改](./author-content-publish.md)章节中概述的步骤。

## 目标

1. Inspect在Adobe XD中创建的页面设计，并将其映射到核心组件。
1. 了解页面模板的详细信息以及如何使用策略来强制对页面内容进行精细控制。
1. 了解模板和页面的关联方式。

## 将生成{#what-you-will-build}的内容

在本教程的这一部分中，您将构建新的杂志文章页面模板，该模板可用于创建新的杂志文章，并与通用结构保持一致。 模板将基于AdobeXD中生成的设计和UI Kit。 本章仅侧重于构建模板的结构或框架。 将不会实施样式，但模板和页面将可正常工作。

## 使用Adobe XD规划UI {#adobexd}

在大多数情况下，首先会规划新网站的模型和静态设计。 [Adobe](https://www.adobe.com/products/xd.html) XD是构建用户体验的设计工具。接下来，我们将检查UI工具包和模型，以帮助规划文章页面模板的结构。

>[!VIDEO](https://video.tv.adobe.com/v/30214/?quality=12&learn=on)

**下载WKND [文章设计文件](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)**。

>[!NOTE]
>
> [AEM核心组件UI Kit还可用作自定义项目的起点](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd)。

## 创建杂志文章页面模板

创建页面时，您必须选择一个模板，以用作创建新页面的基础。模板可定义生成页面的结构、初始内容和允许的组件。

[页面模板](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/templates.html)有3个主要区域：

1. **结构**  — 定义属于模板一部分的组件。内容作者将无法编辑这些内容。
1. **初始内容**  — 定义模板将以开头的组件，内容作者可以编辑和/或删除这些组件
1. **策略**  — 定义有关组件的行为方式以及作者将可以使用的选项的配置。

接下来，在AEM中创建与模型结构匹配的新模板。 这将在AEM的本地实例中发生。 按照以下视频中的步骤操作：

>[!VIDEO](https://video.tv.adobe.com/v/332915/?quality=12&learn=on)

### 解决方案包

可以通过包管理器下载和安装杂志模板](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip)的已完成[解决方案。

## 使用体验片段{#experience-fragments}更新页眉和页脚

创建全局内容（如页眉或页脚）时的常见做法是使用[体验片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html)。 体验片段，允许用户合并多个组件以创建一个可引用的组件。 体验片段具有支持多站点管理和[本地化](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure)的优势。

网站模板会生成页眉和页脚。 接下来，更新体验片段以匹配模型。 按照以下视频中的步骤操作：

>[!VIDEO](https://video.tv.adobe.com/v/332916/?quality=12&learn=on)

以下视频的高级步骤：

1. 下载示例内容包&#x200B;**[WKND-Starter-Assets-Skate-Article-1.0.zip](assets/page-templates/WKND-Starter-Assets-Skate-Article-1.0.zip)**。
1. 使用包管理器上载并安装内容包。
1. 更新页眉和页脚体验片段以使用WKND徽标

## 创建杂志文章页面

接下来，使用杂志文章页面模板创建新页面。 创作页面内容以匹配网站模型。 按照以下视频中的步骤操作：

>[!VIDEO](https://video.tv.adobe.com/v/332917/?quality=12&learn=on)

## 恭喜！ {#congratulations}

恭喜，您刚刚使用Adobe Experience Manager Sites创建了新模板和页面。

### 后续步骤{#next-steps}

此时，杂志文章页面和网站与WKND的品牌样式不匹配。 请按照[主题](theming.md)教程，了解更新CSS和Javascript前端代码的最佳实践，这些代码用于将全局样式应用到站点。

### 解决方案包

可下载本章的解决方案包：[WKND-Magazine-Template-SOLUTION-1.0.zip](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip)。
