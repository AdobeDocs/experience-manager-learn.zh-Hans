---
title: 使用Adobe Target实现个性化
seo-title: 使用Adobe Target实现个性化
description: 一个端到端教程，其中显示了如何使用Adobe Target创建和提供个性化体验。
seo-description: 一个端到端教程，其中显示了如何使用Adobe Target创建和提供个性化体验。
feature: 体验片段
topic: 个性化
role: 开发人员
level: 中间
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '611'
ht-degree: 3%

---


# 使用Adobe Target个性化完整网页体验

在上一章中，我们学习了如何在Adobe Target内使用创建为体验片段并从AEM导出为HTML活动的内容创建基于地理位置的优惠。

在本章中，我们将探索创建活动，以将AEM上托管的站点页面重定向到使用Adobe Target的新页面。

## 方案概述

WKND站点重新设计了主页，并希望将其当前主页访客重定向到新主页。 同时，还要了解重新设计的主页如何帮助提高用户参与度和收入。 作为营销人员，您已分配任务来创建活动，以将访客重定向到新主页。 让我们浏览WKND站点主页并了解如何使用Adobe Target创建活动。

### 涉及的用户

对于本练习，需要参与以下用户，并执行某些任务，您可能需要管理访问权限。

* **Content Producer/Content Editor** (Adobe Experience Manager)
* **营销人** (Adobe Target/优化团队)

### WKND站点主页

![AEM 目标方案1](assets/personalization-use-case-2/aem-target-use-case-2.png)

### 前提条件

* **AEM**
   * [AEM author和publish ](./implementation.md#getting-aem) instancerunning分别位于localhost 4502和4503上。
   * [AEM与Adobe Target集成使用Adobe Experience Platform Launch](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * 访问您的组织Adobe Experience Cloud - <https://>`<yourcompany>`.experiencecloud.adobe.com
   * Experience Cloud配置了以下解决方案
      * [Adobe Target](https://experiencecloud.adobe.com)

## 内容编辑器活动

1. 营销人员与AEM内容编辑器一起发起WKND主页重新设计讨论，并详细说明相关要求。
   * ***要求*** :借助基于卡的设计重新设计WKND站点主页。
2. 然后，根据要求，AEM Content Editor使用基于卡的设计创建新的WKND站点主页并发布新主页。

## 营销人员活动

1. 营销人员创建A/B目标活动，将重定向优惠作为体验，并在添加成功目标和量度的情况下，为新主页分配100%的网站流量。
   1. 在Adobe Target窗口中，导航到&#x200B;**活动**&#x200B;选项卡。
   2. 单击&#x200B;**创建活动**&#x200B;按钮，然后选择活动类型作为&#x200B;**A/B测试**

      ![Adobe Target — 创建活动](assets/personalization-use-case-2/create-ab-activity.png)
   3. 选择&#x200B;**Web**&#x200B;渠道，然后选择&#x200B;**Visual Experience Composer**。
   4. 输入&#x200B;**活动URL**，然后单击&#x200B;**下一步**以打开可视体验书写器。
      ![Adobe Target — 创建活动](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. 要加载&#x200B;**Visual Experience Composer**，请在浏览器上启用&#x200B;**允许加载不安全脚本**并重新加载页面。
      ![体验定位活动](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. 请注意，WKND站点主页在Visual Experience Composer编辑器中打开。
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. 将鼠标悬停在&#x200B;**体验B**上，然后选择视图其他选项。
      ![体验 B](assets/personalization-use-case-2/redirect-url.png)
   8. 选择&#x200B;**“重定向到URL**”选项并输入指向新WKND主页的URL。 (http://localhost:4503/content/wknd/en1.html)
      ![体验 B](assets/personalization-use-case-2/redirect-url-2.png)
   9. **保** 存更改并继续执行活动创建的后续步骤。
   10. 选择&#x200B;**流量分配方法**&#x200B;作为手动流量，并允许100%流量到&#x200B;**体验B**。
      ![体验B流量](assets/personalization-use-case-2/traffic.png)
   11. 单击&#x200B;**下一步**。
   12. 为您的活动提供&#x200B;**目标量度**并保存和关闭A/B测试。
      ![A/B测试目标量度](assets/personalization-use-case-2/goal-metric.png)
   13. 为活动提供名称(**WKND主页重新设计**)并保存更改。
   14. 在活动详细信息屏幕中，确保&#x200B;**激活**您的活动。
      ![激活活动](assets/personalization-use-case-2/ab-activate.png)
   15. 导航到WKND主页(http://localhost:4503/content/wknd/en.html)，您将被重定向到重新设计的WKND站点主页(http://localhost:4503/content/wknd/en1.html)。
      ![WKND主页已重新设计](assets/personalization-use-case-2/WKND-home-page-redesign.png)

## 摘要

在本章中，营销人员能够创建活动，将AEM上托管的网站页面重定向到使用Adobe Target的新页面。
