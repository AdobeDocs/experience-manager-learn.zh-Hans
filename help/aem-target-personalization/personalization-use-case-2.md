---
title: 使用Adobe Target进行个性化
description: 一个端到端教程，其中演示了如何使用Adobe Target创建和提供个性化体验。
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 53cafd06-3a0a-4995-947d-179146b89234
duration: 159
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '580'
ht-degree: 1%

---

# 使用Adobe Target个性化设置完整网页体验

在上一章中，我们了解了如何使用作为HTML片段创建并从AEM中导出的内容在Adobe Target中创建基于地理位置的活动。

在本章中，我们将探讨如何创建活动，以使用Adobe Target将AEM上托管的网页重定向到新页面。

## 方案概述

WKND站点重新设计了其主页，并希望将其当前主页访客重定向到新主页。 同时，还要了解重新设计的主页如何有助于提高用户参与度和收入。 作为营销人员，您已被分派创建活动的任务，以将访客重定向到新主页。 让我们来探索WKND网站主页，了解如何使用Adobe Target创建活动。

### 涉及的用户

在本练习中，需要以下用户参与，要执行某些任务，您可能需要管理权限。

* **内容制作器/内容编辑器** (Adobe Experience Manager)
* **营销人员** (Adobe Target/优化团队)

### WKND站点主页

![AEM Target场景1](assets/personalization-use-case-2/aem-target-use-case-2.png)

### 前提条件

* **AEM**
   * [AEM创作和发布实例](./implementation.md#getting-aem) 分别在localhost 4502和4503上运行。
   * [使用Adobe Experience Platform Launch将AEM与Adobe Target集成](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * 访问您的组织Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud配置了以下解决方案
      * [Adobe Target](https://experiencecloud.adobe.com)

## 内容编辑器活动

1. 营销人员使用AEM内容编辑器启动WKND主页重新设计讨论并详细说明要求。
   * ***要求*** ：使用基于信息卡的设计重新设计WKND站点主页。
2. 然后，AEM内容编辑器将根据要求，使用基于卡片的设计创建一个新的WKND站点主页，并发布新主页。

## 营销人员活动

1. 营销人员创建A/B目标活动，并将重定向选件作为体验，以及向新主页分配100%网站流量，并添加成功目标和量度。
   1. 在Adobe Target窗口中，导航至 **活动** 选项卡。
   2. 单击 **创建活动** 按钮并选择活动类型 **A/B测试**
      ![Adobe Target — 创建活动](assets/personalization-use-case-2/create-ab-activity.png)
   3. 选择 **Web** 渠道并选择 **可视化体验编辑器**.
   4. 输入 **活动URL** 并单击 **下一个** 以打开可视化体验编辑器。
      ![Adobe Target — 创建活动](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. 对象 **可视化体验编辑器** 要加载，请启用 **允许加载Unsafe脚本** 并重新加载页面。
      ![体验定位活动](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. 请注意在可视化体验编辑器中WKND站点主页处于打开状态。
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. 将鼠标悬停在 **体验B** 并选择查看其他选项。
      ![体验B](assets/personalization-use-case-2/redirect-url.png)
   8. 选择 **重定向到URL** 选项并输入新WKND主页的URL。 (http://localhost:4503/content/wknd/en1.html)
      ![体验B](assets/personalization-use-case-2/redirect-url-2.png)
   9. **保存** 所做的更改，然后继续执行活动创建的后续步骤。
   10. 选择 **流量分配方法** 手动分配，并将100%的流量分配给 **体验B**.
      ![体验B流量](assets/personalization-use-case-2/traffic.png)
   11. 单击&#x200B;**下一步**。
   12. 提供 **目标量度** ，并保存并关闭A/B测试。
      ![A/B测试目标量度](assets/personalization-use-case-2/goal-metric.png)
   13. 提供名称(**WKND主页重新设计**)，并保存更改。
   14. 在活动详细信息屏幕中，确保 **激活** 您的活动。
      ![激活活动](assets/personalization-use-case-2/ab-activate.png)
   15. 导航到WKND主页(http://localhost:4503/content/wknd/en.html)，您将被重定向到重新设计的WKND站点主页(http://localhost:4503/content/wknd/en1.html)。
      ![WKND主页重新设计](assets/personalization-use-case-2/WKND-home-page-redesign.png)

## 摘要

在本章中，营销人员能够创建活动，以使用Adobe Target将AEM上托管的网页重定向到新页面。
