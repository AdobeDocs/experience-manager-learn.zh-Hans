---
title: 配置数据源
description: 创建指向MySQL数据库的数据源
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6541
thumbnail: 6541.jpg
topic: Development
role: Developer
level: Beginner
exl-id: a87ff428-15f7-43c9-ad03-707eab6216a9
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '320'
ht-degree: 4%

---

# 配置数据源

AEM可通过多种方式实现与外部数据库的集成。 数据库集成最常见和标准实践之一是通过使用Apache Sling连接池化数据源配置属性 [configMgr](http://localhost:4502/system/console/configMgr).
第一步是下载并部署相应的 [MySQL驱动程序](https://mvnrepository.com/artifact/mysql/mysql-connector-java) 到AEM。
然后，设置特定于数据库的Sling连接池化数据源属性。 以下屏幕截图显示了用于本教程的设置。 数据库架构作为本教程资源的一部分提供给您。

>[!NOTE]
>请确保为数据源命名 `StoreAndRetrieveAfData` 因为这是OSGi服务中使用的名称。


![数据源](assets/data-source.JPG)

| 属性名称 | 属性值 |   |
|---------------------|------------------------------------------------------------------------------------|---|
| 数据源名称 | StoreAndRetrieveAfData |   |
| JDBC驱动器类 | jdbc:mysql://localhost：3306/aemformstutorial |   |
| JDBC连接URI | jdbc:mysql://localhost：3306/aemformstutorial？serverTimezone=UTC&amp;autoReconnect=true |   |
|                     |                                                                                    |   |


## 创建数据库


以下数据库用于此用例。 数据库有一个名为 `formdatawithattachments` ，如下面的屏幕快照中所示的4列。
![数据库](assets/table-schema.JPG)

* 列 **afdata** 将保存自适应表单数据。
* 列 **附件信息** 将保存有关表单附件的信息。
* 列 **telephonenumber** 将保存填写表单的人员的手机号码。

请通过导入 [数据库模式](assets/data-base-schema.sql)
使用MySQL工作台。

## 创建表单数据模型

创建表单数据模型，并将其基于上一步中创建的数据源。
配置 **get** 此表单数据模型的服务，如下面的屏幕快照所示。
确保您不会在 **get** 服务。

此功能的目的 **get** 服务是获取与应用程序id关联的电话号码。

![get-service](assets/get-service.JPG)

然后，此表单数据模型将用于 **我的帐户表单** 以获取与应用程序id关联的电话号码。

## 后续步骤

[编写代码以保存表单附件](./store-form-attachments.md)
