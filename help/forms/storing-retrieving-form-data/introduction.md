---
title: 从MySQL数据库存储和检索表单数据简介
description: 多部分教程，指导您完成存储和检索表单数据时涉及的步骤
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 95795102-4278-4556-8e0f-1b8a359ab093
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 0%

---

# 从MySQL数据库存储和检索自适应表单数据

本教程将指导您完成从数据库中保存和检索自适应表单数据所涉及的步骤。 本教程使用MySQL数据库存储自适应表单数据。 只要已在AEM中部署了特定于数据库的驱动程序，就可以使用您选择的数据库来存储数据。 在高级别上，需要执行以下步骤来实现用例：

* 使用GuideBridge API获取对自适应表单数据的访问权限

* 对Servlet进行POST调用。 此Servlet将数据存储在数据库中。 存储的数据与GUID关联

* 当要使用存储的数据填充自适应表单时，需要检索与GUID关联的数据，并使用 **request.setAttribute** 方法。

## 用例演示

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=12&learn=on)


