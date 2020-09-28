---
title: 配置Retirement Outlook面板
seo-title: 配置Retirement Outlook面板
description: 这是创建第一个交互式通信文档的多步教程的第10部分。 在此部分，我们将通过添加文本和图表组件来配置Retirement Outlook Panel。
seo-description: 这是创建第一个交互式通信文档的多步教程的第10部分。 在此部分，我们将通过添加文本和图表组件来配置Retirement Outlook Panel。
uuid: 1d5119b5-e797-4bf0-9b10-995b3f051f92
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 2ee2cea2-aefa-4d21-a258-248648f73a68
translation-type: tm+mt
source-git-commit: f6a9c0522219614b87c483504c9c64875f2e4286
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 0%

---


# 配置Retirement Outlook面板{#configuring-retirement-outlook-panel}

* 这是创建第一个交互式通信文档的多步教程的第10部分。 在此部分，我们将通过添加文本和图表组件来配置Retirement Outlook Panel。

* 登录AEM Forms并导航到Adobe Experience Manager>Forms>Forms和文档。

* 打开401KStatement文件夹。

* 在编辑模式下打开401KStatement文档。

**配置LeftPanel目标区**

* 点按右侧的LeftPanel目标区域，然后单击“+”图标以显示插入组件对话框。

* 插入文本组件。

* 轻轻点按新添加的文本组件以显示组件工具栏

* 选择“铅笔”图标以编辑默认文本。

* 将默认文本替换&#x200B;**为“您的退休收入展望”**

**配置RightPanel目标区域**

* 点按右侧的RightPanel目标区域，然后单击“+”图标以显示插入组件对话框。

* 插入文本组件。

* 轻轻点按新添加的文本组件以显示组件工具栏。

* 选择“铅笔”图标以编辑默认文本。

* 将默认文本替换为“估&#x200B;**计月退休收入”**

## 添加退休收入展望文档片段 {#add-retirement-income-outlook-document-fragment}

* 单击资产图标，然后应用筛选器以显示“文档片段”类型的资产。 将RetimentIncomeOutlook文档片段拖放到左面板目标区。

* 有关将文档 [片段添加到内容](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-web-channel-aem-forms/9.html) 区域，请参阅本页。

## 添加估计的月收入图表 {#adding-estimated-monthly-income-chart}

* 单击右侧的RightPanel目标区域。 单击“+”图标以插入图表组件。 我们用柱状图来显示月度收入的估计值。 轻轻点按新插入的图表组件。 选择“扳手”图标以打开配置属性表。请按照以下屏幕截图所示，使用以下属性配置图表。

**AEM Forms6.4 —— 配置月度收入估计列表**

![form64](assets/estimatedmonthlyincomechart.png)

**AEM Forms6.5 —— 配置月度收入估计列表**

![forms65](assets/estimatedmonthlyincomechart65.PNG)




