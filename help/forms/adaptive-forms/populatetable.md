---
title: 填充自适应表单表
description: 使用表单数据模型服务调用的结果填充自适应表单表
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: User
level: Intermediate
exl-id: 6e4b901a-6534-4c34-b315-2f2620b74247
last-substantial-update: 2019-06-09T00:00:00Z
duration: 64
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---

# 使用表单数据模型服务调用的结果填充自适应表单表

[实时表单托管于此处](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
本文主要研究如何通过从表单数据模型服务调用中提取数据来填充自适应表单表。 我们将在表格中创建一个摊销时间表，其中列出一段时间内抵押贷款的每次定期付款。 摊销结果由我们的表单数据模型返回。 在单击计算按钮的事件时调用表单数据模型的服务，如屏幕快照中所示。 如屏幕快照中所示，服务调用的输入和输出参数被适当地映射。 输出映射到Row1的列
![clickevent](assets/amortization.PNG)

Row1配置为根据服务调用返回的数据而增大。 请注意此处指定的重复设置。 值为–1表示表中的行数不受限制
![Row1](assets/rowconfiguration.PNG)

## 在您的服务器上部署此项

[按照此处指定的方式安装Tomcat](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[将此zip文件中包含的SampleRest.war文件部署到Tomcat中](assets/sample-rest.zip)
[安装资产](assets/amortizationschedule.zip) 使用AEM包管理器
[打开摊销计划表单](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
输入相应的值，然后单击计算摊销计划应填充到您的表单中
