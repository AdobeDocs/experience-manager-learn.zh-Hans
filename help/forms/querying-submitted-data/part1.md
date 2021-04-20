---
title: AEM Forms，含JSON模式和数据[第1部分]
seo-title: AEM Forms，含JSON模式和数据[Part1]
description: 多部分教程，用于指导您完成创建带有JSON模式的自适应表单和查询提交数据所涉及的步骤。
seo-description: 多部分教程，用于指导您完成创建带有JSON模式的自适应表单和查询提交数据所涉及的步骤。
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '299'
ht-degree: 1%

---


# 基于JSON模式创建自适应表单


AEM Forms 6.3版本引入了基于JSON模式创建自适应Forms的功能。 有关使用JSON模式创建自适应Forms的详细信息，请参阅此[文章](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-form-json-schema-form-model.html)。

根据JSON模式创建自适应表单后，下一步是将提交的数据存储在数据库中。 为此，我们将使用各种数据库供应商引入的新JSON数据类型。 为了本文的目的，我们将使用MySql 8数据库存储提交的数据。

本文使用了MySql 8数据库。 MySQL引入了名为[JSON](https://dev.mysql.com/doc/refman/8.0/en/json.html)的新数据类型。 这使得存储和查询JSON对象变得更简单。 我们会将提交的数据存储在数据库中JSON类型的列中。

以下屏幕截图显示已提交的以JSON数据类型存储的表单数据。 列“formdata”的类型为JSON。 我们还将与数据关联的表单的名称存储在列表单名称中

>[!NOTE]
>
>请确保您的json模式文件命名正确。 例如，它需要使用以下格式&lt;name>模式.json命名。 因此，您的模式文件可以是mortgage.模式.json或credit.模式.json。


![数据存储](assets/datastored.gif)


[可用于创建Adaptive Forms的示例JSON模式。](assets/samplejsonschemas.zip). 下载并解压缩zip文件以获取JSON模式

