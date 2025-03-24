---
title: AEM Sites - WKND教程快速入门
description: 了解如何为一个名为WKND的虚构生活方式品牌实施AEM网站。 获取有关Experience Manager基本主题的演练，如项目设置、maven原型、核心组件、可编辑模板、客户端库和组件开发。
version: Experience Manager as a Cloud Service
jira: KT-13565
mini-toc-levels: 1
index: y
thumbnail: 30476.jpg
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
doc-type: Catalog
exl-id: 09a600f4-1ada-4fb7-ae44-586364cff389
recommendations: disable
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '577'
ht-degree: 5%

---

# AEM Sites - WKND教程快速入门 {#introduction}

{{edge-delivery-services}}

欢迎使用专为不熟悉Adobe Experience Manager (AEM)的开发人员设计的多部分教程。 本教程介绍了虚拟生活方式品牌WKND的AEM站点的实施。 此教程涵盖了项目设置、核心组件、可编辑模板、客户端库和使用 Adobe Experience Manager Site 进行组件开发等基本主题。

## 概述 {#wknd-tutorial-overview}

此多部分教程旨在教导开发人员如何使用Adobe Experience Manager (AEM)中的最新标准和技术实施网站。 完成本教程后，开发人员应该了解该平台的基本基础和AEM中的常见设计模式。

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

## 用于启动站点项目的选项

启动AEM Sites项目有两种基本方法。

**AEM项目原型** — 通过使用Maven模板生成最小的AEM项目来开发AEM的传统方法。 这是推荐的方法用于预期大量自定义的AEM 6.5/6.4项目和AEM as a Cloud Service项目。 本教程提供了对AEM开发的更深入探讨。

[开始使用AEM项目原型的教程](./project-archetype/overview.md)

**AEM站点模板** — 也称为快速站点创建，这是一种使用预定义站点模板生成AEM站点的低代码方法。 使用开箱即用的组件和模板快速启动和运行站点。 使用主题化工作流，仅通过CSS和JavaScript应用品牌特定的样式和自定义设置。 为新项目和开发人员推荐。 仅适用于AEM as a Cloud Service。

[使用站点模板启动教程](./site-template/create-site.md)

## Adobe XD UI套件

为了使本教程更接近真实场景，Adobe才华横溢的UX设计人员使用[Adobe XD](https://www.adobe.com/products/xd.html)为站点创建了模型。 在本教程中，会将各种设计片段实施到可完全创作的AEM站点中。 特别感谢&#x200B;**Lorenzo Buosi**&#x200B;和&#x200B;**Kilian Amendola**，他们为WKND网站创造了美观的设计。

下载XD UI包：

* [AEM核心组件UI套件](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [WKND UI套件](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

## 引用站点 {#reference-site}

WKND站点的完成版本也可用作参考： [https://wknd.site/](https://wknd.site/)

本教程涵盖AEM开发人员所需的主要开发技能，但&#x200B;*不会*&#x200B;端到端地构建整个站点。 已完成的引用站点是另一个探索和查看更多AEM开箱即用功能的有用资源。

若要在进入教程之前测试最新代码，请从GitHub下载并安装&#x200B;**[最新版本](https://github.com/adobe/aem-guides-wknd/releases/latest)**。

### 由Adobe Stock提供支持

WKND参考网站中的许多图像来自[Adobe Stock](https://stock.adobe.com/)，并且是演示资产附加条款[https://www.adobe.com/legal/terms.html](https://www.adobe.com/cn/legal/terms.html)中定义的第三方材料。 如果您要将Adobe Stock图像用于查看此演示网站以外的其他目的，例如将其显示在网站上或营销材料中，则可以在Adobe Stock上购买许可证。

借助Adobe Stock，您可以访问超过1.4亿张高质量、免版税的图像，包括照片、图形、视频和模板，从而快速启动您的创意项目。

## 后续步骤 {#next-steps}

你在等什么?!了解如何[使用Adobe Experience Manager项目原型](./project-archetype/overview.md)生成新的AEM项目，或[使用站点模板](./site-template/create-site.md)创建站点。
