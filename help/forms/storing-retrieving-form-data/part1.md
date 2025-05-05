---
title: 存储和检索MySQL数据库中的表单数据 — 配置数据Source
description: 多部分教程将指导您完成存储和检索表单数据所涉及的步骤
version: Experience Manager 6.4, Experience Manager 6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: dccca658-3373-4de2-8589-21ccba2b7ba6
duration: 36
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '187'
ht-degree: 1%

---

# 配置数据Source

AEM可通过多种方式与外部数据库集成。 数据库集成最常见和标准的做法之一是通过[configMgr](http://localhost:4502/system/console/configMgr)使用Apache Sling连接池化DataSource配置属性。
第一步是在AEM中下载并部署相应的[MySql驱动程序](https://mvnrepository.com/artifact/mysql/mysql-connector-java)。
创建Apache Sling连接池化数据源，并提供以下屏幕快照中指定的属性。 数据库架构作为本教程资源的一部分提供给您。

![数据源](assets/save-continue.PNG)

数据库有一个名为formdata的表，该表包含3列，如下面的屏幕快照所示。

![数据库](assets/data-base-tables.PNG)

可从此处[&#128279;](assets/form-data-db.sql)下载用于创建架构的SQL文件。 您需要使用MySql Workbench导入此文件以创建架构和表。

>[!NOTE]
>请确保将数据源命名为&#x200B;**SaveAndContinue**。 示例代码使用名称连接到数据库。

| 属性名称 | 价值 |
| ------------------------|---------------------------------------|
| 数据源名称 | `SaveAndContinue` |
| JDBC驱动程序类 | `com.mysql.cj.jdbc.Driver` |
| JDBC连接uri | `jdbc:mysql://localhost:3306/aemformstutorial` |
