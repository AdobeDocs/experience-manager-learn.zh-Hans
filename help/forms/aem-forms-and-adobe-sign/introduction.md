---
title: 将AEM Forms与Adobe Sign结合使用
description: Adobe Sign和AEM Forms可自动执行复杂的交易，并将合法的电子签名作为无缝数字体验的一部分。
feature: 自适应Forms,Adobe Sign
version: 6.4,6.5
topic: 开发
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '163'
ht-degree: 1%

---

# 将AEM Forms与Adobe Sign结合使用

Adobe Sign为自适应表单启用电子签名工作流。 电子签名可改进工作流，以处理法律、销售、工资单、人力资源管理等许多领域的文档。
AEM Forms与Adobe Sign之间的集成将允许您执行以下操作

* 使用自适应Forms捕获数据并呈现自动生成的记录文档(DoR)以供签名
* 根据PDF模板创建自适应Forms。 将数据与PDF模板合并，并提供给签名
* 使用“签名文档”工作流组件发送文档以进行签名

## 前提条件

要将Adobe Sign与AEM Forms集成，您需要满足以下条件：

* 启用SSL的AEM Forms服务器
* 有效的Adobe Sign开发人员帐户。
* Adobe Sign API应用程序
* Adobe Sign API应用程序的凭据（客户端ID和客户端密钥）。

