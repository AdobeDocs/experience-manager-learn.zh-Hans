---
title: 使用Adobe Target进行个性化
seo-title: 使用Adobe Target进行个性化
description: 一个端到端教程，其中演示了如何使用Adobe Target创建和提供个性化体验。
seo-description: 一个端到端教程，其中演示了如何使用Adobe Target创建和提供个性化体验。
feature: 体验片段
topic: 个性化
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '609'
ht-degree: 2%

---


# 使用Adobe Target实现完整网页体验的个性化

在上一章中，我们学习了如何使用作为体验片段创建并从AEM导出为HTML选件的内容，在Adobe Target中创建基于地理位置的活动。

在本章中，我们将探索如何创建活动，以使用Adobe Target将托管在AEM上的网站页面重定向到新页面。

## 方案概述

WKND网站重新设计了其主页，并希望将其当前主页的访客重定向到新主页。 同时，还要了解重新设计的主页如何帮助提高用户参与度和收入。 作为营销人员，您已经分配了创建活动以将访客重定向到新主页的任务。 让我们浏览WKND站点主页，并了解如何使用Adobe Target创建活动。

### 涉及的用户

在本练习中，需要涉及以下用户，并且要执行一些可能需要管理访问权限的任务。

* **内容制作者/内容编辑器** (Adobe Experience Manager)
* **营销人员** (Adobe Target/优化团队)

### WKND网站主页

![AEM Target方案1](assets/personalization-use-case-2/aem-target-use-case-2.png)

### 前提条件

* **AEM**
   * [AEM创作和](./implementation.md#getting-aem) 发布实例在localhost 4502和4503上。
   * [使用Adobe Experience Platform Launch与Adobe Target集成](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * 访问您的组织Adobe Experience Cloud - <https://>`<yourcompany>`.experiencecloud.adobe.com
   * Experience Cloud配置了以下解决方案
      * [Adobe Target](https://experiencecloud.adobe.com)

## 内容编辑器活动

1. 营销人员与AEM内容编辑器一起发起WKND主页重新设计讨论，并详细介绍相关要求。
   * ***要求*** :通过基于卡的设计重新设计WKND网站主页。
2. 然后，根据要求，AEM内容编辑器使用基于卡的设计创建新的WKND站点主页并发布新的主页。

## 营销人员活动

1. 营销人员会创建一个A/B目标活动，将重定向选件作为体验，并将分配给新主页的100%网站流量，并添加成功目标和量度。
   1. 在Adobe Target窗口中，导航到&#x200B;**Activities**&#x200B;选项卡。
   2. 单击&#x200B;**创建活动**&#x200B;按钮，然后选择活动类型作为&#x200B;**A/B测试**

      ![Adobe Target — 创建活动](assets/personalization-use-case-2/create-ab-activity.png)
   3. 选择&#x200B;**Web**&#x200B;渠道，然后选择&#x200B;**可视化体验编辑器**。
   4. 输入&#x200B;**活动URL**&#x200B;并单击&#x200B;**下一步**以打开可视化体验编辑器。
      ![Adobe Target — 创建活动](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. 要加载&#x200B;**可视化体验编辑器**，请在浏览器中启用&#x200B;**允许加载不安全脚本**并重新加载页面。
      ![体验定位活动](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. 请注意在可视化体验编辑器编辑器中打开WKND站点主页。
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. 将鼠标悬停在&#x200B;**体验B**上，然后选择查看其他选项。
      ![体验 B](assets/personalization-use-case-2/redirect-url.png)
   8. 选择&#x200B;**重定向到URL**选项，然后输入到新WKND主页的URL。 (http://localhost:4503/content/wknd/en1.html)
      ![体验 B](assets/personalization-use-case-2/redirect-url-2.png)
   9. **** 保存所做的更改，并继续执行活动创建的后续步骤。
   10. 选择&#x200B;**流量分配方法**&#x200B;作为手动流量，并将100%流量分配到&#x200B;**体验B**。
      ![体验B流量](assets/personalization-use-case-2/traffic.png)
   11. 单击&#x200B;**下一步**。
   12. 为活动提供&#x200B;**目标量度**并保存和关闭A/B测试。
      ![A/B测试目标量度](assets/personalization-use-case-2/goal-metric.png)
   13. 为活动提供名称（**WKND主页重新设计**）并保存更改。
   14. 在活动详细信息屏幕中，确保&#x200B;**激活**您的活动。
      ![激活活动](assets/personalization-use-case-2/ab-activate.png)
   15. 导航到WKND主页(http://localhost:4503/content/wknd/en.html)，您将被重定向到重新设计的WKND网站主页(http://localhost:4503/content/wknd/en1.html)。
      ![WKND主页重新设计](assets/personalization-use-case-2/WKND-home-page-redesign.png)

## 摘要

在本章中，营销人员能够创建活动，以使用Adobe Target将托管在AEM上的网站页面重定向到新页面。
