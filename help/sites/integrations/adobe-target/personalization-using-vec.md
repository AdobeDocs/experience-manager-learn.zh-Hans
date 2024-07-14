---
title: 使用可视化体验编辑器的Personalization
description: 了解如何使用可视化体验编辑器创建Adobe Target活动。
version: Cloud Service
jira: KT-6352
thumbnail: 6352-personalization-using-vec.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Sitesas a Cloud Service、AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: becf2bed-0541-45e8-9ce2-f9fb023234e0
duration: 101
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 0%

---

# 使用可视化体验编辑器的Personalization {#personalization-vec}

了解如何使用可视化体验编辑器(VEC)创建A/B测试Target活动。

## 先决条件

要在AEM网站上使用VEC，必须完成以下设置：

1. [将Adobe Target添加到您的AEM网站](./add-target-launch-extension.md)
1. [触发来自标记的Adobe Target调用](./load-and-fire-target.md)

## 方案概述

WKND站点主页以信息卡的形式显示本地活动或可在城市周围执行的最佳操作。 作为营销人员，您已被分派修改主页的任务，具体方法是更改冒险部分Teaser的文本并了解它如何提高转化。

## 使用可视化体验编辑器(VEC)创建A/B测试的步骤

1. 登录到[Adobe Experience Cloud](https://experience.adobe.com/)，点按&#x200B;__Target__，导航到&#x200B;__活动__&#x200B;选项卡

   + 如果您在Experience Cloud功能板上未看到&#x200B;__Target__，请确保在右上角的Adobe切换器中选择了正确的组织组织，并且已在[Adobe Admin Console](https://adminconsole.adobe.com/)中授予该用户访问Target的权限。

1. 单击&#x200B;**创建活动**&#x200B;按钮，然后选择&#x200B;**A/B测试**&#x200B;活动

   ![A/B活动](assets/ab-target-activity.png)

1. 选择&#x200B;**可视化体验编辑器**&#x200B;选项，提供活动URL，然后单击&#x200B;**下一步**

   ![活动URL](assets/ab-test-url.png)

1. 创建活动后，可视化体验编辑器在左侧显示两个选项卡：*体验A*&#x200B;和&#x200B;*体验B*。 从列表中选择体验。 您可以使用&#x200B;**添加体验**&#x200B;按钮向列表中添加新体验。

   ![体验A](assets/experience.png)

1. 在页面上选择图像或文本以开始进行修改，或者使用代码编辑器选取和HTML元素。

   ![元素](assets/select-element.png)

1. 将文本从&#x200B;*Camping in West Australia*&#x200B;更改为&#x200B;*Adventures of Australia*。 “修改”下将显示已添加到体验的更改的列表。 您可以单击并编辑修改后的项目以查看其CSS选择器以及添加到该项目的新内容。

   ![冒险](assets/adventures.png)

1. 将&#x200B;*体验A*&#x200B;重命名为&#x200B;*冒险*
1. 同样，将&#x200B;*体验B*&#x200B;上的文本从&#x200B;*Camping in West Australia*&#x200B;更新为&#x200B;*探索澳大利亚荒野*。

   ![浏览](assets/explore.png)

1. 单击&#x200B;**下一步**&#x200B;以转到定位，让我们将两个体验之间的手动流量分配保持为50-50。

   ![定位](assets/targeting.png)

1. 对于“目标”和“设置”，选择报表源作为Adobe Target，然后选择“目标”量度作为具有页面查看操作的转化。

   ![目标](assets/goals.png)

1. 提供活动的名称并保存。
1. 激活已保存的活动以实时推送更改。

   ![目标](assets/activate.png)

1. 在新选项卡中打开您的网站页面（步骤3中的活动URL），您应该能够从A/B测试活动查看任何体验（探索或冒险）。

   ![目标](assets/publish.png)

## 摘要

在本章中，营销人员能够通过拖放、交换和修改网页的布局和内容来使用可视化体验编辑器创建体验，而无需更改任何代码来运行测试。

## 支持链接

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
