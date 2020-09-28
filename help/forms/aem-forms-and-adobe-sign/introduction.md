---
title: 将AEM Forms与Adobe Sign
description: Adobe Sign和AEM Forms使复杂交易实现自动化，并将合法电子签名作为无缝数字体验的一部分。
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: c60a46027cc8d71fddd41aa31dbb569e4df94823
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 0%

---

# 将AEM Forms与Adobe Sign

Adobe Sign为自适应表单启用电子签名工作流。 电子签名可改善处理法律、销售、工资、人力资源管理等文档的工作流。
AEM Forms和Adobe Sign的整合将允许您执行以下操作

* 使用自适应Forms捕获数据并呈现自动生成的记录文档(DoR)以进行签名
* 根据PDF模板创建自适应Forms。 将数据与pdf模板合并，并为签名提供相同的内容
* 使用“签名文档”工作流组件发送要签名的文档

## 前提条件

您需要以下条件将Adobe Sign与AEM Forms集成：

* 启用SSL的AEM Forms服务器
* 一个活跃的Adobe Sign开发者帐户。
* Adobe SignAPI应用程序
* Adobe SignAPI应用程序的凭据（客户端ID和客户端机密）。

