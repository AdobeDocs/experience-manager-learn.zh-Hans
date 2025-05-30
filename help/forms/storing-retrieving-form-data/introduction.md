---
title: 存储和检索MySQL数据库中的表单数据简介
description: 多部分教程将指导您完成存储和检索表单数据所涉及的步骤
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 95795102-4278-4556-8e0f-1b8a359ab093
last-substantial-update: 2019-07-07T00:00:00Z
duration: 236
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 0%

---

# 存储和检索MySQL数据库中的自适应表单数据

本教程将指导您完成从数据库保存和检索自适应表单数据所涉及的步骤。 本教程使用MySQL数据库来存储自适应表单数据。 只要您在AEM中部署了特定于数据库的驱动程序，就可以使用您选择的数据库来存储数据。 从较高层面来看，要实现此用例，需要执行以下步骤：

* 使用GuideBridge API获取对自适应表单数据的访问权限

* 对servlet进行POST调用。 此servlet将数据存储在数据库中。 存储的数据与GUID相关联

* 如果要使用存储的数据填充自适应表单，请检索与GUID关联的数据，并使用&#x200B;**request.setAttribute**&#x200B;方法填充自适应表单。

## 用例演示

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=12&learn=on)


