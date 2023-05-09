---
title: 从MySQL数据库存储和检索带有附件的表单数据
description: 多部分教程，指导您完成使用附件存储和检索表单数据时涉及的步骤
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6593
thumbnail: 327122.jpg
topic: Development
role: Developer
level: Experienced
exl-id: b278652f-6c09-4abc-b92e-20bfaf2e791a
last-substantial-update: 2020-11-07T00:00:00Z
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '212'
ht-degree: 2%

---

# 2FA自适应表单数据的存储与检索

本教程将指导您完成使用2FA保存和检索带有附件的自适应表单数据时涉及的步骤。 本教程使用MySQL数据库存储自适应表单数据。 只要已在AEM中部署了特定于数据库的驱动程序，就可以使用您选择的数据库来存储数据。 在高级别上，需要执行以下步骤来实现用例：

* 使用GuideBridge API获取对自适应表单数据的访问权限

* 对Servlet进行POST调用。 此Servlet将数据存储在数据库中，将表单附件存储在CRX存储库中。 数据库中存储的数据与GUID相关联。

* 当要使用存储的数据填充自适应表单时，需要检索与GUID关联的数据，并使用 **request.setAttribute** 方法。

## 用例演示

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=12&learn=on)

## 前提条件

此内容的受众应在以下方面具有一些经验：

* 自适应表单
* 表单数据模型
* OSGi服务/组件
* AEM客户端库


## 后续步骤

[配置数据源](./configure-data-source.md)