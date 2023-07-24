---
title: 将Analytics与Commerce集成教程
description: 了解如何将Analytics与Commerce集成。
solution: Analytics, Commerce
feature: Integrations
topic: Integrations
role: Leader, Architect, Admin, Developer
level: Beginner
index: true
hidefromtoc: true
kt: null
thumbnail: null
last-substantial-update: 2023-04-11T00:00:00Z
badgeIntegration: label="集成" type="positive"
source-git-commit: ed53392381fa568de8230288e6b85c87540222cf
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 0%

---


# 将Analytics与Commerce集成

* 验证该帐户是否有权访问Adobe Analytics。

* 在Adobe Analytics中创建项目。

* 创建架构。
   * 您需要此项才能在后续步骤中选择选项。 要创建架构，请查看“数据管理”下方的左列，然后找到架构。 现在，在左上角，单击“创建架构”。 选择XDM ExperienceEvent。
   * 在左侧查找字段组，单击添加
      * 在搜索中，您可以通过输入 `ExperienceEvent Commerce`
      * 查找 `Adobe Analytics ExperienceEvent Commerce` 并选中该框
      * 确保单击 `Add field groups` 保存并继续
* 创建一个数据集，您在下一步设置“数据流”时需要此项。
   * 数据集位于左列“数据管理”下，并查找“数据集”。
   * 然后单击右上角的“创建数据集”。 从架构创建数据集。
   * 搜索并使用您之前创建的架构
* 创建数据流。 您可以通过使用“左列中的数据收集”并查找“数据流”来访问它。
* 创建包含面板和区段的表。 对于本教程而言，这非常复杂，您需要一位经验丰富的Analytics人员来提供帮助。


最后，要查看报表，请导航到experience.adobe.com以查找工作区项目，单击要查看的项目的链接，然后您应会看到与此图像类似的内容

![某些商业数据的Analytics屏幕截图](./assets/analytics-commerce/analytics-screenshot-commerce-items.png)