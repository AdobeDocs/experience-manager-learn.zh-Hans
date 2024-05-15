---
title: 配置投资组合面板
description: 这是创建您的第一个交互式通信文档的多步教程的第11部分。在本部分中，我们将添加饼图以显示当前和模型投资组合。
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 77de4e76-53ee-467c-a71c-d1d3ea15283b
topic: Development
role: Developer
level: Beginner
exl-id: 774d7a6e-2b8f-4a70-98c5-e7712478ff75
duration: 69
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 0%

---

# 配置投资组合面板

在本部分中，我们将添加饼图以显示当前和模型投资组合。

* 登录到AEM Forms，然后导航至Adobe Experience Manager > Forms > Forms和文档。

* 打开401KStatement文件夹。

* 在编辑模式下打开401KStatement。

* 我们将添加两张饼图，以代表账户持有人的当前和模型投资组合。

## 当前资产组合 {#current-asset-mix}

* 点按右侧的“CurrentAssetMix”面板，然后选择“+”图标并插入文本组件。 将默认文本更改为“当前资产组合”。

* 点按“CurrentAssetMix”面板，然后选择“+”图标并插入图表组件。 点按新插入的图表组件，然后单击“扳手”图标以打开图表的配置属性表。

* 设置属性，如下图所示。 确保您的图表类型为“饼图”。

* 请注意绑定到X轴和Y轴的数据模型对象。 您需要选择表单数据模型的根元素，然后向下钻取以选择相应的元素。

* ![currentassetmix](assets/currentassetmixchart.png)

## 模型资产组合 {#model-asset-mix}

* 点按右侧的“RecommendedAssetMix”面板，然后选择“+”图标并插入文本组件。 将默认文本更改为“模型资产组合”。

* 点按“RecommendedAssetMix”面板，然后选择“+”图标并插入图表组件。 点按新插入的图表组件，然后单击“扳手”图标以打开图表的配置属性表。

* 设置属性，如下图所示。 确保您的图表类型为“饼图”。

* 请注意绑定到X轴和Y轴的数据模型对象。 您需要选择表单数据模型的根元素，然后向下钻取以选择相应的元素。

* ![assettype](assets/modelassettypechart.png)

## 后续步骤

[准备投放Web渠道文档](./parttwelve.md)
