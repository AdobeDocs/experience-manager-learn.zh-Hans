---
title: 配置“停用展望”面板
description: 这是创建您的第一个交互式通信文档的多步教程的第10部分。 在本部分中，我们将通过添加文本和图表组件来配置“停用展望”面板。
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 2ee2cea2-aefa-4d21-a258-248648f73a68
topic: Development
role: Developer
level: Beginner
exl-id: 0dd8a430-9a4e-4dc7-ad75-6ad2490430f2
duration: 71
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '347'
ht-degree: 0%

---

# 配置“停用展望”面板{#configuring-retirement-outlook-panel}

* 这是创建您的第一个交互式通信文档的多步教程的第10部分。 在本部分中，我们将通过添加文本和图表组件来配置“停用展望”面板。

* 登录到AEM Forms，然后导航至Adobe Experience Manager > Forms > Forms和文档。

* 打开401KStatement文件夹。

* 在编辑模式下打开401KStatement文档。

**配置LeftPanel目标区域**

* 点按右侧的LeftPanel目标区域，然后单击“+”图标以显示“插入组件”对话框。

* 插入文本组件。

* 轻点按新添加的文本组件以调出组件工具栏

* 选择“铅笔”图标以编辑默认文本。

* 将默认文本替换为“**您的退休收入展望”**

**配置RightPanel目标区域**

* 点按右侧的RightPanel目标区域，然后单击“+”图标以显示“插入组件”对话框。

* 插入文本组件。

* 轻点按新添加的文本组件以调出组件工具栏。

* 选择“铅笔”图标以编辑默认文本。

* 将默认文本替换为“**预计每月退休收入”**

## 添加退休收入Outlook文档片段 {#add-retirement-income-outlook-document-fragment}

* 单击Assets图标并应用过滤器以显示“文档片段”类型的资源。 将RetirationIncomeOutlook文档片段拖放到左侧面板目标区域。

* 在将文档片段添加到内容区域时，您可以参考[此页面](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/partseven.html)。

## 添加预计每月收入图表 {#adding-estimated-monthly-income-chart}

* 单击右侧的RightPanel目标区域。 单击“+”图标可插入图表组件。 我们将使用柱状图显示预计的每月收入。 轻点按新插入的图表组件。 选择“扳手”图标以打开配置属性表。使用以下属性配置图表，如下面的屏幕快照所示。

**AEM Forms 6.4 — 配置预计月收入柱状图**

![表单64](assets/estimatedmonthlyincomechart.png)

**AEM Forms 6.5 — 配置预计月收入柱状图**

![表单65](assets/estimatedmonthlyincomechart65.PNG)

## 后续步骤

[配置饼图](./parteleven.md)
