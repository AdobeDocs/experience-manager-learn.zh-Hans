---
title: 创建数据库表
description: 创建表单数据模型要使用的数据库
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-5811
thumbnail: kt-5811.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 1136244a-c3e6-45f6-8af8-eb3c100f838e
duration: 21
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '105'
ht-degree: 1%

---

# 创建数据库表

表单数据模型可以基于RDBMS、RESTfull、SOAP或OData源。 本课程的重点是使用RDBMS数据源支持的表单数据模型预归档自适应表单。 在本教程中，使用了MYSQL数据库。 我们创建了以下两个表来演示用例

* **newhire**&#x200B;表 — 此表存储newhire信息

  ![newhire](assets/newhire-table.png)


* **受益人**&#x200B;表 — 此存储新受益人

  ![受益人](assets/beneficiaries-table.png)

您可以使用MySQL Workbench将[sql文件](assets/db-schema.sql)导入到包含一些示例数据的表。

## 后续步骤

[配置表单数据模型](./configuring-form-data-model.md)
