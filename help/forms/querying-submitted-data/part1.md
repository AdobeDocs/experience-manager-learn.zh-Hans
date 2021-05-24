---
title: 具有JSON模式和数据的AEM Forms[第1部分]
seo-title: 具有JSON模式和数据的AEM Forms[Part1]
description: 多部分教程，用于指导您完成使用JSON模式创建自适应表单以及查询提交数据时涉及的步骤。
seo-description: 多部分教程，用于指导您完成使用JSON模式创建自适应表单以及查询提交数据时涉及的步骤。
feature: 自适应表单
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: 开发
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '297'
ht-degree: 1%

---


# 基于JSON模式创建自适应表单


AEM Forms 6.3版本中引入了基于JSON模式创建自适应Forms的功能。 本文[文章](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-form-json-schema-form-model.html)中详细说明了有关使用JSON模式创建自适应Forms的详细信息。

基于JSON模式创建自适应表单后，下一步是将提交的数据存储在数据库中。 为此，我们将使用各种数据库供应商引入的新JSON数据类型。 为了本文的目的，我们将使用MySql 8数据库来存储提交的数据。

本文使用了MySql 8数据库。 MySQL引入了名为[JSON](https://dev.mysql.com/doc/refman/8.0/en/json.html)的新数据类型。 这可以更轻松地存储和查询JSON对象。 我们将将提交的数据存储在数据库中JSON类型的列中。

以下屏幕截图显示了已提交的表单数据，这些数据以JSON数据类型存储。 列“formdata”的类型为JSON。 我们还将与数据关联的表单的名称存储在列表单名称中

>[!NOTE]
>
>请确保您的json架构文件的名称正确。 例如，需要使用以下格式&lt;name>schema.json来命名它。 因此，您的架构文件可以是mortgage.schema.json或credit.schema.json。


![数据存储](assets/datastored.gif)


[可用于创建自适应Forms的JSON架构示例。](assets/samplejsonschemas.zip). 下载并解压缩zip文件，以获取JSON架构

