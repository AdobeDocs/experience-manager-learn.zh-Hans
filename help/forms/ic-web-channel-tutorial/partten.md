---
title: 配置Retirement Outlook面板
seo-title: Configuring Retirement Outlook Panel
description: 这是创建首个交互式通信文档的多步教程的10部分。 在本部分中，我们将通过添加文本和图表组件来配置Reriement Outlook面板。
seo-description: This is part 10 of a multi-step tutorial for creating your first interactive communications document. In this part, we will configure Retirement Outlook Panel by adding text and chart components.
uuid: 1d5119b5-e797-4bf0-9b10-995b3f051f92
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 2ee2cea2-aefa-4d21-a258-248648f73a68
topic: Development
role: Developer
level: Beginner
source-git-commit: 0049c9fd864bd4dd4f8c33b1e40e94aad3ffc5b9
workflow-type: tm+mt
source-wordcount: '344'
ht-degree: 0%

---


# 配置Retirement Outlook面板{#configuring-retirement-outlook-panel}

* 这是创建首个交互式通信文档的多步教程的10部分。 在本部分中，我们将通过添加文本和图表组件来配置Reriement Outlook面板。

* 登录AEM Forms，然后导航到Adobe Experience Manager > Forms > Forms和文档。

* 打开401KStatement文件夹。

* 在编辑模式下打开401KStatement文档。

**配置LeftPanel目标区域**

* 点按右侧的LeftPanel目标区域，然后单击“+”图标以调出插入组件对话框。

* 插入文本组件。

* 轻轻点按新添加的文本组件，以显示组件工具栏

* 选择“铅笔”图标以编辑默认文本。

* 将默认文本替换为“**Your Retiement Inceme Outlook”**

**配置RightPanel目标区域**

* 点按右侧的RightPanel目标区域，然后单击“+”图标以调出插入组件对话框。

* 插入文本组件。

* 轻轻点按新添加的文本组件，以显示组件工具栏。

* 选择“铅笔”图标以编辑默认文本。

* 将默认文本替换为“**估计每月退休收入”**

## 添加退休收入展望文档片段 {#add-retirement-income-outlook-document-fragment}

* 单击资产图标，然后应用过滤器以显示“文档片段”类型的资产。 将RetiementIncomeOutlook文档片段拖放到左面板目标区域。

* 在向内容区域添加文档片段时，您可以参阅[到此页面](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/partseven.html)。

## 添加预计月收入图 {#adding-estimated-monthly-income-chart}

* 单击右侧的RightPanel目标区域。 单击“+”图标以插入图表组件。 我们将用柱状图显示月收入估计值。 轻轻点按新插入的图表组件。 选择“扳手”图标以打开配置属性工作表。如下面的屏幕截图所示，使用以下属性配置图表。

**AEM Forms 6.4 — 配置预计每月收入列表**

![form64](assets/estimatedmonthlyincomechart.png)

**AEM Forms 6.5 — 配置预计每月收入列表**

![forms65](assets/estimatedmonthlyincomechart65.PNG)




