---
title: 使用Adobe Target可视化体验编辑器进行个性化
seo-title: Personalization using Adobe Target Visual Experience Composer (VEC)
description: 一个端到端教程，其中演示了如何使用Adobe Target可视化体验编辑器(VEC)创建和提供个性化体验。
seo-description: An end-to-end tutorial showing how to create and deliver personalized experience using Adobe Target Visual Experience Composer (VEC).
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
exl-id: 1550e6a7-04b5-4a40-9d7b-88074283402f
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '585'
ht-degree: 2%

---

# 使用可视化体验编辑器进行个性化

在本章中，我们将探索使用创建体验 **可视化体验编辑器** 方法是从Target中拖放、交换和修改网页的布局和内容。

## 方案概述

WKND站点主页以卡片布局的形式显示城市周围的本地活动或最佳操作。 作为营销人员，您已被分派通过重新排列卡片布局来修改主页的任务，以便查看它对用户参与度和推动转化有何影响。

### 涉及的用户

在本练习中，需要涉及以下用户，要执行某些任务，您可能需要管理访问权限。

* **内容制作者/内容编辑器** (Adobe Experience Manager)
* **营销人员** (Adobe Target/优化团队)

### WKND站点主页

![AEM Target场景1](assets/personalization-use-case-3/aem-target-use-case-3.png)

### 前提条件

* **AEM**
   * [AEM发布实例](./implementation.md#getting-aem) 在4503上运行
   * [使用Adobe Experience Platform Launch将AEM与Adobe Target集成](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * 访问您的组织Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud配置有 [Adobe Target](https://experiencecloud.adobe.com)

## 营销人员活动

1. 营销人员在Adobe Target中创建A/B定位活动。
   1. 在Adobe Target窗口中，导航到 **活动** 选项卡。
   2. 单击 **创建活动** 按钮并选择活动类型 **A/B测试**
      ![Adobe Target — 创建活动](assets/personalization-use-case-2/create-ab-activity.png)
   3. 选择 **Web** 渠道并选择 **可视化体验编辑器**.
   4. 输入 **活动URL** 并单击 **下一个** 以打开可视化体验编辑器。
      ![Adobe Target — 创建活动](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. 对象 **可视化体验编辑器** 要加载，请启用 **允许加载Unsafe脚本** ，然后重新加载页面。
      ![体验定位活动](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. 请注意，WKND站点主页在可视化体验编辑器中打开。
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. **体验A** 提供默认的WKND主页，让我们编辑内容布局 **体验B**.
      ![体验 B](assets/personalization-use-case-3/use-case3-experience-b.png)
   8. 单击其中一个卡片布局容器(*最佳烘烤师*)并选择 **重新排列** 选项。
      ![容器选择](assets/personalization-use-case-3/container-selection.png)
   9. 单击要重新排列的容器，并将其拖放到所需位置。 让我们重新排列 *最佳烘烤师* 容器从第一行第一列到第一行第三列。 现在， *最佳烘烤师* 容器位于 *摄影展览* 容器。
      ![容器交换](assets/personalization-use-case-3/container-swap.png)
      **交换后**
      ![已交换容器](assets/personalization-use-case-3/after-swap-1-3.png)
   10. 同样，重新排列其他卡容器的位置。
      ![已交换容器](assets/personalization-use-case-3/after-swap-all.png)
   11. 此外，我们还要在轮盘组件下方和卡片布局上方添加标题文本。
   12. 单击轮盘容器，然后选择 **此项后插入>HTML** 选项以添加HTML。
      ![添加文本](assets/personalization-use-case-3/add-text.png)

      ```html
      <h1 style="text-align:center">Check Out the Hot Spots in Town</h1>
      ```

      ![添加文本](assets/personalization-use-case-3/after-changes.png)
   13. 单击 **下一个** 以继续您的活动。
   14. 选择 **流量分配方法** 作为手动，将100%的流量分配给 **体验B**.
      ![体验B流量](assets/personalization-use-case-2/traffic.png)
   15. 单击&#x200B;**下一步**。
   16. 提供 **目标量度** ，并保存并关闭A/B测试。
      ![A/B测试目标量度](assets/personalization-use-case-2/goal-metric.png)
   17. 提供名称(**WKND主页刷新**)，并保存更改。
   18. 在活动详细信息屏幕中，确保 **激活** 您的活动。
      ![激活活动](assets/personalization-use-case-3/save-activity.png)
   19. 导航到WKND主页(http://localhost:4503/content/wknd/en.html)，您会注意到我们在WKND主页刷新A/B测试活动中添加的更改。
      ![已刷新WKND主页](assets/personalization-use-case-3/activity-result.png)
   20. 打开浏览器控制台，并检查“网络”选项卡以查找“WKND主页刷新A/B测试”活动的目标响应。
      ![网络活动](assets/personalization-use-case-3/activity-result.png)

## 摘要

在本章中，营销人员能够通过拖放、交换和修改网页的布局和内容来创建使用可视化体验编辑器的体验，而无需更改任何代码来运行测试。
