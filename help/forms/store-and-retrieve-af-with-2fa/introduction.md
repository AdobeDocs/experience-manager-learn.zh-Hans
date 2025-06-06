---
title: 从MySQL数据库存储和检索带有附件的表单数据
description: 多部分教程将指导您完成存储和检索带有附件的表单数据所涉及的步骤
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6593
thumbnail: 327122.jpg
topic: Development
role: Developer
level: Experienced
exl-id: b278652f-6c09-4abc-b92e-20bfaf2e791a
last-substantial-update: 2020-11-07T00:00:00Z
duration: 148
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '212'
ht-degree: 2%

---

# 使用2FA存储和检索自适应表单数据

本教程将指导您完成使用2FA保存和检索带有附件的自适应表单数据所涉及的步骤。 本教程使用MySQL数据库来存储自适应表单数据。 只要您在AEM中部署了特定于数据库的驱动程序，就可以使用您选择的数据库来存储数据。 从较高层面来看，要实现此用例，需要执行以下步骤：

* 使用GuideBridge API获取对自适应表单数据的访问权限

* 对servlet进行POST调用。 此servlet将数据存储在数据库中，并将表单附件存储在CRX存储库中。 数据库中存储的数据与GUID相关联。

* 如果要使用存储的数据填充自适应表单，请检索与GUID关联的数据，并使用&#x200B;**request.setAttribute**&#x200B;方法填充自适应表单。

## 用例演示

>[!VIDEO](https://video.tv.adobe.com/v/346927?quality=12&learn=on&captions=chi_hans)

## 先决条件

预计本内容的受众将在以下领域有一些经验：

* 自适应表单
* 表单数据模型
* OSGi服务/组件
* AEM客户端库


## 后续步骤

[配置数据Source](./configure-data-source.md)