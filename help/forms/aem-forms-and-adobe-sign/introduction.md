---
title: 将AEM Forms与Acrobat Sign结合使用
description: Acrobat Sign和AEM Forms可自动执行复杂的交易，并包括法律电子签名，作为无缝数字体验的一部分。
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 0be61f04-c3ed-4ecb-bab7-73fd308c14e0
last-substantial-update: 2020-03-20T00:00:00Z
source-git-commit: 81b96f59450448a3d5b17a61aa025acd60d0cce1
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 11%

---

# 将AEM Forms与Acrobat Sign结合使用

Acrobat Sign支持自适应表单的电子签名工作流。 电子签名改进了法律、销售、工资单、人力资源管理和其他许多方面的文档的处理工作流。AEM Forms与Acrobat Sign之间的集成将允许您执行以下操作

* 使用自适应Forms捕获数据并显示自动生成的记录文档(DoR)以供签名
* 根据您的PDF模板创建自适应Forms。 将数据与PDF模板合并，并对签名提供相同的内容
* 使用签名文档工作流组件发送文档以供签名

## 前提条件

要将Acrobat Sign与AEM Forms集成，您需要以下项：

* 启用SSL的AEM Forms服务器
* 有效的Acrobat Sign开发人员帐户。
* Acrobat Sign API应用程序
* Acrobat Sign API应用程序的凭据（客户端ID和客户端密码）。
