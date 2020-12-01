---
title: AEM Forms，含JSON模式和数据[第1部分]
seo-title: AEM Forms，含JSON模式和数据[Part1]
description: 多部分教程，指导您逐步完成使用JSON模式创建自适应表单和查询提交数据所涉及的步骤。
seo-description: 多部分教程，指导您逐步完成使用JSON模式创建自适应表单和查询提交数据所涉及的步骤。
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 0%

---


# 基于JSON模式创建自适应表单


AEM Forms6.3版引入了基于JSON模式创建自适应Forms的功能。 有关使用JSON模式创建自适应Forms的详细信息，请参阅本[文章](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-form-json-schema-form-model.html)。

根据JSON模式创建自适应表单后，下一步是将提交的数据存储在数据库中。 为此，我们将使用由不同数据库供应商引入的新JSON数据类型。 为了本文的目的，我们将使用MySql 8数据库存储提交的数据。

本文使用了MySql 8数据库。 MySQL引入了一种名为[JSON](https://dev.mysql.com/doc/refman/8.0/en/json.html)的新数据类型。 这使得存储和查询JSON对象变得更简单。 我们将将提交的数据存储在数据库的JSON类型列中。

以下屏幕截图显示以JSON数据类型存储的已提交表单数据。 列“formdata”的类型为JSON。 我们还将与数据关联的表单的名称存储在列表单名称中

>[!NOTE]
>
>请确保您的json模式文件命名正确。 例如，它需要以下格式命名：&lt;name>模式.json。 因此，您的模式文件可以是mortgage.模式.json或credit.模式.json。


![数据存储](assets/datastored.gif)


[可用于创建自适应Forms的示例JSON模式。](assets/samplejsonschemas.zip). 下载并解压缩zip文件以获取JSON模式

