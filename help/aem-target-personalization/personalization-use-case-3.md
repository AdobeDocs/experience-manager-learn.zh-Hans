---
title: 使用Adobe Target Visual Experience Composer实现个性化
seo-title: 使用Adobe Target Visual Experience Composer(VEC)实现个性化
description: 一个端到端教程，其中演示了如何使用Adobe Target Visual Experience Composer(VEC)创建和提供个性化体验。
seo-description: 一个端到端教程，其中演示了如何使用Adobe Target Visual Experience Composer(VEC)创建和提供个性化体验。
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '615'
ht-degree: 3%

---


# 使用Visual Experience Composer实现个性化

在本章中，我们将探索使用&#x200B;**Visual Experience Composer**&#x200B;创建体验，方法是从目标中拖放、交换和修改网页的布局和内容。

## 方案概述

WKND站点主页以卡布局的形式显示本地活动或围绕城市的最佳工作。 作为营销人员，您已分配任务来修改主页，方法是重新排列卡布局，了解它如何影响用户参与和推动转化率。

### 涉及的用户

对于本练习，需要参与以下用户，并执行某些任务，您可能需要管理访问权限。

* **Content Producer/Content Editor** (Adobe Experience Manager)
* **营销人** (Adobe Target/优化团队)

### WKND站点主页

![AEM 目标方案1](assets/personalization-use-case-3/aem-target-use-case-3.png)

### 前提条件

* **AEM**
   * [AEM publish ](./implementation.md#getting-aem) instanceronning on 4503
   * [AEM与Adobe Target集成使用Adobe Experience Platform Launch](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * 访问您的组织Adobe Experience Cloud - <https://>`<yourcompany>`.experiencecloud.adobe.com
   * Experience Cloud已配置[Adobe Target](https://experiencecloud.adobe.com)

## 营销人员活动

1. 营销人员在Adobe Target中创建A/B目标活动。
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
   7. **体** 验A提供默认的WKND主页，让我们编辑体验B的内容 **布局**。
      ![体验 B](assets/personalization-use-case-3/use-case3-experience-b.png)
   8. 单击其中一个卡布局容器（*最佳过渡者*），然后选择&#x200B;**重新排列**选项。
      ![容器选择](assets/personalization-use-case-3/container-selection.png)
   9. 单击要重新排列的容器，并将其拖放到所需位置。 让我们重新排列&#x200B;*最佳容器*&#x200B;从第1行第1列到第1行第3列。 现在，*Best Roasters*&#x200B;容器将位于&#x200B;*Photography Exheris*容器旁。
      ![容器交换](assets/personalization-use-case-3/container-swap.png)

      **交换后**
      ![容器交换](assets/personalization-use-case-3/after-swap-1-3.png)
   10. 同样，重新排列其他卡容器的位置。
      ![容器交换](assets/personalization-use-case-3/after-swap-all.png)
   11. 我们还可以在传送组件下方和卡布局上方添加标题文本。
   12. 单击传送容器，然后选择&#x200B;**Inset After > HTML**选项以添加HTML。
      ![添加文本](assets/personalization-use-case-3/add-text.png)

      ```html
      <h1 style="text-align:center">Check Out the Hot Spots in Town</h1>
      ```

      ![添加文本](assets/personalization-use-case-3/after-changes.png)
   13. 单击&#x200B;**下一步**&#x200B;继续活动。
   14. 选择&#x200B;**流量分配方法**&#x200B;作为手动流量，并允许100%流量到&#x200B;**体验B**。
      ![体验B流量](assets/personalization-use-case-2/traffic.png)
   15. 单击&#x200B;**下一步**。
   16. 为您的活动提供&#x200B;**目标量度**并保存和关闭A/B测试。
      ![A/B测试目标量度](assets/personalization-use-case-2/goal-metric.png)
   17. 为活动提供名称(**WKND主页刷新**)并保存更改。
   18. 在活动详细信息屏幕中，确保&#x200B;**激活**您的活动。
      ![激活活动](assets/personalization-use-case-3/save-activity.png)
   19. 导航到WKND主页(http://localhost:4503/content/wknd/en.html)，您会注意到我们添加到WKND主页刷新A/B测试活动的更改。
      ![WKND主页已刷新](assets/personalization-use-case-3/activity-result.png)
   20. 打开浏览器控制台，并检查网络选项卡，以查找WKND主页刷新A/B测试活动的目标响应。
      ![网络活动](assets/personalization-use-case-3/activity-result.png)

## 摘要

在本章中，营销人员能够通过拖放、交换和修改网页的布局和内容来创建使用可视体验书写器的体验，而无需更改任何代码来运行测试。
