---
title: 签名多文档疑难解答解决方案
description: 测试解决方案并排除故障
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-6960
thumbnail: 6960.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: 99cba29e-4ae3-4160-a4c7-a5b6579618c0
duration: 109
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '382'
ht-degree: 0%

---

# 测试和故障排除


## 预览再融资表单

用例在客户服务代理填写并提交时触发 [再融资表单](http://localhost:4502/content/dam/formsanddocuments/formsandsigndemo/refinanceform/jcr:content?wcmmode=disabled).

签署多个Forms工作流将在提交此表单时触发，客户将收到一封电子邮件通知，其中包含开始表单填写和签署流程的链接。

## 在程序包中填写表单

向客户演示如何填写并签署包中的第一个表单。 成功签署表单后，客户可以导航到资源包中的下一个表单。 填写并签署所有表单后，向客户显示“**全部完成**”表单。

## 疑难解答

### 未生成电子邮件通知

电子邮件通知由签名多表单工作流中的发送电子邮件组件发送。 如果此工作流中的任何步骤失败，则会发送电子邮件通知。 确保工作流中的自定义流程步骤正在MySQL数据库中创建行。 如果正在创建行，请检查您的Day CQ邮件服务配置设置

### 电子邮件通知中的链接不起作用

电子邮件通知中的链接是动态生成的。 如果您的AEM服务器未在localhost：4502上运行，请在“签名多个Forms”工作流的“将Forms存储为签名”步骤的参数中提供正确的服务器名称和端口

### 无法签署表单

如果通过从数据源获取数据而未能正确填充表单，则可能会发生这种情况。 检查服务器的stdout日志。 fetchformdata.jsp将一些有用的消息写出到stdout。

### 无法导航到资源包中的下一个表单

成功签署包中的表单时，会触发更新签名状态工作流。 工作流中的第一步会更新数据库中表单的签名状态。 请检查表单的状态是否从0更新为1。

### 未看到AllDone表单

当没有更多表单可登录包时，会向用户显示AllDone表单。如果您未看到AllDone表单，请检查GetNextFormToSign.js文件的第33行中使用的URL，该文件是 **getnextform** 客户端库。
