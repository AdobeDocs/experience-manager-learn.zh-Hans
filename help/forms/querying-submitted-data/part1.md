---
title: 具有JSON架构和数据的AEM Forms[第1部分]
description: 多部分教程将指导您完成使用JSON架构创建自适应表单和查询提交的数据所涉及的步骤。
feature: Adaptive Forms
doc-type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: c588bdca-b8a8-4de2-97e0-ba08b195699f
duration: 62
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 0%

---

# 创建基于JSON架构的自适应表单


AEM Forms 6.3版本中引入了创建基于JSON架构的自适应Forms的功能。 有关使用JSON架构创建自适应Forms的详细信息，请参阅本文 [文章](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/adaptive-form-json-schema-form-model.html).

创建基于JSON模式的自适应表单后，下一步是将提交的数据存储在数据库中。 为此，我们将使用由各种数据库供应商引入的新JSON数据类型。 为便于本文使用，我们将使用MySql 8数据库来存储提交的数据。

本文使用了MySql 8数据库。 MySQL引入了一种新的数据类型，称为 [JSON](https://dev.mysql.com/doc/refman/8.0/en/json.html). 这样可以更轻松地存储和查询JSON对象。 我们将提交的数据存储在数据库的JSON类型列中。

以下屏幕抓图显示了以JSON数据类型存储的提交的表单数据。 列“formdata”的类型为JSON。 我们还将与数据关联的表单名称存储在列表单名称中

>[!NOTE]
>
>请确保已正确命名您的json架构文件。 例如，它需要以下列格式命名 &lt;name>schema.json。 因此，您的架构文件可以是mortgage.schema.json或credit.schema.json。


![数据存储](assets/datastored.gif)


[可用于创建自适应Forms的JSON架构示例。](assets/samplejsonschemas.zip). 下载并解压缩zip文件以获取JSON架构
