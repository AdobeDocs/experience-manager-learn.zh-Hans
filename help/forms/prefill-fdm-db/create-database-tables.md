---
title: 创建数据库表
description: 创建表单数据模型要使用的数据库
feature: 自适应表单
version: 6.4,6.5
kt: 5811
thumbnail: kt-5811.jpg
topic: 开发
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '102'
ht-degree: 2%

---


# 创建数据库表

表单数据模型可以基于RDBMS、RESTfull、SOAP或OData源。 本课程的重点是使用由RDBMS数据源支持的表单数据模型预归档自适应表单。 本教程中使用了MYSQL数据库。 我们创建了以下两个表来演示用例

* **** newhiretable — 此表存储新信息

   ![newhire](assets/newhire-table.png)


* **** 受益人 — 这家店的受益人是

   ![受益人](assets/beneficiaries-table.png)

您可以使用MySQL Workbench将[sql文件](assets/db-schema.sql)导入到包含某些示例数据的表中。