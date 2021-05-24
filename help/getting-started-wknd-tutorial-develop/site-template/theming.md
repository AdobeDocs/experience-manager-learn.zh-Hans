---
title: 主题工作流
seo-title: AEM Sites入门 — 主题工作流
description: 了解如何更新Adobe Experience Manager网站的主题源以应用特定于品牌的样式。 了解如何使用代理服务器查看CSS和Javascript更新的实时预览。 本教程还将介绍如何使用GitHub操作将主题更新部署到AEM网站。
sub-product: 站点
version: Cloud Service
type: Tutorial
feature: 核心组件
topic: 内容管理、开发
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '581'
ht-degree: 0%

---


# 主题工作流 {#theming}

>[!CAUTION]
>
> 此处展示的快速网站创建功能将于2021年下半年发布。 相关文档可供预览。

在本章中，我们将更新Adobe Experience Manager网站的主题源，以应用特定于品牌的样式。 我们将了解在我们针对实时网站编码时，如何使用代理服务器查看CSS和Javascript更新的预览。 本教程还将介绍如何使用GitHub操作将主题更新部署到AEM网站。

最后，我们的网站将进行更新以包含与WKND品牌匹配的样式。

## 前提条件 {#prerequisites}

这是一个多部分教程，假定已完成[页面模板](./page-templates.md)章节中概述的步骤。

## 目标

1. 了解如何下载和修改网站的主题源。
1. 了解如何针对实时网站编码以进行实时预览。
1. 了解使用GitHub操作将编译的CSS和JavaScript更新作为主题一部分交付的端到端工作流。

## 更新主题{#theme-update}

接下来，对主题源进行更改，以便网站与WKND品牌的外观相匹配。

>[!VIDEO](https://video.tv.adobe.com/v/332918/?quality=12&learn=on)

视频的高级步骤：

1. 在AEM中创建本地用户以与代理开发服务器一起使用。
1. 从AEM下载主题源，然后使用本地IDE（如VSCode）打开。
1. 修改主题源，并使用代理开发服务器实时预览CSS和JavaScript更改。
1. 更新主题源，以便杂志文章与WKND品牌风格和模型相匹配。

### 解决方案文件

下载[WKND主题](assets/theming/WKND-THEME-src.zip)的已完成样式

## 部署主题{#deploy-theme}

使用GitHub操作将主题更新部署到AEM环境。

>[!VIDEO](https://video.tv.adobe.com/v/332919/?quality=12&learn=on)

视频的高级步骤：

1. 将主题源项目作为新存储库](https://docs.github.com/en/github/importing-your-projects-to-github/adding-an-existing-project-to-github-using-the-command-line)添加到[GitHub。
1. 在GitHub中创建[个人访问令牌](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token)并保存到安全位置。
1. 在AEM环境中配置GitHub设置，以指向GitHub存储库并包含您的个人访问令牌。
1. 在GitHub存储库中创建以下[加密密钥](https://docs.github.com/en/actions/reference/encrypted-secrets):
   * **AEM_SITE**  - AEM站点的根(即 `wknd`)
   * **AEM_URL**  - AEM创作环境的url
   * **GIT_TOKEN**  — 步骤2中的个人访问令牌。
1. 运行GitHub操作：**生成和部署Github工件**。 这将产生运行操作的下游效果：**使用项目id**&#x200B;更新AEM上的主题配置，该项目将按`AEM_URL`和`AEM_SITE`指定的方式将主题源部署到AEM环境。

### 示例repo

有几个示例GitHub repo可用作引用：

* [aem-site-template-basic-theme-e2e](https://github.com/adobe/aem-site-template-basic-theme-e2e)  — 用作“真实世界”项目的示例。
* [https://github.com/godanny86/wknd-theme](https://github.com/godanny86/wknd-theme)  — 用作下面教程的示例。

## 恭喜！ {#congratulations}

恭喜，您刚刚创建并部署了一个主题到AEM!

### 后续步骤{#next-steps}

深入了解AEM开发，并通过使用[AEM项目原型](../project-archetype/overview.md)创建站点，进一步了解基础技术。
