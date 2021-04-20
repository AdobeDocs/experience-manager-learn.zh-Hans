---
title: 从MySQL数据库存储和检索带有附件的表单数据
description: 多部分教程，指导您逐步了解使用附件存储和检索表单数据所涉及的步骤
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6593
thumbnail: 327122.jpg
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '212'
ht-degree: 4%

---


# 2FA自适应表单数据的存储与检索

本教程将指导您完成使用2FA保存和检索带有附件的自适应表单数据所涉及的步骤。 本教程使用MySQL数据库存储自适应表单数据。 只要已在AEM中部署特定于数据库的驱动程序，您选择的数据库就可以用于存储数据。 在高级别上，需要采取以下步骤来实现用例：

* 使用GuideBridge API获取对自适应表单数据的访问

* 对servlet进行POST调用。 此servlet将数据存储在数据库中，并将表单附件存储在CRX存储库中。 数据库中存储的数据与GUID关联。

* 如果要用存储的数据填充自适应表单，请检索与GUID关联的数据，并使用&#x200B;**request.setAttribute**&#x200B;方法填充自适应表单。

## 用例演示

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=9&learn=on)

## 前提条件

预计受众此内容将在以下方面获得一些经验：

* 自适应表单
* 表单数据模型
* OSGi服务/组件
* AEM客户端库
