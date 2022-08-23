---
title: 从MySQL数据库存储和检索表单数据 — 配置数据源
description: 多部分教程，指导您完成存储和检索表单数据时涉及的步骤
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: dccca658-3373-4de2-8589-21ccba2b7ba6
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '199'
ht-degree: 4%

---

# 配置数据源

AEM可通过多种方式与外部数据库集成。 数据库集成最常见的标准做法之一是，通过 [configMgr](http://localhost:4502/system/console/configMgr).
第一步是下载并部署相应的 [MySql驱动程序](https://mvnrepository.com/artifact/mysql/mysql-connector-java) 在AEM中。
创建Apache Sling连接池化数据源，并提供在以下屏幕快照中指定的属性。 本教程资产中将向您提供数据库模式。

![数据源](assets/save-continue.PNG)

数据库有一个名为formdata的表，其中包含3列，如下面的屏幕截图所示。

![数据库](assets/data-base-tables.PNG)

用于创建架构的sql文件可以是 [从此处下载](assets/form-data-db.sql). 您需要使用MySql Workbench导入此文件，以创建架构和表。

>[!NOTE]
>请确保为数据源命名 **SaveAndContinue**. 示例代码使用名称连接到数据库。

| 属性名称 | 值 |
| ------------------------|---------------------------------------|
| 数据源名称 | SaveAndContinue |
| JDBC驱动程序类 | com.mysql.cj.jdbc.Driver |
| JDBC连接URI | jdbc:mysql://localhost:3306/aemformstutorial |
