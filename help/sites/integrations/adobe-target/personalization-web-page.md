---
title: 完整网页体验的个性化
description: 了解如何使用Adobe Target创建Target活动，将AEM网站页面重定向到新页面。
jira: KT-6353
thumbnail: 6353-personalization-web-page.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Sitesas a Cloud Service、AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 2d201b48-c0fb-4bb4-a7d8-da9f4702e9ff
duration: 124
source-git-commit: 970093bb54046fee49e2ac209f1588e70582ab67
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 0%

---

# 完整网页体验的个性化 {#personalization-fpe}

了解如何使用Adobe Target创建活动，将AEM上托管的网页重定向到新页面。

## 先决条件

要个性化AEM网站的完整页面，必须完成以下设置：

1. [将Adobe Target添加到您的AEM网站](./add-target-launch-extension.md)
1. [触发来自标记的Adobe Target调用](./load-and-fire-target.md)

## 方案概述

WKND站点重新设计了其主页，并希望将其当前主页访客重定向到新主页。 同时，还要了解重新设计的主页如何有助于提高用户参与度和收入。 作为营销人员，您已被分派创建活动的任务，以将访客重定向到新主页。 让我们来探索WKND网站主页，了解如何使用Adobe Target创建活动。

## 使用可视化体验编辑器(VEC)创建A/B测试的步骤

1. 登录Adobe Target并导航到活动选项卡
1. 单击 **创建活动** 按钮，然后选择 **A/B测试** 活动

   ![A/B活动](assets/ab-target-activity.png)

1. 选择 **可视化体验编辑器** 选项，提供活动URL，然后单击 **下一个**

   ![活动URL](assets/ab-test-url.png)

1. 创建活动后，可视化体验编辑器在左侧显示两个选项卡： *体验A* 和 *体验B*. 从列表中选择体验。 您可以使用向列表中添加新体验 **添加体验** 按钮。

   ![体验选项](assets/experience-options.png)

1. 查看适用于体验A的选项，然后选择 **重定向到URL** 选项并提供新WKND站点主页的URL。

   ![重定向URL](assets/redirect-url.png)

1. 重命名 *体验A* 到 *新建WKND主页* 和 *体验B* 到 *WKND主页*

   ![冒险](assets/new-experiences.png)

1. 单击 **下一个** 以转到定位，并保持两个体验之间的手动流量分配为50-50。

   ![定位](assets/targeting.png)

1. 对于“目标”和“设置”，选择报表源作为Adobe Target，然后选择“目标”量度作为具有页面查看操作的转化。

   ![目标](assets/goals.png)

1. 提供活动的名称并保存。
1. 激活已保存的活动以实时推送更改。

   ![目标](assets/activate.png)

1. 在新选项卡中打开您的网站页面（步骤3中的活动URL），您应该能够从A/B测试活动中查看任一体验（WKND主页或新WKND主页）。 `us/en.html` 重定向到 `us/home.html`.

   ![目标](assets/redirect-test.png)

## 摘要

作为营销人员，您可以使用Adobe Target创建活动，将AEM上托管的网页重定向到新页面。

## 支持链接

* [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
