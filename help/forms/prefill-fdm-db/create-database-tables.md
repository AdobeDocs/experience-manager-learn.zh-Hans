---
title: 创建数据库表
description: 创建表单数据模型要使用的数据库
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5811
thumbnail: kt-5811.jpg
translation-type: tm+mt
source-git-commit: b085a2c75f8e0b4860d503774ea01a108773ad09
workflow-type: tm+mt
source-wordcount: '99'
ht-degree: 0%

---


# 创建数据库表

表单数据模型可以基于RDBMS、RESTfull、SOAP或OData源。 本课程的重点是使用RDBMS数据源支持的表单数据模型预先填写自适应表单。 本教程使用了MYSQL数据库。 我们创建了以下两个表来演示用例

* **newhire** table —— 此表存储新信息

   ![newhire](assets/newhire-table.png)


* **受益人** 表——这家商店的受益人

   ![受益人](assets/beneficiaries-table.png)

您可以使用 [MySQL工作台](assets/db-schema.sql) ，将sql文件导入到包含一些示例数据的表中。