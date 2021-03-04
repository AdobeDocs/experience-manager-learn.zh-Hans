---
title: AEM Forms中的多系列图
seo-title: AEM Forms中的多系列图
description: 创建适当的表单数据模型以在打印和Web渠道文档中创建多系列图表。
seo-description: 创建适当的表单数据模型以在打印和Web渠道文档中创建多系列图表。
feature: 交互式通信
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: 开发
role: 开发人员
level: 初学者
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 1%

---


# 多系列图表

AEM Forms 6.5引入了创建和配置多个系列图表的功能。 多个系列图表通常与Line、Bar、Column图表类型关联使用。 下图是多系列图的一个好示例。 图表显示，在一段时间内，3个不同的共同基金中，1万美元的增长。 要在AEM Forms中创建和使用此类图表，您需要创建适当的表单数据模型。

![多系列](assets/seriescharts.jfif)

要在AEM Forms中创建多系列图表，您需要创建一个具有必要实体和实体之间关联的适当表单数据模型。 以下屏幕截图突出显示了3个实体之间的实体和关联。 在最高层，我们有一个名为&quot;组织&quot;的实体，它与基金实体有一对多的联系。 基金实体与业绩实体有一对多的关联。

![formdatamodel](assets/formdatamodel.jfif)


## 创建多系列图表的表单数据模型

>[!VIDEO](https://video.tv.adobe.com/v/26352/quality=9)


### 配置线系图

>[!VIDEO](https://video.tv.adobe.com/v/26353?quality=9&learn=on)


要在系统上测试它，请执行以下步骤

* [使用AEM包管理器下载并导入MutualFundFactSheet.zip。](assets/mutualfundfactsheet.zip)
* [将SeriesChartSampleData.json下载到您的硬盘上。](assets/serieschartsampledata.json) 这是用于填充图表的示例数据。
* [导航到Forms和文档。](https://helpx.adobe.com/aem/forms.html/content/dam/formsanddocuments.html)
* 轻轻选择“MutualFundGrowthFactSheet”交互式通信模板。
* 单击预览 |上传示例数据。
* 浏览到作为本文一部分提供的示例数据文件。
* 预览“MutualFundGrowthFactSheet”交互式通信的打印渠道与上一步下载的样本数据。
