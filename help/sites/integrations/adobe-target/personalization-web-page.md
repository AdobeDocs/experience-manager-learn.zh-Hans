---
title: 完整网页体验的个性化
description: 了解如何创建Target活动，以使用Adobe Target将AEM网站页面重定向到新页面。
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
activity: use
version: Cloud Service
kt: 6353
thumbnail: 6353-personalization-web-page.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: 2d201b48-c0fb-4bb4-a7d8-da9f4702e9ff
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '461'
ht-degree: 0%

---

# 完整网页体验的个性化 {#personalization-fpe}

了解如何使用Adobe Target创建活动，将AEM上托管的网站页面重定向到新页面。

## 前提条件

要个性化AEM网站的完整页面，必须完成以下设置：

1. [将Adobe Target添加到您的AEM网站](./add-target-launch-extension.md)
1. [从Launch触发Adobe Target调用](./load-and-fire-target.md)

## 方案概述

WKND网站重新设计了其主页，并希望将其当前主页的访客重定向到新主页。 同时，还要了解重新设计的主页如何帮助提高用户参与度和收入。 作为营销人员，您已经分配了创建活动以将访客重定向到新主页的任务。 让我们浏览WKND站点主页，并了解如何使用Adobe Target创建活动。

## 使用可视化体验编辑器(VEC)创建A/B测试的步骤

1. 登录Adobe Target，然后导航到活动选项卡
1. 单击&#x200B;**创建活动**&#x200B;按钮，然后选择&#x200B;**A/B测试**&#x200B;活动

   ![A/B活动](assets/ab-target-activity.png)

1. 选择&#x200B;**可视化体验编辑器**&#x200B;选项，提供活动URL，然后单击&#x200B;**下一步**

   ![活动URL](assets/ab-test-url.png)

1. 创建新活动后，可视化体验编辑器在左侧显示两个选项卡：*体验A*&#x200B;和&#x200B;*体验B*。 从列表中选择体验。 您可以使用&#x200B;**添加体验**&#x200B;按钮，将新体验添加到列表。

   ![体验选项](assets/experience-options.png)

1. 查看体验A的可用选项，然后选择&#x200B;**重定向到URL**&#x200B;选项，并为新的WKND网站主页提供URL。

   ![重定向URL](assets/redirect-url.png)

1. 将&#x200B;*体验A*&#x200B;重命名为&#x200B;*新的WKND主页*&#x200B;和&#x200B;*体验B*&#x200B;重命名为&#x200B;*WKND主页*

   ![冒险](assets/new-experiences.png)

1. 单击&#x200B;**Next**&#x200B;以转到“定位”，并在两个体验之间保持手动流量分配50-50。

   ![定位](assets/targeting.png)

1. 对于“目标和设置”，选择报表源作为Adobe Target，然后在页面查看操作中选择目标量度作为转化。

   ![目标](assets/goals.png)

1. 提供活动名称并进行保存。
1. 激活保存的活动以实时推送更改。

   ![目标](assets/activate.png)

1. 在新选项卡中打开您的网站页面（步骤3中的活动URL），您应该能够从我们的A/B测试活动中查看任一体验（WKND主页或新的WKND主页）。 `us/en.html` 重定向到 `us/home.html`。

   ![目标](assets/redirect-test.png)

## 摘要

作为营销人员，您能够创建活动，使用Adobe Target将托管在AEM上的页面重定向到新页面。

## 支持链接

* [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
* [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
