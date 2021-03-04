---
title: '填充自适应表单表 '
seo-title: 填充自适应表单表
description: 使用表单数据模型服务调用的结果填充自适应表单表
seo-description: 使用表单数据模型服务调用的结果填充自适应表单表
feature: 自适应表单
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 0%

---


# 使用表单数据模型服务调用的结果填充自适应表单表

[此处托管实时表](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
单在本文中，我们查看如何通过从表单数据模型服务调用中获取数据来填充自适应表单表。我们将在表格中创建一个摊销计划，以列表每笔定期按揭的付款。 摊销结果由表单数据模型返回。 按照屏幕截图所示，在计算按钮的单击事件上调用表单数据模型的服务。 正确映射服务调用的输入和输出参数，如屏幕快照中所示。 输出将映射到行1的列
![clickevent](assets/amortization.PNG)

将行1配置为根据服务调用返回的数据而增大。 请注意此处指定的重复设置。 值为–1表示表中行数不限
![行1](assets/rowconfiguration.PNG)

## 在您的服务器上部署此组件

[按此处指定安](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[装Tomcat部署SampleRest.war文](https://forms.enablementadobe.com/content/DemoServerBundles/SampleRest.war)
[件使用AEM包管 ](assets/amortizationschedule.zip) 理器安装资产打
[开摊销计划表单输入相应值并单](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
击计算应在表单中填充摊销计划

