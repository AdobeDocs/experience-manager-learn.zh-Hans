---
title: 存储和检索MySQL数据库中的表单数据 — 配置数据源
description: 多部分教程将指导您完成存储和检索表单数据所涉及的步骤
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: dccca658-3373-4de2-8589-21ccba2b7ba6
duration: 49
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 1%

---

# 配置数据源

AEM可通过多种方式实现与外部数据库的集成。 数据库集成最常见和标准实践之一是通过使用Apache Sling连接池化数据源配置属性。 [configMgr](http://localhost:4502/system/console/configMgr).
第一步是下载并部署相应的 [MySql驱动程序](https://mvnrepository.com/artifact/mysql/mysql-connector-java) 在AEM中。
创建Apache Sling连接池化数据源，并提供以下屏幕快照中指定的属性。 数据库架构作为本教程资源的一部分提供给您。

![数据源](assets/save-continue.PNG)

数据库有一个名为formdata的表，该表包含3列，如下面的屏幕快照所示。

![数据库](assets/data-base-tables.PNG)

用于创建架构的sql文件可以是 [从此处下载](assets/form-data-db.sql). 您需要使用MySql Workbench导入此文件以创建架构和表。

>[!NOTE]
>请确保为数据源命名 **SaveAndContinue**. 示例代码使用名称连接到数据库。

| 属性名称 | 价值 |
| ------------------------|---------------------------------------|
| 数据源名称 | SaveAndContinue |
| JDBC驱动程序类 | com.mysql.cj.jdbc.Driver |
| JDBC连接uri | jdbc:mysql://localhost：3306/aemformstutorial |
