---
title: 配置数据源
description: 创建指向MySQL数据库的数据源
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6541
thumbnail: 6541.jpg
topic: Development
role: Developer
level: Beginner
exl-id: a87ff428-15f7-43c9-ad03-707eab6216a9
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '303'
ht-degree: 3%

---

# 配置数据源

AEM可通过多种方式与外部数据库集成。 数据库集成的最常见和标准做法之一，是通过 [configMgr](http://localhost:4502/system/console/configMgr).
第一步是下载并部署相应的 [MySQL驱动程序](https://mvnrepository.com/artifact/mysql/mysql-connector-java) 到AEM。
然后，设置特定于您的数据库的Sling连接池化数据源属性。 以下屏幕截图显示了用于本教程的设置。 本教程资产中将向您提供数据库模式。

![数据源](assets/data-source.JPG)


* JDBC驱动程序类： `com.mysql.cj.jdbc.Driver`
* JDBC连接URI: `jdbc:mysql://localhost:3306/aemformstutorial`

>[!NOTE]
>请确保为数据源命名 `StoreAndRetrieveAfData` 因为这是OSGi服务中使用的名称。


## 创建数据库


以下数据库用于此用例。 数据库有一个名为 `formdatawithattachments` 中包含4列，如下面的屏幕截图所示。
![数据库](assets/table-schema.JPG)

* 列 **afdata** 将保存自适应表单数据。
* 列 **attachmentsInfo** 将保存有关表单附件的信息。
* 列 **电话号码** 将保存填写表单的人员的手机号码。

请通过导入 [数据库模式](assets/data-base-schema.sql)
使用MySQL Workbench。

## 创建表单数据模型

创建表单数据模型，并将其基于上一步中创建的数据源。
配置 **get** 此表单数据模型的服务，如以下屏幕截图所示。
确保未在 **get** 服务。

这个目的 **get** 服务是获取与应用程序id关联的电话号码。

![get-service](assets/get-service.JPG)

此表单数据模型随后将用在 **MyAccountForm** 以获取与应用程序id关联的电话号码。

## 后续步骤

[编写代码以保存表单附件](./store-form-attachments.md)
