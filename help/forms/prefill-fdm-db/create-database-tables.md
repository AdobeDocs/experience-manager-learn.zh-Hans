---
title: 创建数据库表
description: 创建表单数据模型要使用的数据库
feature: 自适应表单
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5811
thumbnail: kt-5811.jpg
topic: 开发
role: 开发人员
level: 初学者
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '104'
ht-degree: 3%

---


# 创建数据库表

表单数据模型可以基于RDBMS、RESTfull、SOAP或OData源。 本课程的重点是使用RDBMS数据源支持的表单数据模型预先填写自适应表单。 本教程使用了MYSQL数据库。 我们创建了以下两个表来演示用例

* **** newhiretable — 此表存储新信息

   ![newhire](assets/newhire-table.png)


* **受** 益者 — 这家店的受益者

   ![受益人](assets/beneficiaries-table.png)

您可以使用MySQL Workbench将[sql文件](assets/db-schema.sql)导入到包含某些示例数据的表。