---
title: 使用Adobe Target实现个性化
seo-title: 使用Adobe Target实现个性化
description: 一个端到端教程，其中演示了如何使用Adobe Target创建和提供个性化体验。
seo-description: 一个端到端教程，其中演示了如何使用Adobe Target创建和提供个性化体验。
translation-type: tm+mt
source-git-commit: 0443c8ff42e773021ff8b6e969f5c1c31eea3ae4
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 2%

---


# 使用Adobe Target个性化整个网页体验

在上一章中，我们学习了如何在Adobe Target内使用创建为体验片段的内容以及从AEM导出为HTML活动创建基于地理位置的优惠。

在本章中，我们将探索创建活动，将AEM上托管的站点页面重定向到使用Adobe Target的新页面。

## 方案概述

WKND网站重新设计了主页，希望将其当前主页访客重定向到新主页。 同时，还要了解经过重新设计的主页如何帮助提高用户参与度和收入。 作为营销人员，您已分配任务来创建活动，以将访客重定向到新主页。 让我们浏览WKND站点主页并学习如何使用Adobe Target创建活动。

### 涉及的用户

对于本练习，需要参与以下用户，并执行某些任务，您可能需要管理访问权限。

* **内容制作者／内容编辑** (Adobe Experience Manager)
* **营销人员** (Adobe Target/优化团队)

### WKND站点主页

![AEM目标方案1](assets/personalization-use-case-2/aem-target-use-case-2.png)

### 前提条件

* **AEM**
   * [AEM author和publish实例](./implementation.md#getting-aem) （分别运行于localhost 4502和4503上）。
   * [AEM使用Adobe Experience Platform Launch与Adobe Target集成](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * 访问您的组织Adobe Experience Cloud <https://>`<yourcompany>`-.experiencecloud.adobe.com
   * Experience Cloud配置了以下解决方案
      * [Adobe Target](https://experiencecloud.adobe.com)

## 内容编辑器活动

1. 营销人员与AEM内容编辑者发起WKND主页重新设计讨论，并详细说明要求。
   * ***要求*** :重新设计WKND站点主页并基于卡进行设计。
2. 根据要求，AEM Content Editor随后使用基于卡的设计创建新的WKND站点主页并发布新主页。

## 营销人员活动

1. 营销人员创建A/B目标活动，将重定向优惠作为体验，并在添加成功目标和指标后将分配给新主页的100%网站流量分配给新。
   1. 在您的Adobe Target窗口中，导航到 **活动** 选项卡。
   2. 单 **击“创建活动** ”按钮，然后选择“A/ **B测试”活动类型**

      ![Adobe Target-创建活动](assets/personalization-use-case-2/create-ab-activity.png)
   3. 选择Web **渠道** ，然后选择 **可视体验书写器**。
   4. 输入 **活动URL** ，然 **后单击** 下一步以打开Visual Experience Composer。
      ![Adobe Target-创建活动](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. 要加 **载Visual Experience Composer** ，请在浏览器 **上启用“允许加载不安全脚本** ”并重新加载页面。
      ![体验定位活动](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. 请注意，WKND站点主页在Visual Experience Composer编辑器中打开。
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. 将鼠标悬停 **在体验B上** ，然后选择“视图”其他选项。
      ![体验 B](assets/personalization-use-case-2/redirect-url.png)
   8. 选 **择“重定向到** URL”选项，然后输入指向新WKND主页的URL。 (http://localhost:4503/content/wknd/en1.html)
      ![体验 B](assets/personalization-use-case-2/redirect-url-2.png)
   9. **保存** 您所做的更改，并继续执行活动创建的后续步骤。
   10. 选择流 **量分配方法** （人工分配），将100%流量分配 **到体验B**。
      ![体验B流量](assets/personalization-use-case-2/traffic.png)
   11. 单击&#x200B;**下一步**。
   12. 提 **供活动** 、保存并关闭A/B测试的目标指标。
      ![A/B测试目标度量](assets/personalization-use-case-2/goal-metric.png)
   13. 为您的主页提&#x200B;**供名称**(WKND活动重新设计)，并保存您所做的更改。
   14. 从活动详细信息屏幕，确保激 **活** 活动。
      ![激活活动](assets/personalization-use-case-2/ab-activate.png)
   15. 导航到WKND主页(http://localhost:4503/content/wknd/en.html)，您将被重定向到重新设计的WKND站点主页(http://localhost:4503/content/wknd/en1.html)。
      ![WKND主页已重新设计](assets/personalization-use-case-2/WKND-home-page-redesign.png)

## 摘要

在本章中，营销人员能够创建一个活动，将托管在AEM上的站点页面重定向到使用Adobe Target的新页面。
