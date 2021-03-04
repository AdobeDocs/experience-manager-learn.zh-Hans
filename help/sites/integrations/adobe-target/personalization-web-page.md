---
title: 个性化完整网页体验
description: 了解如何创建目标活动，以使用Adobe Target将AEM网站页面重定向到新页面。
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6353
thumbnail: 6353-personalization-web-page.jpg
topic: 集成
role: 开发人员
level: 中间
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '464'
ht-degree: 1%

---


# 个性化完整网页体验{#personalization-fpe}

了解如何创建活动，以使用Adobe Target将AEM上托管的站点页面重定向到新页面。

## 前提条件

要个性化AEM网站的完整页面，必须完成以下设置：

1. [将Adobe Target添加到AEM网站](./add-target-launch-extension.md)
1. [从Launch触发Adobe Target呼叫](./load-and-fire-target.md)

## 方案概述

WKND站点重新设计了主页，并希望将其当前主页访客重定向到新主页。 同时，还要了解重新设计的主页如何帮助提高用户参与度和收入。 作为营销人员，您已分配任务来创建活动，以将访客重定向到新主页。 让我们浏览WKND站点主页并了解如何使用Adobe Target创建活动。

## 使用Visual Experience Composer(VEC)创建A/B测试的步骤

1. 登录Adobe Target并导航到“活动”选项卡
1. 单击&#x200B;**创建活动**&#x200B;按钮，然后选择&#x200B;**A/B测试**&#x200B;活动

   ![A/B活动](assets/ab-target-activity.png)

1. 选择&#x200B;**Visual Experience Composer**&#x200B;选项，提供活动URL，然后单击&#x200B;**Next**

   ![活动URL](assets/ab-test-url.png)

1. 创建新活动后，Visual Experience Composer在左侧显示两个选项卡：*体验A*&#x200B;和&#x200B;*体验B*。 从列表中选择体验。 您可以使用&#x200B;**添加体验**&#x200B;按钮将新体验添加到列表。

   ![体验选项](assets/experience-options.png)

1. 可用于体验A的视图选项，然后选择&#x200B;**重定向到URL**&#x200B;选项，并为新的WKND站点主页提供URL。

   ![重定向URL](assets/redirect-url.png)

1. 将&#x200B;*体验A*&#x200B;重命名为&#x200B;*新WKND主页*&#x200B;和&#x200B;*体验B*&#x200B;重命名为&#x200B;*WKND主页*

   ![冒险](assets/new-experiences.png)

1. 单击&#x200B;**下一步**&#x200B;移至定位，并将两个体验之间的手动流量分配保持为50-50。

   ![定位](assets/targeting.png)

1. 对于“目标”和设置，选择报告源作为Adobe Target，然后选择目标量度作为包含页面视图操作的“转换”。

   ![目标](assets/goals.png)

1. 为活动提供名称并保存。
1. 激活保存的活动，将更改实时推送。

   ![目标](assets/activate.png)

1. 在新选项卡中打开您的网站页面(步骤3中的活动URL)，您应该可以从A/B测试活动视图体验(WKND主页或新WKND主页)。 `us/en.html` 重定向 `us/home.html`。

   ![目标](assets/redirect-test.png)

## 摘要

作为营销人员，您能够创建活动，将AEM上托管的网站页面重定向到使用Adobe Target的新页面。

## 支持链接

* [Adobe Experience Cloud调试器 — Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
* [Adobe Experience Cloud调试器 — Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)

