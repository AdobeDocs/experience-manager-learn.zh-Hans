---
title: 使用Adobe Target可视化体验编辑器进行个性化
seo-title: Personalization using Adobe Target Visual Experience Composer (VEC)
description: 一个端到端教程，其中演示了如何使用Adobe Target可视化体验编辑器(VEC)创建和提供个性化体验。
seo-description: An end-to-end tutorial showing how to create and deliver personalized experience using Adobe Target Visual Experience Composer (VEC).
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '583'
ht-degree: 2%

---


# 使用可视化体验编辑器进行个性化

在本章中，我们将探讨如何使用&#x200B;**可视化体验编辑器**&#x200B;创建体验，方法是在Target中拖放、交换和修改网页的布局和内容。

## 方案概述

WKND网站主页以卡布局的形式显示城市周围的本地活动或最佳工作。 作为营销人员，已经为您分配了修改主页的任务，方法是重新排列卡片布局，以了解它对用户参与度和促进转化的影响。

### 涉及的用户

在本练习中，需要涉及以下用户，并且要执行一些可能需要管理访问权限的任务。

* **内容制作者/内容编辑器** (Adobe Experience Manager)
* **营销人员** (Adobe Target/优化团队)

### WKND网站主页

![AEM Target方案1](assets/personalization-use-case-3/aem-target-use-case-3.png)

### 前提条件

* **AEM**
   * [AEM 4503](./implementation.md#getting-aem) 上的发布实例
   * [使用Adobe Experience Platform Launch与Adobe Target集成](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * 访问您的组织Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud已配置[Adobe Target](https://experiencecloud.adobe.com)

## 营销人员活动

1. 营销人员在Adobe Target中创建A/B目标活动。
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
   7. **体** 验提供默认的WKND主页，让我们编辑体验B的内容 **布局**。
      ![体验 B](assets/personalization-use-case-3/use-case3-experience-b.png)
   8. 单击其中一个卡布局容器(*Best Roasters*)，然后选择&#x200B;**Reargan**选项。
      ![容器选择](assets/personalization-use-case-3/container-selection.png)
   9. 单击要重新排列的容器，并将其拖放到所需位置。 让我们将&#x200B;*Best Roasters*&#x200B;容器从第1行第1列重新排列为第1行第3列。 现在， *Best Roasters*&#x200B;容器将位于&#x200B;*摄影展*容器旁边。
      ![容器交换](assets/personalization-use-case-3/container-swap.png)

      **交换后**
      ![交换的容器](assets/personalization-use-case-3/after-swap-1-3.png)
   10. 同样，重新排列其他卡片容器的位置。
      ![交换的容器](assets/personalization-use-case-3/after-swap-all.png)
   11. 我们还将在轮播组件下方和卡片布局上方添加标题文本。
   12. 单击传送容器，然后选择&#x200B;**Inset After > HTML**选项以添加HTML。
      ![添加文本](assets/personalization-use-case-3/add-text.png)

      ```html
      <h1 style="text-align:center">Check Out the Hot Spots in Town</h1>
      ```

      ![添加文本](assets/personalization-use-case-3/after-changes.png)
   13. 单击&#x200B;**Next**&#x200B;以继续活动。
   14. 选择&#x200B;**流量分配方法**&#x200B;作为手动流量，并将100%流量分配到&#x200B;**体验B**。
      ![体验B流量](assets/personalization-use-case-2/traffic.png)
   15. 单击&#x200B;**下一步**。
   16. 为活动提供&#x200B;**目标量度**并保存和关闭A/B测试。
      ![A/B测试目标量度](assets/personalization-use-case-2/goal-metric.png)
   17. 为活动提供名称（**WKND主页刷新**）并保存更改。
   18. 在活动详细信息屏幕中，确保&#x200B;**激活**您的活动。
      ![激活活动](assets/personalization-use-case-3/save-activity.png)
   19. 导航到WKND主页(http://localhost:4503/content/wknd/en.html)，您会注意到我们向WKND主页刷新A/B测试活动添加的更改。
      ![WKND主页已刷新](assets/personalization-use-case-3/activity-result.png)
   20. 打开浏览器控制台，然后检查“网络”选项卡以查找WKND主页刷新A/B测试活动的目标响应。
      ![网络活动](assets/personalization-use-case-3/activity-result.png)

## 摘要

在本章中，营销人员能够通过拖放、交换和修改网页的布局和内容来使用可视化体验编辑器创建体验，而无需更改任何代码来运行测试。
