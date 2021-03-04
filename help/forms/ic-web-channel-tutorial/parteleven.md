---
title: 配置投资组合面板
seo-title: 配置投资组合面板
description: 这是创建您的第一个交互式通信文档的多步教程的第11部分。在本部分中，我们将添加饼图以显示当前和模型投资组合。
seo-description: 这是创建您的第一个交互式通信文档的多步教程的第11部分。在本部分中，我们将添加饼图以显示当前和模型投资组合。
uuid: b0132912-cb6e-4dec-8309-5125d29ad291
feature: 交互式通信
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 77de4e76-53ee-467c-a71c-d1d3ea15283b
topic: 开发
role: 开发人员
level: 初学者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 1%

---


# 配置投资组合面板

在这部分中，我们将添加饼图来显示当前和模型投资组合。

* 登录AEM Forms并导航到Adobe Experience Manager > Forms > Forms和文档。

* 打开401KStatement文件夹。

* 在编辑模式下打开401KStatement。

* 我们将增加2张饼图，以代表账户持有人目前及模型投资组合。

## 当前资产组合{#current-asset-mix}

* 点按右侧的“CurrentAssetMix”面板，选择“+”图标并插入文本组件。 将默认文本更改为“当前资产混合”。

* 点按“CurrentAssetMix”面板，选择“+”图标并插入图表组件。 点按新插入的图表组件并单击“扳手”图标以打开图表的配置属性工作表。

* 如下图所示设置属性。 确保图表类型为饼图。

* 请注意绑定到X和Y轴的数据模型对象。 您需要选择表单数据模型的根元素，然后向下展开以选择相应的元素。

* ![currentassetmix](assets/currentassetmixchart.png)

## 模型资产混合{#model-asset-mix}

* 点按右侧的“RecommendedAssetMix”面板，选择“+”图标并插入文本组件。 将默认文本更改为“Model Asset Mix”。

* 点按“RecommendedAssetMix”面板，选择“+”图标并插入图表组件。 点按新插入的图表组件并单击“扳手”图标以打开图表的配置属性工作表。

* 如下图所示设置属性。 确保图表类型为饼图。

* 请注意绑定到X和Y轴的数据模型对象。 您需要选择表单数据模型的根元素，然后向下展开以选择相应的元素。

* ![assettype](assets/modelassettypechart.png)

