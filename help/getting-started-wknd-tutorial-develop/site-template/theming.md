---
title: 主题工作流
seo-title: AEM Sites入门 — 主题工作流程
description: 了解如何更新Adobe Experience Manager站点的主题源以应用品牌特定的样式。 了解如何使用代理服务器视图CSS和Javascript更新的实时预览。 本教程还将介绍如何使用GitHub Actions将主题更新部署到AEM站点。
sub-product: 站点
version: Cloud Service
type: Tutorial
feature: 核心组件
topic: 内容管理，开发
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
translation-type: tm+mt
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '581'
ht-degree: 0%

---


# 主题工作流{#theming}

>[!CAUTION]
>
> 此处展示的快速站点创建功能将在2021年下半年发布。 相关文档可供预览使用。

在本章中，我们将更新Adobe Experience Manager站点的主题源以应用品牌特定的样式。 我们将学习如何使用代理服务器视图CSS和Javascript更新的预览，当我们针对实时站点编码时。 本教程还将介绍如何使用GitHub Actions将主题更新部署到AEM站点。

最后，我们的站点将更新以包含与WKND品牌匹配的样式。

## 前提条件 {#prerequisites}

这是一个多部分教程，假定已完成[页面模板](./page-templates.md)一章中概述的步骤。

## 目标

1. 了解如何下载和修改站点的主题源。
1. 了解如何针对实时站点编写代码以实现实时预览。
1. 了解使用GitHub Actions将编译的CSS和JavaScript更新作为主题的一部分交付的端到端工作流程。

## 更新主题{#theme-update}

接下来，对主题源进行更改，以使站点与WKND品牌的外观相匹配。

>[!VIDEO](https://video.tv.adobe.com/v/332918/?quality=12&learn=on)

视频的高级步骤：

1. 在AEM中创建一个本地用户，以便与代理开发服务器一起使用。
1. 从AEM下载主题源，然后使用本地IDE（如VSCode）打开。
1. 修改主题源并使用代理开发服务器实时预览CSS和JavaScript更改。
1. 更新主题源，使杂志文章与WKND品牌样式和模型匹配。

### 解决方案文件

下载[WKND主题](assets/theming/WKND-THEME-src.zip)的已完成样式

## 部署主题{#deploy-theme}

使用GitHub Actions将主题更新部署到AEM环境。

>[!VIDEO](https://video.tv.adobe.com/v/332919/?quality=12&learn=on)

视频的高级步骤：

1. 将您的主题源项目添加到[GitHub作为新存储库](https://docs.github.com/en/github/importing-your-projects-to-github/adding-an-existing-project-to-github-using-the-command-line)。
1. 在GitHub中创建[个人访问令牌并保存到安全位置。](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token)
1. 在您的AEM环境中配置GitHub设置以指向您的GitHub存储库并包含您的个人访问令牌。
1. 在您的GitHub存储库中创建以下[加密机密](https://docs.github.com/en/actions/reference/encrypted-secrets):
   * **AEM_SITE** - AEM站点的根(即 `wknd`)
   * **AEM_URL** - AEM作者环境的url
   * **GIT_TOKEN**  — 步骤2中的个人访问令牌。
1. 运行GitHub操作：**构建和部署Github对象**。 这将产生运行操作的下游效果：**更新AEM上的主题配置（对象id为**），它将主题源部署到由`AEM_URL`和`AEM_SITE`指定的AEM环境。

### 重订示例

有几个GitHub错误可用作参考：

* [aem-site-template-basic-theme-e2e](https://github.com/adobe/aem-site-template-basic-theme-e2e)  — 用作“real-world”项目的示例。
* [https://github.com/godanny86/wknd-theme](https://github.com/godanny86/wknd-theme)  — 用作教程后面的示例。

## 恭喜！{#congratulations}

祝贺您，您刚刚创建并更新了主题，将主题部署到AEM!

### 后续步骤{#next-steps}

使用[AEM Project Archetype](../project-archetype/overview.md)创建站点，更深入地了解AEM开发并了解更多底层技术。
