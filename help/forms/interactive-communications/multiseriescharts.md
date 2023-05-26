---
title: AEM Forms中的多系列图表
description: 创建相应的表单数据模型，以便在打印文档和Web渠道文档中创建多系列图表。
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f4af7cb9-cc3b-4bec-9428-ab4f1a3cf41a
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 0%

---

# 多系列图表

AEM Forms 6.5引入了创建和配置多系列图表的功能。 多系列图表通常与折线图、条形图、柱状图类型结合使用。 以下图表是多系列图表的良好示例。 图表显示了3个不同共同基金在一段时期内增长1万美元的情况。 为了能够在AEM Forms中创建和使用此类图表，您需要创建相应的表单数据模型。

![多系列图表](assets/seriescharts.jfif)

要在AEM Forms中创建多系列图表，您需要创建相应的表单数据模型，其中包含必要的实体和实体之间的关联。 以下屏幕截图突出显示实体和3个实体之间的关联。 在最顶层，我们有一个称为“组织”的实体，它与Fund实体有一对多关系。 而基金实体则与业绩实体有一对多关系。

![表单数据模型](assets/formdatamodel.jfif)

## 为多系列图表创建表单数据模型

>[!VIDEO](https://video.tv.adobe.com/v/26352?quality=12&learn=on)

### 配置折线图

>[!VIDEO](https://video.tv.adobe.com/v/26353?quality=12&learn=on)

要在您的系统上对此进行测试，请执行以下步骤

* [使用AEM包管理器下载并导入MutualFundFactSheet.zip。](assets/mutualfundfactsheet.zip)
* [将SeriesChartSampleData.json下载到硬盘上。](assets/serieschartsampledata.json) 这是用于填充图表的示例数据。
* [导航到Forms和文档。](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 轻轻选择“MutualFundGrowthFactSheet”交互式通信模板。
* 单击预览 |打印渠道 |上传示例数据。
* 浏览到本文中提供的示例数据文件。
* 使用上一步中下载的示例数据预览“MutualFundGrowthFactSheet”交互式通信的打印渠道。
