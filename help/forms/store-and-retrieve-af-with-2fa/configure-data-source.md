---
title: 配置数据源
description: 创建指向MySQL数据库的数据源
feature: 自适应表单
type: Tutorial
version: 6.4,6.5
kt: 6541
thumbnail: 6541.jpg
topic: 开发
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 4%

---


# 配置数据源

AEM可通过多种方式与外部数据库集成。 数据库集成最常见的标准做法之一是通过[configMgr](http://localhost:4502/system/console/configMgr)使用Apache Sling连接池化数据源配置属性。
第一步是下载相应的[MySQL驱动程序](https://mvnrepository.com/artifact/mysql/mysql-connector-java)并将其部署到AEM。
然后，设置特定于您的数据库的Sling连接池化数据源属性。 以下屏幕截图显示了用于本教程的设置。 本教程资产中将向您提供数据库模式。

![数据源](assets/data-source.JPG)


* JDBC驱动程序类：`com.mysql.cj.jdbc.Driver`
* JDBC连接URI:`jdbc:mysql://localhost:3306/aemformstutorial`

>[!NOTE]
>请确保将数据源`StoreAndRetrieveAfData`命名为，因为这是OSGi服务中使用的名称。


## 创建数据库


以下数据库用于此用例。 数据库有一个名为`formdatawithattachments`的表，其中有4列，如下面的屏幕截图所示。
![数据库](assets/table-schema.JPG)

* 列&#x200B;**afdata**&#x200B;将保存自适应表单数据。
* 列&#x200B;**attachmentsInfo**&#x200B;将保存有关表单附件的信息。
* 列&#x200B;**telephoneNumber**&#x200B;将包含填写表单的人员的手机号码。

请通过导入[数据库架构](assets/data-base-schema.sql)来创建数据库
使用MySQL Workbench。

## 创建表单数据模型

创建表单数据模型，并将其基于上一步中创建的数据源。
配置此表单数据模型的**get**服务，如以下屏幕快照所示。
确保未在**get**&#x200B;服务中返回数组。

此&#x200B;**get**&#x200B;服务用于获取与应用程序ID关联的电话号码。

![get-service](assets/get-service.JPG)

然后，此表单数据模型将用在&#x200B;**MyAccountForm**&#x200B;中，以获取与应用程序ID关联的电话号码。
