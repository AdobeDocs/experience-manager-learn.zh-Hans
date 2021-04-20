---
title: 将AEM Forms与Adobe Sign结合使用
description: Adobe Sign和AEM Forms让复杂交易实现自动化，并将合法的电子签名作为无缝数字体验的一部分。
feature: Adaptive Forms,Adobe Sign
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 1%

---

# 将AEM Forms与Adobe Sign结合使用

Adobe Sign支持自适应表单的电子签名工作流。 电子签名可改善处理法律、销售、工资、人力资源管理等领域文档的工作流。
AEM Forms和Adobe Sign之间的集成允许您执行以下操作

* 使用自适应Forms捕获数据并为签名呈现自动生成的记录文档(DoR)
* 根据您的PDF模板创建自适应Forms。 将数据与pdf模板合并，并为签名提供相同的内容
* 使用Sign文档工作流组件发送要签名的文档

## 前提条件

您需要以下内容才能将Adobe Sign与AEM Forms集成：

* 启用SSL的AEM Forms服务器
* 一个有效的Adobe Sign开发人员帐户。
* Adobe Sign API应用程序
* Adobe Sign API应用程序的凭据（客户端ID和客户端机密）。

