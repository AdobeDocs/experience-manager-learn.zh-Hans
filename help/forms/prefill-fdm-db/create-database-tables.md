---
title: 创建数据库表
description: 创建表单数据模型要使用的数据库
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-5811
thumbnail: kt-5811.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 1136244a-c3e6-45f6-8af8-eb3c100f838e
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '105'
ht-degree: 1%

---

# 创建数据库表

表单数据模型可以基于RDBMS、RESTfull、SOAP或OData源。 本课程的重点是使用RDBMS数据源支持的表单数据模型预归档自适应表单。 在本教程中，使用了MYSQL数据库。 我们创建了以下两个表来演示用例

* **纽惠尔** 表 — 此表存储新信息

  ![纽惠尔](assets/newhire-table.png)


* **受益人** 表 — 此存储新受益人

  ![受益人](assets/beneficiaries-table.png)

您可以导入 [sql文件](assets/db-schema.sql) 使用MySQL Workbench创建包含一些示例数据的表。

## 后续步骤

[配置表单数据模型](./configuring-form-data-model.md)
