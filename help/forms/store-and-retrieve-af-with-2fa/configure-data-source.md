---
title: 配置数据源
description: 创建指向MySQL数据库的DataSource
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6541
thumbnail: 6541.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 3%

---


# 配置数据源

AEM支持与外部数据库集成的方式有很多。 数据库集成最常见的标准做法之一是通过configMgr使用Apache Sling Connection池化DataSource配置属 [性](http://localhost:4502/system/console/configMgr)。
第一步是下载合适的MySQL驱动程 [序并将其部](https://mvnrepository.com/artifact/mysql/mysql-connector-java) 署到AEM。
然后，设置特定于您的数据库的Sling Connection池化DataSource属性。 以下屏幕截图显示了本教程使用的设置。 数据库模式是作为本教程资源的一部分提供给您的。

![数据源](assets/data-source.JPG)


* JDBC驱动程序类： `com.mysql.cj.jdbc.Driver`
* JDBC连接URI: `jdbc:mysql://localhost:3306/aemformstutorial`

>[!NOTE]
>请确保将数据源命 `StoreAndRetrieveAfData` 名为OSGi服务中使用的名称。


## 创建数据库


以下数据库用于此用例。 数据库有一个名为 `formdatawithattachments` 的表，其中有4列，如下面的屏幕截图所示。
![数据库](assets/table-schema.JPG)

* 列afdata **将保** 存自适应表单数据。
* 列 **attachmentsInfo** 将保存有关表单附件的信息。
* 列 **telphoneNumber** 将包含填写表单的人员的移动号码。

请通过使用MySQL工作台导入数 [据库模式](assets/data-base-schema.sql)来创建数据库。

## 创建表单数据模型

创建表单数据模型，并将其基于在上一步中创建的数据源。
如下 **屏幕截图** ，配置此表单数据模型的get服务。
确保未在get服务中返回 **阵列** 。

此 **获取** 服务用于获取与应用程序ID关联的电话号码。

![get-service](assets/get-service.JPG)

此表单数据模型随后将用于MyAccountForm **中** ，以获取与应用程序ID关联的电话号码。
