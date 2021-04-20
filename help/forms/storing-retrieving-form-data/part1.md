---
title: 从MySQL数据库存储和检索表单数据
description: 多部分教程，指导您完成存储和检索表单数据所涉及的步骤
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '202'
ht-degree: 5%

---

# 配置数据源

AEM支持与外部数据库集成的方式有很多。 数据库集成最常见的标准做法之一是通过[configMgr](http://localhost:4502/system/console/configMgr)使用Apache Sling Connection池化DataSource配置属性。
第一步是下载并部署AEM中相应的[MySql驱动程序](https://mvnrepository.com/artifact/mysql/mysql-connector-java)。
创建Apache Sling Connection Pooled DataSource并提供以下屏幕快照中指定的属性。 数据库模式作为本教程资源的一部分提供给您。

![数据源](assets/save-continue.PNG)

数据库有一个名为formdata的表，其中有3列，如下面的屏幕截图所示。

![数据库](assets/data-base-tables.PNG)

创建模式的sql文件可从此处](assets/form-data-db.sql)下载。 [您需要使用MySql Workbench导入此文件以创建模式和表。

>[!NOTE]
>请确保将数据源命名为&#x200B;**SaveAndContinue**。 示例代码使用名称连接到数据库。

| 属性名称 | 值 |
------------------------|---------------------------------------
| 数据源名称 | 保存并继续 |
| JDBC驱动程序类 | com.mysql.cj.jdbc.Driver |
| JDBC连接URI | jdbc:mysql://localhost:3306/aemformstutorial |


