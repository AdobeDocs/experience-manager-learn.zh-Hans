---
title: 使用可视化体验编辑器进行个性化
description: 了解如何使用可视化体验编辑器创建Adobe Target活动。
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
activity: use
version: Cloud Service
kt: 6352
thumbnail: 6352-personalization-using-vec.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: becf2bed-0541-45e8-9ce2-f9fb023234e0
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '517'
ht-degree: 1%

---

# 使用可视化体验编辑器进行个性化 {#personalization-vec}

了解如何使用可视化体验编辑器(VEC)创建A/B测试Target活动。

## 前提条件

要在AEM网站上使用VEC，必须完成以下设置：

1. [将Adobe Target添加到您的AEM网站](./add-target-launch-extension.md)
1. [从Launch触发Adobe Target调用](./load-and-fire-target.md)

## 方案概述

WKND网站主页以信息卡的形式显示城市周围的本地活动或最佳活动。 作为营销人员，您已经分配了修改主页的任务，方法是对冒险部分Teaser进行文本更改，并了解它如何提高转化。

## 使用可视化体验编辑器(VEC)创建A/B测试的步骤

1. 登录 [Adobe Experience Cloud](https://experience.adobe.com/)，点按 __Target__，导航到 __活动__ 选项卡

   + 如果您没有看到 __Target__ 在“Experience Cloud”功能板上，确保在右上方的组织切换器中选择了正确的Adobe组织，并且已为您的用户授予了在 [Adobe Admin Console](https://adminconsole.adobe.com/).

1. 单击 **创建活动** 按钮，然后选择 **A/B测试** 活动

   ![A/B活动](assets/ab-target-activity.png)

1. 选择 **可视化体验编辑器** 选项，提供活动URL，然后单击 **下一个**

   ![活动URL](assets/ab-test-url.png)

1. 创建新活动后，可视化体验编辑器在左侧显示两个选项卡： *体验A* 和 *体验B*. 从列表中选择体验。 您可以使用 **添加体验** 按钮。

   ![体验A](assets/experience.png)

1. 在页面上选择要开始修改的图像或文本，或使用代码编辑器可选取和HTML元素。

   ![元素](assets/select-element.png)

1. 将文本从 *西澳大利亚露营* to *澳大利亚历险*. 添加到体验的更改列表将显示在修改下。 您可以单击并编辑修改的项目，以查看其CSS选择器以及添加到该项目的新内容。

   ![冒险](assets/adventures.png)

1. 重命名 *体验A* to *冒险*
1. 同样，在 *体验B* 从 *西澳大利亚露营* to *探索澳大利亚荒野*.

   ![探索](assets/explore.png)

1. 单击 **下一个** 要转到“定位”，让我们在两个体验之间将“手动”流量分配为50-50。

   ![定位](assets/targeting.png)

1. 对于“目标和设置”，选择报表源作为Adobe Target，然后在页面查看操作中选择目标量度作为转化。

   ![目标](assets/goals.png)

1. 提供活动名称并进行保存。
1. 激活保存的活动以实时推送更改。

   ![目标](assets/activate.png)

1. 在新选项卡中打开您的网站页面（步骤3中的活动URL），您应该能够从我们的A/B测试活动中查看其中任一体验（冒险或浏览）。

   ![目标](assets/publish.png)

## 摘要

在本章中，营销人员能够通过拖放、交换和修改网页的布局和内容来使用可视化体验编辑器创建体验，而无需更改任何代码来运行测试。

## 支持链接

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
