---
title: 创作内容片段-AEM无头入门- GraphQL
description: 开始使用Adobe Experience Manager(AEM)和GraphQL。 根据内容片段模型创建和编辑新的内容片段。 了解如何创建内容片段的变体。
sub-product: 资产
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6713
thumbnail: 22451.jpg
translation-type: tm+mt
source-git-commit: 2ea667d3bdb73fa4da87b877f14db77d896448a7
workflow-type: tm+mt
source-wordcount: '806'
ht-degree: 0%

---


# 创作内容片段{#authoring-content-fragments}

>[!CAUTION]
>
> 针对内容片段投放的AEM GraphQL API将于2021年初发布。
> 相关文档可供预览使用。

在本章中，您将根据[新定义的参与者内容片段模型](./content-fragment-models.md)创建和编辑新的内容片段。 您还将学习如何创建内容片段的变体。

## 前提条件 {#prerequisites}

这是一个多部分教程，假定[定义内容片段模型](./content-fragment-models.md)中概述的步骤已完成。

## 目标{#objectives}

* 基于内容片段模型创作内容片段
* 创建内容片段变体

## 内容片段创作概述{#overview}

>[!VIDEO](https://video.tv.adobe.com/v/22451/?quality=12&learn=on)

以上视频概括介绍了内容片段的创作。

## 创建内容片段{#create-content-fragment}

在上一章[定义内容片段模型](./content-fragment-models.md)中，创建了&#x200B;**参与者**&#x200B;模型。 使用此模型创作新的内容片段。

1. 从&#x200B;**AEM开始**&#x200B;菜单导航到&#x200B;**资产** > **文件**。
1. 单击文件夹可导航到&#x200B;**WKND站点** > **英语** > **参与者**。 此文件夹包含WKND品牌投稿人的列表头部快照。

1. 单击右上角的&#x200B;**创建**&#x200B;并选择&#x200B;**内容片段**:

   ![单击创建新片段](assets/author-content-fragments/create-content-fragment-menu.png)

1. 选择&#x200B;**Contributor**&#x200B;型号，然后单击&#x200B;**Next**。

   ![选择参与者模型](assets/author-content-fragments/select-contributor-model.png)

   这与上一章中创建的&#x200B;**Contributor**&#x200B;模型相同。

1. 输入&#x200B;**Stacey Roswells**&#x200B;作为标题，然后单击&#x200B;**创建**。
1. 单击&#x200B;**成功**&#x200B;对话框中的&#x200B;**打开**&#x200B;以打开新创建的片段。

   ![已创建新内容片段](assets/author-content-fragments/new-content-fragment.png)

   请注意，模型定义的字段现在可用于创作内容片段的此实例。

1. 对于&#x200B;**全名**，请输入：**Stacey Roswells**。
1. 对于&#x200B;**传记**，请输入简短的传记。 需要灵感吗？ 请随意重新使用此[文本文件](assets/author-content-fragments/stacey-roswells-bio.txt)。
1. 对于&#x200B;**图片引用**，单击&#x200B;**文件夹**&#x200B;图标并浏览至&#x200B;**WKND站点** > **英语** **参与者** > **stacey-roswells.jpg&lt;a1/>.**&#x200B;此值将根据路径计算：`/content/dam/wknd/en/contributors/stacey-roswells.jpg`。
1. 对于&#x200B;**职业**，选择&#x200B;**摄影师**。

   ![创作的片段](assets/author-content-fragments/stacye-roswell-fragment-authored.png)

1. 单击&#x200B;**保存**&#x200B;以保存更改。

## 创建内容片段变体

所有内容片段开始具有&#x200B;**主控**&#x200B;变量。 **主控**&#x200B;变量可视为片段的&#x200B;*default*&#x200B;内容，当内容通过GraphQL API公开时自动使用。 也可以创建内容片段的变体。 此功能优惠了设计实现的更多灵活性。

变量可用于目标特定渠道。 例如，可以创建&#x200B;**mobile**&#x200B;变体，其中包含较少的文本或引用渠道特定的图像。 如何使用变量真正取决于实现。 与任何功能一样，在使用之前应当进行仔细的规划。

接下来，创建一个新变体以了解可用功能。

1. 重新打开&#x200B;**Stacey Roswells**&#x200B;内容片段。
1. 在左侧边栏中，单击&#x200B;**创建变量**。
1. 在&#x200B;**New Variation**&#x200B;模式中，输入&#x200B;**Summary**&#x200B;的Title。

   ![新变体——摘要](assets/author-content-fragments/new-variation-summary.png)

1. 单击&#x200B;**Bilium**&#x200B;多行字段，然后单击&#x200B;**展开**&#x200B;按钮，以输入多行字段的全屏视图。

   ![输入全屏视图](assets/author-content-fragments/enter-full-screen-view.png)

1. 单击右上角菜单中的&#x200B;**“摘要文本**”。

1. 输入&#x200B;**目标****50**&#x200B;单词，然后单击&#x200B;**开始**。

   ![总结预览](assets/author-content-fragments/summarize-text-preview.png)

   这将打开一个总结预览。 AEM机器语言处理器将尝试根据目标字计数来汇总文本。 您还可以选择不同的句子来删除。

1. 如果您对总结感到满意，请单击&#x200B;**摘要**。 单击多行文本字段并切换&#x200B;**展开**&#x200B;按钮以返回主视图。

1. 单击&#x200B;**保存**&#x200B;以保存更改。

## 创建其他内容片段

重复[创建内容片段](#create-content-fragment)中所述的步骤，以创建额外的&#x200B;**参与者**。 下一章将用作如何查询多个片段的示例。

1. 在&#x200B;**参与者**&#x200B;文件夹中，单击右上角的&#x200B;**创建**，然后选择&#x200B;**内容片段**:
1. 选择&#x200B;**Contributor**&#x200B;型号，然后单击&#x200B;**Next**。
1. 输入&#x200B;**Jacob Wester**&#x200B;作为标题，然后单击&#x200B;**创建**。
1. 单击&#x200B;**成功**&#x200B;对话框中的&#x200B;**打开**&#x200B;以打开新创建的片段。
1. 对于&#x200B;**全名**，请输入：**Jacob Wester**。
1. 对于&#x200B;**传记**，请输入简短的传记。 需要灵感吗？ 请随意重新使用此[文本文件](assets/author-content-fragments/jacob-wester.txt)。
1. 对于&#x200B;**图片引用**，单击&#x200B;**文件夹**&#x200B;图标并浏览至&#x200B;**WKND站点** > **英语****参与者** > **jacob_wester.jpg**。 此值将根据路径计算：`/content/dam/wknd/en/contributors/jacob_wester.jpg`。
1. 对于&#x200B;**占位**，选择&#x200B;**书写器**。
1. 单击&#x200B;**保存**&#x200B;以保存更改。 无需创建变体，除非您愿意！

   ![其他内容片段](assets/author-content-fragments/additional-content-fragment.png)

   您现在应当有两个&#x200B;**参与者**&#x200B;片段。

## 恭喜！{#congratulations}

恭喜您，您刚刚创作了多个内容片段并创建了一个变体。

## 后续步骤{#next-steps}

在下一章[浏览GraphQL API](explore-graphql-api.md)中，您将使用内置的GrapiQL工具浏览AEM GraphQL API。 了解AEM如何根据内容片段模型自动生成GraphQL模式。 您将尝试使用GraphQL语法构建基本查询。