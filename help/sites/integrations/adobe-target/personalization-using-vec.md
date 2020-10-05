---
title: 使用视觉体验书写器实现个性化
description: 了解如何使用视觉体验书写器创建Adobe Target活动。
feature: targeting
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6352
thumbnail: 6352-personalization-using-vec.jpg
translation-type: tm+mt
source-git-commit: 892cb074814eabd347ba7aef883721df0ee4d431
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# 使用视觉体验书写器实现个性化 {#personalization-vec}

了解如何使用Visual Experience Composer(VEC)创建A/B测试目标活动。


## 方案概述

WKND站点主页以信息卡的形式显示本地活动或在城市周围执行的最佳操作。 作为营销人员，您已经获得了修改主页的任务，具体方法是对冒险部分Teaser进行文本更改并了解它如何提高转化率。

## 使用Visual Experience Composer(VEC)创建A/B测试的步骤

1. 登录Adobe Target并导航到活动选项卡
1. 单击 **“创建活动** ”按钮，然后选择 **A/B测试活动** 。

   ![A/B活动](assets/ab-target-activity.png)

1. 选择“ **可视体验书写器** ”选项，提供活动URL，然后单击“下 **一步”**

   ![活动URL](assets/ab-test-url.png)

1. 创建新活动后，Visual Experience Composer在左侧显示两个选项卡： *体验* A *和体验B*。 从列表中选择体验。 您可以使用添加体验按钮将新体验 **添加到列表** 。

   ![体验A](assets/experience.png)

1. 在页面上选择要进行修改的图像或文本，或使用代码编辑器选择和HTML元素进行开始。

   ![元素](assets/select-element.png)

1. 将文本从Camping *in Western Australia更改为**Adventures of Australia*。 添加到体验的列表更改将显示在修改下。 您可以单击并编辑修改后的项目，以视图其CSS选择器和添加到它的新内容。

   ![冒险](assets/adventures.png)

1. 将体 *验A重命名* 为 *Adventure*
1. 同样，从西澳大利亚 *的Camping* *in Australia更新Experience B的文* 本，探 *索澳大利亚荒野*。

   ![浏览](assets/explore.png)

1. 单 **击下** 一步以转到定位，让我们在两个体验之间保持手动流量分配50-50。

   ![定位](assets/targeting.png)

1. 对于“目标”和“设置”，选择报告源作为“Adobe Target”，然后使用页面视图操作选择目标量度作为“转换”。

   ![目标](assets/goals.png)

1. 为活动提供名称并保存。
1. 激活保存的活动以实时推送更改。

   ![目标](assets/activate.png)

1. 在新选项卡中打开您的网站页面(步骤3中的活动URL)，您应能够从我们的A/B测试活动中视图体验（冒险或浏览）。

   ![目标](assets/publish.png)

## 摘要

在本章中，营销人员能够通过拖放、交换和修改网页的布局和内容来创建使用视觉体验书写器的体验，而无需更改任何代码来运行测试。

## 支持链接

* [Adobe Experience Cloud调试器- Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
* [Adobe Experience Cloud调试器- Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
