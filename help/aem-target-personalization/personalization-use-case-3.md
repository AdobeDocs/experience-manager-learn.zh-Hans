---
title: 使用Adobe Target可视化体验编辑器的Personalization
description: 一个端到端教程，其中演示了如何使用Adobe Target可视化体验编辑器(VEC)创建和提供个性化体验。
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 1550e6a7-04b5-4a40-9d7b-88074283402f
duration: 112
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '581'
ht-degree: 1%

---

# 使用可视化体验编辑器的Personalization

在本章中，我们将探讨如何通过在Target中拖放、交换和修改网页的布局和内容，使用&#x200B;**可视化体验编辑器**&#x200B;创建体验。

## 方案概述

WKND站点主页以卡片布局的形式显示本地活动或城市周围的最佳做法。 作为营销人员，您已被分配通过重新排列卡片布局来修改主页的任务，以便查看它对用户参与度和推动转化有何影响。

### 涉及的用户

在本练习中，需要以下用户参与，要执行某些任务，您可能需要管理权限。

* **内容制作者/内容编辑器** (Adobe Experience Manager)
* **营销人员**(Adobe Target/优化团队)

### WKND站点主页

![AEM Target方案1](assets/personalization-use-case-3/aem-target-use-case-3.png)

### 先决条件

* **AEM**
   * 在4503上运行的[AEM发布实例](./implementation.md#getting-aem)
   * [使用标记与Adobe Target集成的AEM](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * 访问您的组织Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * 已使用[Adobe Target](https://experiencecloud.adobe.com)设置的Experience Cloud

## 营销人员活动

1. 营销人员在Adobe Target中创建A/B定位活动。
   1. 在Adobe Target窗口中，导航到&#x200B;**活动**&#x200B;选项卡。
   2. 单击&#x200B;**创建活动**&#x200B;按钮并将活动类型选为&#x200B;**A/B测试**

      ![Adobe Target — 创建活动](assets/personalization-use-case-2/create-ab-activity.png)
   3. 选择&#x200B;**Web**&#x200B;渠道并选择&#x200B;**可视化体验编辑器**。
   4. 输入&#x200B;**活动URL**&#x200B;并单击&#x200B;**下一步**&#x200B;打开可视化体验编辑器。

      ![Adobe Target — 创建活动](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. 若要加载&#x200B;**可视化体验编辑器**，请启用&#x200B;**允许在浏览器上加载Unsafe脚本**，然后重新加载您的页面。

      ![体验定位活动](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. 请注意在可视化体验编辑器中WKND站点主页处于打开状态。

      ![VEC](assets/personalization-use-case-2/vec.png)
   7. **体验A**&#x200B;提供默认的WKND主页，让我们编辑&#x200B;**体验B**&#x200B;的内容布局。

      ![体验B](assets/personalization-use-case-3/use-case3-experience-b.png)
   8. 单击其中一个卡片布局容器(*Best Roasters*)并选择&#x200B;**重新排列**&#x200B;选项。

      ![容器选择](assets/personalization-use-case-3/container-selection.png)
   9. 单击要重新排列的容器，并将其拖放到所需位置。 让我们将&#x200B;*最佳烘焙器*&#x200B;容器从第一行第一列重新排列到第一行第三列。 现在&#x200B;*最佳烘焙师*&#x200B;容器位于&#x200B;*摄影展览*容器旁边。

      ![容器交换](assets/personalization-use-case-3/container-swap.png)
      交换后&#x200B;**&#x200B;**
      ![容器已交换](assets/personalization-use-case-3/after-swap-1-3.png)
   10. 同样，重新排列其他卡容器的位置。

       ![容器已交换](assets/personalization-use-case-3/after-swap-all.png)
   11. 我们还要在轮盘组件下方和卡片布局上方添加标题文本。
   12. 单击轮播容器，然后选择&#x200B;**此项后插入>HTML**&#x200B;选项以添加HTML。

       ![添加文本](assets/personalization-use-case-3/add-text.png)

       ```html
       <h1 style="text-align:center">Check Out the Hot Spots in Town</h1>
       ```

       ![添加文本](assets/personalization-use-case-3/after-changes.png)
   13. 单击&#x200B;**下一步**&#x200B;继续您的活动。
   14. 选择&#x200B;**流量分配方法**&#x200B;作为手动，并为&#x200B;**体验B**&#x200B;分配100%的流量。

       ![体验B流量](assets/personalization-use-case-2/traffic.png)
   15. 单击&#x200B;**下一步**。
   16. 为活动提供&#x200B;**目标量度**，并保存和关闭A/B测试。

       ![A/B测试目标指标](assets/personalization-use-case-2/goal-metric.png)
   17. 提供活动的名称（**WKND主页刷新**）并保存更改。
   18. 在“活动详细信息”屏幕中，确保&#x200B;**激活**&#x200B;您的活动。

       ![激活活动](assets/personalization-use-case-3/save-activity.png)
   19. 导航到WKND主页(http://localhost:4503/content/wknd/en.html)，您会注意到我们在WKND主页刷新A/B测试活动中添加的更改。

       ![WKND主页已刷新](assets/personalization-use-case-3/activity-result.png)
   20. 打开浏览器控制台，然后检查“网络”选项卡以查找WKND主页刷新A/B测试活动的目标响应。

      ![网络活动](assets/personalization-use-case-3/activity-result.png)

## 摘要

在本章中，营销人员能够通过拖放、交换和修改网页的布局和内容来创建使用可视化体验编辑器的体验，而无需更改任何代码来运行测试。
