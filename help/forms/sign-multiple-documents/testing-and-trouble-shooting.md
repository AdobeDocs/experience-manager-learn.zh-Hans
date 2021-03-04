---
title: 对多个文档解决方案进行签名疑难解答
description: 测试和故障拍摄解决方案
feature: 自适应表单
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6960
thumbnail: 6960.jpg
topic: 开发
role: 开发人员
level: 中间
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 1%

---


# 测试和疑难解答


## 预览再融资表单

当客户服务代理填写并提交[对表单](http://localhost:4502/content/dam/formsanddocuments/formsandsigndemo/refinanceform/jcr:content?wcmmode=disabled)进行再融资时，将触发用例。

对多个Forms工作流在提交此表单时会触发该工作流，客户会收到一封电子邮件通知，其中包含一个链接，用于开始表单填写和签名流程。

## 在包中填写表单

客户将填写并签署包中的第一个表单。 成功签署表单后，客户可以导航到包中的下一个表单。 填写所有表单并对其签名后，客户会看到“**AllDone**”表单。

## 故障诊断

### 未生成电子邮件通知

电子邮件通知由“签名多个表单”工作流中的“发送电子邮件”组件发送。 如果此工作流中的任何步骤失败，将发送电子邮件通知。 确保工作流中的自定义进程步骤正在MySQL数据库中创建行。 如果创建行，请检查Day CQ邮件服务配置设置

### 电子邮件通知中的链接无效

电子邮件通知中的链接会动态生成。 如果您的AEM服务器未在localhost:4502上运行，请在“对多个Forms进行签名”工作流的“要签名的存储Forms”步骤的参数中提供正确的服务器名称和端口

### 无法对表单进行签名

如果从数据源获取数据时未正确填充表单，则会发生这种情况。 检查服务器的静态日志。 fetchformdata.jsp将一些有用的消息写入stdout。

### 无法导航到包中的下一个表单

成功签署包中的表单后，将触发更新签名状态工作流。 工作流中的第一步更新数据库中表单的签名状态。 请检查表单的状态是否从0更新为1。

### 看不到AllDone表单

当包中没有可登录的表单时，AllDone表单将显示给用户。如果您没有看到AllDone表单，请检查作为&#x200B;**getnextform**&#x200B;客户端库的一部分的GetNextFormToSign.js文件第33行中使用的URL。











