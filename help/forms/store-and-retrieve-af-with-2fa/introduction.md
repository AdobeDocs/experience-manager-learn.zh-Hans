---
title: 从MySQL数据库存储和检索带有附件的表单数据
description: 多部分教程将指导您完成存储和检索带有附件的表单数据所涉及的步骤
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6593
thumbnail: 327122.jpg
topic: Development
role: Developer
level: Experienced
exl-id: b278652f-6c09-4abc-b92e-20bfaf2e791a
last-substantial-update: 2020-11-07T00:00:00Z
duration: 159
source-git-commit: 0400242f6a99bc5209a8b483469d5fd88eac077e
workflow-type: tm+mt
source-wordcount: '212'
ht-degree: 2%

---

# 使用2FA存储和检索自适应表单数据

本教程将指导您完成使用2FA保存和检索带有附件的自适应表单数据所涉及的步骤。 本教程使用MySQL数据库来存储自适应表单数据。 只要在AEM中部署了数据库特定的驱动程序，就可以使用您选择的数据库来存储数据。 从较高层面来看，要实现此用例，需要执行以下步骤：

* 使用GuideBridge API获取对自适应表单数据的访问权限

* 对servlet进行POST调用。 此servlet将数据存储在数据库中，并将表单附件存储在CRX存储库中。 数据库中存储的数据与GUID相关联。

* 如果要使用存储的数据填充自适应表单，请检索与GUID关联的数据，然后使用 **request.setAttribute** 方法。

## 用例演示

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=12&learn=on)

## 先决条件

预计本内容的受众将在以下领域有一些经验：

* 自适应表单
* 表单数据模型
* OSGi服务/组件
* AEM客户端库


## 后续步骤

[配置数据源](./configure-data-source.md)