---
title: 从MySQL数据库存储和检索表单数据
description: 多部分教程，指导您完成存储和检索表单数据所涉及的步骤
feature: 自适应表单
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: 开发
role: 开发人员
level: 富有经验
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 2%

---


# 从MySQL数据库存储和检索自适应表单数据

本教程将指导您完成从数据库保存和检索自适应表单数据所涉及的步骤。 本教程使用MySQL数据库存储自适应表单数据。 只要已在AEM中部署特定于数据库的驱动程序，您选择的数据库就可以用于存储数据。 在高级别上，需要采取以下步骤来实现用例：

* 使用GuideBridge API获取对自适应表单数据的访问

* 对servlet进行POST调用。 此servlet将数据存储在数据库中。 存储的数据与GUID关联

* 如果要用存储的数据填充自适应表单，请检索与GUID关联的数据，并使用&#x200B;**request.setAttribute**&#x200B;方法填充自适应表单。

## 用例演示

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=9&learn=on)
