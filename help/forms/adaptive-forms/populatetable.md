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
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '240'
ht-degree: 0%

---

# 使用表单数据模型服务调用的结果填充自适应表单表

[在此处托管实时表单](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
在本文中，我们通过从表单数据模型服务调用中获取数据来查看填充自适应表单表。 我们将在表格中创建一个摊销时间表，列出一段时间内每一笔抵押贷款的定期付款。 摊销结果由我们的表单数据模型返回。 表单数据模型的服务将在计算按钮的点击事件中调用，如屏幕截图所示。 服务调用的输入和输出参数被正确映射，如屏幕快照中所示。 输出已映射到Row1的列
![clickevent](assets/amortization.PNG)

Row1配置为根据服务调用返回的数据而增长。 请注意此处指定的重复设置。 值为–1表示表中的行数不限
![Row1](assets/rowconfiguration.PNG)

## 在您的服务器上部署此组件

[按照此处指定的方式安装Tomcat](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[部署Tomcat中此zip文件中包含的SampleRest.war文件](assets/sample-rest.zip)
[安装资产 ](assets/amortizationschedule.zip) 使用AEM包管理器
[打开摊销计划表](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
输入相应的值并单击计算摊销计划应填充在您的表单中
