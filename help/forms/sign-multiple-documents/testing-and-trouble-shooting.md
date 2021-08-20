---
title: 对多个文档解决方案进行签名故障诊断
description: 测试和故障排除解决方案
feature: 自适应表单
version: 6.4,6.5
kt: 6960
thumbnail: 6960.jpg
topic: 开发
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '392'
ht-degree: 1%

---


# 测试和故障诊断


## 预览再融资表单

当客户服务代理填写并提交[再融资表单](http://localhost:4502/content/dam/formsanddocuments/formsandsigndemo/refinanceform/jcr:content?wcmmode=disabled)时，将触发用例。

在提交此表单时，签署多个Forms工作流会触发，客户会收到一封包含链接的电子邮件通知，以开始表单填写和签名流程。

## 在包中填写表单

客户将填写并签署包中的第一个表单。 成功签名表单后，客户可以导航到包中的下一个表单。 填写所有表单并对其签名后，客户将看到“**AllDone**”表单。

## 故障诊断

### 未生成电子邮件通知

电子邮件通知由签名多表单工作流中的发送电子邮件组件发送。 如果此工作流中的任何步骤失败，则会发送电子邮件通知。 确保工作流中的自定义进程步骤正在MySQL数据库中创建行。 如果正在创建行，请检查Day CQ Mail Service配置设置

### 电子邮件通知中的链接不起作用

电子邮件通知中的链接是动态生成的。 如果您的AEM服务器未在localhost:4502上运行，请在“签名多个Forms”工作流的“要签名的存储Forms”步骤的参数中提供正确的服务器名称和端口

### 无法签署表单

如果表单未通过从数据源获取数据来正确填充，则可能会发生这种情况。 检查服务器的状态日志。 fetchformdata.jsp将一些有用的消息写入stdout。

### 无法导航到包中的下一个表单

成功签署包中的表单后，将触发更新签名状态工作流。 工作流中的第一步是更新数据库中表单的签名状态。 请检查表单的状态是否从0更新为1。

### 未看到AllDone表单

当包中没有可登录的表单时，将向用户显示AllDone表单。如果您没有看到AllDone表单，请检查GetNextFormToSign.js文件第33行中使用的URL，该文件是&#x200B;**getnextform**&#x200B;客户端库的一部分。











