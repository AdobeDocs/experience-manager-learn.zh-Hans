---
title: 创作内容片段 — AEM无外设快速入门 — GraphQL
description: 开始使用Adobe Experience Manager(AEM)和GraphQL。 基于内容片段模型创建和编辑新的内容片段。 了解如何创建内容片段的变体。
sub-product: 资产
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6713
thumbnail: 22451.jpg
feature: '"内容片段， GraphQL API"'
topic: “无头、内容管理”
role: 开发人员
level: 初学者
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '794'
ht-degree: 0%

---


# 创作内容片段{#authoring-content-fragments}

在本章中，您将根据[新定义的参与者内容片段模型](./content-fragment-models.md)创建和编辑新的内容片段。 您还将学习如何创建内容片段的变体。

## 前提条件 {#prerequisites}

这是一个多部分教程，假定[定义内容片段模型](./content-fragment-models.md)中概述的步骤已完成。

## 目标{#objectives}

* 基于内容片段模型创作内容片段
* 创建内容片段变量

## 内容片段创作概述{#overview}

>[!VIDEO](https://video.tv.adobe.com/v/22451/?quality=12&learn=on)

以上视频概括介绍了内容片段的创作。

## 创建内容片段{#create-content-fragment}

在上一章[定义内容片段模型](./content-fragment-models.md)中，创建了&#x200B;**参与者**&#x200B;模型。 使用此模型创作新的内容片段。

1. 从&#x200B;**AEM 开始**&#x200B;菜单导航到&#x200B;**资产** > **文件**。
1. 单击文件夹可导航到&#x200B;**WKND Site** > **English** > **Contributors**。 此文件夹包含WKND品牌的投稿人的头部列表。

1. 单击右上角的&#x200B;**创建**，然后选择&#x200B;**内容片段**:

   ![单击创建新片段](assets/author-content-fragments/create-content-fragment-menu.png)

1. 选择&#x200B;**Contributor**&#x200B;型号，然后单击&#x200B;**下一步**。

   ![选择参与者模型](assets/author-content-fragments/select-contributor-model.png)

   这与在上一章中创建的&#x200B;**Contributor**&#x200B;模型相同。

1. 输入&#x200B;**Stacey Roswells**&#x200B;作为标题，然后单击&#x200B;**创建**。
1. 单击&#x200B;**成功**&#x200B;对话框中的&#x200B;**打开**&#x200B;以打开新创建的片段。

   ![已创建新内容片段](assets/author-content-fragments/new-content-fragment.png)

   请注意，模型定义的字段现在可用于创作此内容片段实例。

1. 对于&#x200B;**全名**，请输入：**史黛西·罗斯威尔斯**。
1. 对于&#x200B;**传记**，请输入简短传记。 需要灵感吗？ 请随时重用此[文本文件](assets/author-content-fragments/stacey-roswells-bio.txt)。
1. 对于&#x200B;**图片参考**，单击&#x200B;**文件夹**&#x200B;图标并浏览至&#x200B;**WKND站点** > **英语** > **参与者** > **stacey-roswells.jpg**.. 这将评估路径：`/content/dam/wknd/en/contributors/stacey-roswells.jpg`。
1. 对于&#x200B;**职业**，选择&#x200B;**摄影师**。

   ![创作的片段](assets/author-content-fragments/stacye-roswell-fragment-authored.png)

1. 单击&#x200B;**保存**&#x200B;以保存更改。

## 创建内容片段变体

所有内容片段开始具有&#x200B;**主控**&#x200B;变量。 **主控**&#x200B;变量可视为片段的&#x200B;*default*&#x200B;内容，当内容通过GraphQL API公开时自动使用。 也可以创建内容片段的变体。 此功能为设计实施提供了更多灵活性。

变量可用于目标特定渠道。 例如，可以创建&#x200B;**mobile**&#x200B;变量，它包含较少的文本或引用特定于渠道的图像。 如何使用变量取决于实施。 与任何功能一样，在使用前应进行仔细的规划。

接下来，创建一个新变体，了解可用功能。

1. 重新打开&#x200B;**Stacey Roswells**&#x200B;内容片段。
1. 在左边栏中，单击&#x200B;**创建变量**。
1. 在&#x200B;**新变量**&#x200B;模式中，输入&#x200B;**摘要**&#x200B;的标题。

   ![新变体 — 摘要](assets/author-content-fragments/new-variation-summary.png)

1. 单击&#x200B;**Berium**&#x200B;多行字段，然后单击&#x200B;**展开**&#x200B;按钮以输入多行字段的全屏视图。

   ![进入全屏视图](assets/author-content-fragments/enter-full-screen-view.png)

1. 单击右上角菜单中的&#x200B;**摘要文本**。

1. 输入&#x200B;**目标**，共&#x200B;**50**&#x200B;个单词，然后单击&#x200B;**开始**。

   ![总结预览](assets/author-content-fragments/summarize-text-preview.png)

   这将打开一个总结预览。 AEM计算机语言处理器将尝试根据目标字数统计来汇总文本。 您还可以选择要删除的不同句子。

1. 如果您对总结感到满意，请单击&#x200B;**摘要**。 单击多行文本字段并切换&#x200B;**展开**&#x200B;按钮以返回主视图。

1. 单击&#x200B;**保存**&#x200B;以保存更改。

## 创建其他内容片段

重复[创建内容片段](#create-content-fragment)中所述的步骤，以创建额外的&#x200B;**参与者**。 下一章将用作如何查询多个片段的示例。

1. 在&#x200B;**参与者**&#x200B;文件夹中，单击右上角的&#x200B;**创建**，然后选择&#x200B;**内容片段**:
1. 选择&#x200B;**Contributor**&#x200B;型号，然后单击&#x200B;**下一步**。
1. 输入&#x200B;**Jacob Wester**&#x200B;作为标题，然后单击&#x200B;**创建**。
1. 单击&#x200B;**成功**&#x200B;对话框中的&#x200B;**打开**&#x200B;以打开新创建的片段。
1. 对于&#x200B;**全名**，请输入：**Jacob Wester**。
1. 对于&#x200B;**传记**，请输入简短传记。 需要灵感吗？ 请随时重用此[文本文件](assets/author-content-fragments/jacob-wester.txt)。
1. 对于&#x200B;**图片引用**，单击&#x200B;**文件夹**&#x200B;图标，浏览至&#x200B;**WKND站点** > **英语** > **参与者** > **jacob_wester.jpg**。 这将评估路径：`/content/dam/wknd/en/contributors/jacob_wester.jpg`。
1. 对于&#x200B;**占领**，选择&#x200B;**书写器**。
1. 单击&#x200B;**保存**&#x200B;以保存更改。 除非您愿意，否则无需创建变量！

   ![其他内容片段](assets/author-content-fragments/additional-content-fragment.png)

   您现在应当有两个&#x200B;**参与者**&#x200B;片段。

## 恭喜！{#congratulations}

恭喜您，您刚刚创作了多个内容片段并创建了一个变体。

## 后续步骤{#next-steps}

在下一章[浏览GraphQL API](explore-graphql-api.md)中，您将使用内置的GrapiQL工具浏览AEM GraphQL API。 了解AEM如何根据内容片段模型自动生成GraphQL模式。 您将尝试使用GraphQL语法构建基本查询。