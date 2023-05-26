---
title: AEM Sites - WKND教程快速入门
description: 了解如何为名为WKND的虚构生活方式品牌实施AEM站点。 获取有关基本Experience Manager主题的演练，如项目设置、maven原型、核心组件、可编辑模板、客户端库和组件开发。
topics: development
version: Cloud Service
activity: develop
audience: developer
KT: 4132
mini-toc-levels: 1
index: y
thumbnail: 30476.jpg
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
exl-id: 09a600f4-1ada-4fb7-ae44-586364cff389
recommendations: disable
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '599'
ht-degree: 7%

---

# AEM Sites - WKND教程快速入门 {#introduction}

欢迎使用专为不熟悉Adobe Experience Manager (AEM)的开发人员设计的多部分教程。 本教程介绍了虚拟生活方式品牌WKND的AEM站点的实施。 此教程涵盖了项目设置、核心组件、可编辑模板、客户端库和使用 Adobe Experience Manager Sites 进行组件开发等基本主题。

## 概述 {#wknd-tutorial-overview}

本多部分教程的目标是教导开发人员如何使用Adobe Experience Manager (AEM)中的最新标准和技术来实施网站。 完成本教程后，开发人员应了解该平台的基本基础和AEM中的常见设计模式。

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

## 用于启动站点项目的选项

启动AEM Sites项目有两种基本方法。

**AEM项目原型**  — 传统的开发方式，通过使用Maven模板生成最小的AEMAEM项目。 这是推荐的方法用于AEM 6.5/6.4项目和AEMas a Cloud Service项目，这些项目预计要进行大量自定义。 本教程提供了对AEM开发的更深入探讨。

[开始教程，了解AEM项目原型](./project-archetype/overview.md)

**AEM站点模板**  — 也称为快速站点创建，这是一种使用预定义站点模板来生成AEM站点的低代码方法。 使用现成的组件和模板快速启动并运行站点。 使用主题化工作流，仅通过CSS和JavaScript应用品牌特定的样式和自定义项。 建议新项目和开发人员使用。 仅适用于AEMas a Cloud Service。

[使用站点模板开始教程](./site-template/create-site.md)

## Adobe XD UI套件

为了使本教程更接近真实场景，Adobe的天才UX设计人员使用为站点创建模型 [Adobe XD](https://www.adobe.com/products/xd.html). 在本教程中，各种设计都实施到了完全可创作的AEM站点中。 特别感谢 **洛伦佐·布瓦** 和 **基利安·阿门多拉** 创造了WKND网站的美丽设计

下载XD UI包：

* [AEM核心组件UI套件](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [WKND UI套件](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

## 引用站点 {#reference-site}

WKND站点的完成版本也可用作参考： [https://wknd.site/](https://wknd.site/)

本教程涵盖AEM开发人员所需的主要开发技能，但将 *非* 端到端地构建整个站点。 已完成的引用站点是另一个探索和查看更多AEM开箱即用功能的绝佳资源。

要在进入教程之前测试最新的代码，请下载并安装 **[GitHub的最新版本](https://github.com/adobe/aem-guides-wknd/releases/latest)**.

### 由Adobe Stock提供支持

WKND参考网站中的许多图像来自 [Adobe Stock](https://stock.adobe.com/) 属于演示资产中定义的第三方材料，其他条款位于 [https://www.adobe.com/legal/terms.html](https://www.adobe.com/cn/legal/terms.html). 如果您希望将Adobe Stock图像用于查看此演示网站以外的其他目的，例如将其显示在网站上或营销材料中，则可以在Adobe Stock上购买许可证。

借助Adobe Stock，您可以访问超过1.4亿张高质量、免版税的图像（包括照片、图形、视频和模板），以快速启动您的创意项目。

## 后续步骤 {#next-steps}

你在等什么?!了解如何 [使用AEM项目原型生成新的Adobe Experience Manager项目](./project-archetype/overview.md) 或 [使用站点模板创建站点](./site-template/create-site.md).
