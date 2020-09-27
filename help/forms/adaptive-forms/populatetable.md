---
title: '填充自适应表单表 '
seo-title: 填充自适应表单表
description: 使用表单数据模型服务调用的结果填充自适应表单表
seo-description: 使用表单数据模型服务调用的结果填充自适应表单表
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 449202af47b6bbcd9f860d5c5391d1f7096d489e
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 0%

---


# 使用表单数据模型服务调用的结果填充自适应表单表

[此处托管实时表](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)单在本文中，我们通过从表单数据模型服务调用中获取数据来查看如何填充自适应表单表。 我们将在一张表格中创建一个摊销计划，对每笔定期的抵押贷款进行列表。 摊销结果由我们的表单数据模型返回。 如屏幕截图所示，单击计算按钮事件将调用表单数据模型的服务。 正确映射服务调用的输入和输出参数，如屏幕快照中所示。 输出将映射到Row1类活动的列![。](assets/amortization.PNG)

将行1配置为根据服务调用返回的数据增大。 请注意此处指定的重复设置。 值-1表示表Row1中行数不限![。](assets/rowconfiguration.PNG)

## 在您的服务器上部署此组件

[按此处指定安装](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)[Tomcat部署SampleRest.war文件使用AEM包管理器安](https://forms.enablementadobe.com/content/DemoServerBundles/SampleRest.war)装资产打[开摊销计划表 ](assets/amortizationschedule.zip)[](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)输入相应的值并单击计算摊销计划应填充到您的表单中

