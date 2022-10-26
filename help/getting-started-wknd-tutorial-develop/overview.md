---
title: AEM Sites入门 — WKND教程
description: 了解如何为名为WKND的虚构生活方式品牌实施AEM网站。 逐步了解基本的Experience Manager主题，如项目设置、Maven原型、核心组件、可编辑模板、客户端库和组件开发。
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

# AEM Sites入门 — WKND教程 {#introduction}

欢迎参加为初次使用Adobe Experience Manager(AEM)的开发人员而设计的多部分教程。 本教程将指导您实施一个AEM网站，以打造一个虚构的生活方式品牌WKND。 此教程涵盖了项目设置、核心组件、可编辑模板、客户端库和使用 Adobe Experience Manager Sites 进行组件开发等基本主题。

## 概述 {#wknd-tutorial-overview}

此多部分教程的目标是向开发人员讲授如何在Adobe Experience Manager(AEM)中使用最新标准和技术实施网站。 完成本教程后，开发人员应该了解该平台的基本基础以及AEM中的常见设计模式。

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

## 用于启动站点项目的选项

启动AEM Sites项目有两种基本方法。

**AEM项目原型**  — 通过使用Maven模板生成最小的AEM项目，采用传统的AEM开发方法。 对于预计会进行大量自定义的AEM 6.5/6.4项目和AEMas a Cloud Service项目，建议使用此方法。 本教程可让您更深入地了解AEM开发。

[使用AEM项目原型开始教程](./project-archetype/overview.md)

**AEM网站模板**  — 也称为快速网站创建，是一种使用预定义网站模板生成AEM网站的低代码方法。 使用开箱即用的组件和模板，快速启动并运行网站。 使用主题工作流仅通过CSS和JavaScript应用品牌特定的样式和自定义。 建议新项目和开发人员使用。 仅适用于AEMas a Cloud Service。

[使用网站模板启动教程](./site-template/create-site.md)

## Adobe XD UI Kit

为了使本教程更接近真实场景，Adobe才华出众的UX设计人员使用 [Adobe XD](https://www.adobe.com/products/xd.html). 在本教程中，各种设计都被实施到一个完全可创作的AEM网站中。 特别感谢 **洛伦佐·布奥西** 和 **基利安·阿门多拉** 为WKND站点创造了美丽的设计。

下载XD UI工具包：

* [AEM核心组件UI套件](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [WKND UI套件](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

## 引用站点 {#reference-site}

WKND站点的完成版本也可作为参考： [https://wknd.site/](https://wknd.site/)

本教程涵盖AEM开发人员所需的主要开发技能，但将会 *not* 端到端构建整个站点。 完成的引用站点是探索和查看更多开箱即用功能的又一大资源。

要在跳入教程之前测试最新代码，请下载并安装 **[GitHub的最新版本](https://github.com/adobe/aem-guides-wknd/releases/latest)**.

### 由Adobe Stock提供支持

WKND参考网站中的许多图像都来自 [Adobe Stock](https://stock.adobe.com/) 和是演示资产附加条款中定义的第三方材料， [https://www.adobe.com/legal/terms.html](https://www.adobe.com/cn/legal/terms.html). 如果您想将Adobe Stock图像用于除查看此演示网站之外的其他目的，例如在网站上显示该图像，或在营销材料中，则可以在Adobe Stock上购买许可证。

借助Adobe Stock，您可以访问超过1.4亿张高品质、免版税的图像，包括照片、图形、视频和模板，以快速启动您的创意项目。

## 后续步骤 {#next-steps}

你在等什么?!了解如何 [使用AEM项目原型生成新的Adobe Experience Manager项目](./project-archetype/overview.md) 或 [使用网站模板创建网站](./site-template/create-site.md).
