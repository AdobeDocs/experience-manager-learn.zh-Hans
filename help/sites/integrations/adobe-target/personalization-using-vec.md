---
title: 使用Visual Experience Composer实现个性化
description: 了解如何使用Visual Experience Composer创建Adobe Target活动。
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6352
thumbnail: 6352-personalization-using-vec.jpg
topic: Integrations
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '521'
ht-degree: 1%

---


# 使用Visual Experience Composer {#personalization-vec}实现个性化

了解如何使用Visual Experience Composer(VEC)创建A/B测试目标活动。

## 前提条件

要在AEM网站上使用VEC，必须完成以下设置：

1. [将Adobe Target添加到AEM网站](./add-target-launch-extension.md)
1. [从Launch触发Adobe Target呼叫](./load-and-fire-target.md)

## 方案概述

WKND站点主页以信息卡的形式显示本地活动或城市周围最适合做的事情。 作为营销人员，您已分配任务来修改主页，方法是对冒险部分Teaser进行文本更改并了解它如何提高转化率。

## 使用Visual Experience Composer(VEC)创建A/B测试的步骤

1. 登录[Adobe Experience Cloud](https://experience.adobe.com/)，点按&#x200B;__目标__，导航至&#x200B;__活动__&#x200B;选项卡

   + 如果在Experience Cloud仪表板上未看到&#x200B;__目标__，请确保在右上方的组织切换器中选择了正确的Adobe组织，并且已授予您用户对[Adobe Admin Console](https://adminconsole.adobe.com/)中的目标的访问权限。

1. 单击&#x200B;**创建活动**&#x200B;按钮，然后选择&#x200B;**A/B测试**&#x200B;活动

   ![A/B活动](assets/ab-target-activity.png)

1. 选择&#x200B;**Visual Experience Composer**&#x200B;选项，提供活动URL，然后单击&#x200B;**Next**

   ![活动URL](assets/ab-test-url.png)

1. 创建新活动后，Visual Experience Composer在左侧显示两个选项卡：*体验A*&#x200B;和&#x200B;*体验B*。 从列表中选择体验。 您可以使用&#x200B;**添加体验**&#x200B;按钮将新体验添加到列表。

   ![体验A](assets/experience.png)

1. 在页面上选择要进行修改的图像或文本，或使用代码编辑器选取和开始HTML元素。

   ![元素](assets/select-element.png)

1. 将文本从&#x200B;*Camping in Western Australia*&#x200B;更改为&#x200B;*Adventures of Australia*。 列表添加到体验的更改将显示在修改下。 您可以单击并编辑修改的项目，以视图其CSS选择器和添加到它的新内容。

   ![冒险](assets/adventures.png)

1. 将&#x200B;*体验A*&#x200B;重命名为&#x200B;*冒险*
1. 同样，将&#x200B;*体验B*&#x200B;上的文本从&#x200B;*Camping in Western Australia*&#x200B;更新为&#x200B;*探索澳大利亚荒野*。

   ![浏览](assets/explore.png)

1. 单击&#x200B;**下一步**&#x200B;移至定位，让我们在两个体验之间保持手动流量分配50-50。

   ![定位](assets/targeting.png)

1. 对于“目标”和设置，选择报告源作为Adobe Target，然后选择目标量度作为包含页面视图操作的“转换”。

   ![目标](assets/goals.png)

1. 为活动提供名称并保存。
1. 激活保存的活动，将更改实时推送。

   ![目标](assets/activate.png)

1. 在新选项卡中打开您的网站页面(步骤3中的活动URL)，您应该可以从我们的A/B测试活动视图体验（冒险或浏览）。

   ![目标](assets/publish.png)

## 摘要

在本章中，营销人员能够通过拖放、交换和修改网页的布局和内容来创建使用可视体验书写器的体验，而无需更改任何代码来运行测试。

## 支持链接

+ [Adobe Experience Cloud调试器 — Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud调试器 — Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
