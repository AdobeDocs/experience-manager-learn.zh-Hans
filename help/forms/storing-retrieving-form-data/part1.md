---
title: 从MySQL数据库存储和检索表单数据
description: 多部分教程，指导您完成存储和检索表单数据时涉及的步骤
version: 6.3,6.4,6.5
feature: 自适应表单
topic: 开发
role: Developer
level: Experienced
source-git-commit: 3569d8b2a38d1cce02f6f4ff8b0c583f4dc118b6
workflow-type: tm+mt
source-wordcount: '199'
ht-degree: 5%

---

# 配置数据源

AEM可通过多种方式与外部数据库集成。 数据库集成最常见的标准做法之一是通过[configMgr](http://localhost:4502/system/console/configMgr)使用Apache Sling连接池化数据源配置属性。
第一步是在AEM中下载并部署相应的[MySql驱动程序](https://mvnrepository.com/artifact/mysql/mysql-connector-java)。
创建Apache Sling连接池化数据源，并提供在以下屏幕快照中指定的属性。 本教程资产中将向您提供数据库模式。

![数据源](assets/save-continue.PNG)

数据库有一个名为formdata的表，其中包含3列，如下面的屏幕截图所示。

![数据库](assets/data-base-tables.PNG)

创建架构的sql文件可从此处](assets/form-data-db.sql)下载。 [您需要使用MySql Workbench导入此文件，以创建架构和表。

>[!NOTE]
>请确保将数据源命名为&#x200B;**SaveAndContinue**。 示例代码使用名称连接到数据库。

| 属性名称 | 值 |
| ------------------------|---------------------------------------|
| 数据源名称 | SaveAndContinue |
| JDBC驱动程序类 | com.mysql.cj.jdbc.Driver |
| JDBC连接URI | jdbc:mysql://localhost:3306/aemformstorial |
