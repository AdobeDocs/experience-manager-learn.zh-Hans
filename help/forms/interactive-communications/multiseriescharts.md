---
title: AEM Forms中的多系列图
seo-title: AEM Forms中的多系列图
description: 创建适当的表单数据模型，以在打印和Web渠道文档中创建多系列图表。
seo-description: 创建适当的表单数据模型，以在打印和Web渠道文档中创建多系列图表。
feature: 交互式通信
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: 开发
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '304'
ht-degree: 1%

---


# 多系列图

AEM Forms 6.5引入了创建和配置多个系列图的功能。 多个系列图通常与折线图、条形图、柱状图类型相关联使用。 下图是多系列图的一个好示例。 图表显示，在一段时间内，3个不同的共同基金中，1万美元的资金增长。 要在AEM Forms中创建和使用此类图表，您需要创建相应的表单数据模型。

![多序列](assets/seriescharts.jfif)

要在AEM Forms中创建多系列图，您需要创建一个具有必要实体和实体间关联的相应表单数据模型。 以下屏幕截图突出显示了实体以及3个实体之间的关联。 在顶层，我们有一个名为“组织”的实体，该实体与基金实体有一对多的关联。 基金实体与业绩实体有一对多的关联。

![formdatamodel](assets/formdatamodel.jfif)


## 为多系列图创建表单数据模型

>[!VIDEO](https://video.tv.adobe.com/v/26352/quality=9)


### 配置折线图

>[!VIDEO](https://video.tv.adobe.com/v/26353?quality=9&learn=on)


要在系统上测试此功能，请执行以下步骤

* [使用AEM包管理器下载和导入MutualFundFactSheet.zip。](assets/mutualfundfactsheet.zip)
* [将SeriesChartSampleData.json下载到您的硬盘。](assets/serieschartsampledata.json) 这是用于填充图表的示例数据。
* [导航到Forms和文档。](https://helpx.adobe.com/aem/forms.html/content/dam/formsanddocuments.html)
* 轻轻选择“MutualFundGrowthFactSheet”交互式通信模板。
* 单击预览 |上载示例数据。
* 浏览到本文中提供的样例数据文件。
* 预览“MutualFundGrowthFactSheet”交互式通信的打印渠道，其中包含在上一步中下载的示例数据。
