---
title: 全网页体验的个性化
description: 了解如何创建活动，使用Adobe Target将托管在AEM上的站点页面重定向到新页面。
feature: targeting
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6353
thumbnail: 6353-personalization-web-page.jpg
translation-type: tm+mt
source-git-commit: 892cb074814eabd347ba7aef883721df0ee4d431
workflow-type: tm+mt
source-wordcount: '430'
ht-degree: 0%

---


# 全网页体验的个性化 {#personalization-fpe}

了解如何创建活动，使用Adobe Target将托管在AEM上的站点页面重定向到新页面。

## 方案概述

WKND网站重新设计了主页，希望将其当前主页访客重定向到新主页。 同时，还要了解经过重新设计的主页如何帮助提高用户参与度和收入。 作为营销人员，您已分配任务来创建活动，以将访客重定向到新主页。 让我们浏览WKND站点主页并学习如何使用Adobe Target创建活动。

## 使用Visual Experience Composer(VEC)创建A/B测试的步骤

1. 登录Adobe Target并导航到活动选项卡
1. 单击 **“创建活动** ”按钮，然后选择 **A/B测试活动** 。

   ![A/B活动](assets/ab-target-activity.png)

1. 选择“ **可视体验书写器** ”选项，提供活动URL，然后单击“下 **一步”**

   ![活动URL](assets/ab-test-url.png)

1. 创建新活动后，Visual Experience Composer在左侧显示两个选项卡： *体验* A *和体验B*。 从列表中选择体验。 您可以使用添加体验按钮将新体验 **添加到列表** 。

   ![体验选项](assets/experience-options.png)

1. 视图选项可用于体验A，然后选 **择重定向到URL** 选项，并为新的WKND站点主页提供URL。

   ![重定向URL](assets/redirect-url.png)

1. 将体 *验A* 重命 *名为新WKND* 主页，将体 *验B重命名为* WKND *主页*

   ![冒险](assets/new-experiences.png)

1. 单 **击下** 一步以转到定位，并在两个体验之间保持手动流量分配50-50。

   ![定位](assets/targeting.png)

1. 对于“目标”和“设置”，选择报告源作为“Adobe Target”，然后使用页面视图操作选择目标量度作为“转换”。

   ![目标](assets/goals.png)

1. 为活动提供名称并保存。
1. 激活保存的活动以实时推送更改。

   ![目标](assets/activate.png)

1. 在新选项卡中打开您的网站页面(步骤3中的活动URL)，您应能够从我们的A/B测试活动视图体验(WKND主页或新WKND主页)。 `us/en.html` 重定向 `us/home.html`到。

   ![目标](assets/redirect-test.png)

## 摘要

作为营销人员，您能够创建活动，将托管在AEM上的网站页面重定向到使用Adobe Target的新页面。

## 支持链接

* [Adobe Experience Cloud调试器- Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
* [Adobe Experience Cloud调试器- Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)

