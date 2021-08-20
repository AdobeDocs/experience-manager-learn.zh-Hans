---
title: '填充自适应表单表 '
description: 使用表单数据模型服务调用的结果填充自适应表单表
feature: 自适应表单
version: 6.4,6.5
topic: 开发
role: User
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '240'
ht-degree: 1%

---


# 使用表单数据模型服务调用的结果填充自适应表单表

[将在此处托管实](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
时表单在本文中，我们通过从表单数据模型服务调用中获取数据来查看如何填充自适应表单表。我们将在表格中创建一个摊销时间表，列出一段时间内每一笔抵押贷款的定期付款。 摊销结果由我们的表单数据模型返回。 表单数据模型的服务将在计算按钮的点击事件中调用，如屏幕截图所示。 服务调用的输入和输出参数被正确映射，如屏幕快照中所示。 输出已映射到Row1的列
![clickevent](assets/amortization.PNG)

Row1配置为根据服务调用返回的数据而增长。 请注意此处指定的重复设置。 值为–1表示表中的行数不限
![Row1](assets/rowconfiguration.PNG)

## 在您的服务器上部署此组件

[按照此处指定的](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[方式安装Tomcat部署SampleRest.war](https://forms.enablementadobe.com/content/DemoServerBundles/SampleRest.war)
[文件使用AEM包管理器 ](assets/amortizationschedule.zip) 安装资产打
[开摊销计划表单输入相](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
应的值并单击计算摊销计划应在您的表单中填充

