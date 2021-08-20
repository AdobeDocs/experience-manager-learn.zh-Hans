---
title: 从MySQL数据库存储和检索表单数据
description: 多部分教程，指导您完成存储和检索表单数据时涉及的步骤
feature: 自适应表单
type: Tutorial
version: 6.3,6.4,6.5
topic: 开发
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 1%

---


# 从MySQL数据库存储和检索自适应表单数据

本教程将指导您完成从数据库中保存和检索自适应表单数据所涉及的步骤。 本教程使用MySQL数据库存储自适应表单数据。 只要已在AEM中部署了特定于数据库的驱动程序，就可以使用您选择的数据库来存储数据。 在高级别上，需要执行以下步骤来实现用例：

* 使用GuideBridge API获取对自适应表单数据的访问权限

* 对Servlet进行POST调用。 此Servlet将数据存储在数据库中。 存储的数据与GUID关联

* 如果要使用存储的数据填充自适应表单，请检索与GUID关联的数据，然后使用&#x200B;**request.setAttribute**&#x200B;方法填充自适应表单。

## 用例演示

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=9&learn=on)
