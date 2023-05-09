---
title: 配置AEM数据源
description: 配置MySQL备份数据源以存储和检索表单数据
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
version: 6.4,6.5
kt: 6899
thumbnail: 6899.jpg
exl-id: 2e851ae5-6caa-42e3-8af2-090766a6f36a
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 5%

---

# 配置数据源

AEM可通过多种方式与外部数据库集成。 集成数据库的最常见方式之一是，通过 [configMgr](http://localhost:4502/system/console/configMgr).
第一步是下载并部署相应的 [MySql驱动程序](https://mvnrepository.com/artifact/mysql/mysql-connector-java) 在AEM中。
创建Apache Sling连接池化数据源，并提供在以下屏幕快照中指定的属性。 本教程资产中将向您提供数据库模式。

![数据源](assets/data-source.PNG)

数据库有一个名为formdata的表，其中包含3列，如下面的屏幕截图所示。

![数据库](assets/data-base.PNG)


>[!NOTE]
>请确保为数据源命名 **aemformation**. 示例代码使用名称连接到数据库。

| 属性名称 | 价值 |
| ------------------------|--------------------------------------- |
| 数据源名称 | SaveAndContinue |
| JDBC驱动程序类 | com.mysql.cj.jdbc.Driver |
| JDBC连接URI | jdbc:mysql://localhost:3306/aemformstutorial |

## Assets

用于创建架构的SQL文件可以 [从此处下载](assets/sign-multiple-forms.sql). 您需要使用MySql Workbench导入此文件，以创建架构和表。

## 后续步骤

[创建OSGi服务以在数据库中存储和获取数据](./create-osgi-service.md)
