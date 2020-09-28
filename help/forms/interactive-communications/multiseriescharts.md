---
title: AEM Forms多序列图
seo-title: AEM Forms多序列图
description: 创建适当的表单渠道模型以在打印和Web文档中创建多系列图表。
seo-description: 创建适当的表单渠道模型以在打印和Web文档中创建多系列图表。
feature: interactive-communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---


# 多系列图表

AEM Forms6.5引入了创建和配置多个系列图表的功能。 多个系列图表通常与线、条、柱状图类型关联使用。 下图是多系列图的一个好示例。 图表显示了3个不同的共同基金在一段时间内1万美元的增长。 要能够在AEM Forms创建和使用此类图表，您需要创建适当的表单数据模型。

![多序列](assets/seriescharts.jfif)

要在AEM Forms创建多系列图表，您需要创建具有必要实体和实体之间关联的适当表单数据模型。 以下屏幕截图突出显示了实体以及3个实体之间的关联。 在顶层，我们有一个名为&quot;组织&quot;的实体，它与基金实体有一对多的关联。 基金实体又与业绩实体有一对多关系。

![formdatamodel](assets/formdatamodel.jfif)


## 创建多序列图表的表单数据模型

>[!VIDEO](https://video.tv.adobe.com/v/26352/quality=9)


### 配置线型系列图

>[!VIDEO](https://video.tv.adobe.com/v/26353?quality=9&learn=on)


要在系统上测试它，请按照以下步骤操作

* [使用AEM包管理器下载并导入MutualFundFactSheet.zip。](assets/mutualfundfactsheet.zip)
* [将SeriesChartSampleData.json下载到您的硬盘上。](assets/serieschartsampledata.json) 这是用于填充图表的示例数据。
* [导航到Forms和文档。](https://helpx.adobe.com/aem/forms.html/content/dam/formsanddocuments.html)
* 轻轻选择“MutualFundGrowthFactSheet”交互式通信模板。
* 单击预览 |上传示例数据。
* 浏览到作为本文一部分提供的示例数据文件。
* 预览“MutualFundGrowthFactSheet”交互通信的打印渠道与上一步下载的样本数据。
