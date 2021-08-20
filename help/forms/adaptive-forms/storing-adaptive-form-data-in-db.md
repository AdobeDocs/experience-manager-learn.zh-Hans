---
title: 存储自适应表单数据
description: 将自适应表单数据存储到DataBase中，作为AEM工作流的一部分
feature: 自适应Forms，表单数据模型
version: 6.3,6.4,6.5
topic: 开发
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '380'
ht-degree: 1%

---


# 将自适应表单提交存储在数据库中

在您选择的数据库中存储已提交表单数据的方法有很多。 JDBC数据源可用于将数据直接存储到数据库中。 可以写入自定义OSGi包，将数据存储到数据库中。 本文使用AEM工作流中的自定义流程步骤来存储数据。
用例是在自适应表单提交时触发AEM工作流，工作流中的一个步骤将提交的数据存储到数据库中。

**请按照下面所述的步骤在您的系统上执行此操作**

* [下载Zip文件并将其内容解压缩到硬盘上](assets/storeafdataindb.zip)

   * 使用包管理器将StoreAFInDBWorkflow.zip导入AEM。 包具有将AF数据存储到数据库的示例工作流。 打开工作流模型。 该工作流只包含一个步骤。 此步骤将调用包中写入的代码，以将AF数据存储到数据库中。 我在给这个过程传递一个论点。 这是要保存其数据的自适应表单的名称。
   * 使用Felix Web控制台部署insertdata.core-0.0.1-SNAPSHOT.jar 。 此包具有将提交的表单数据写入数据库的代码

* 转到[ConfigMgr](http://localhost:4502/system/console/configMgr)

   * 搜索“JDBC连接池”。 创建新的Day Commons JDBC连接池。 指定特定于数据库的设置。

   * ![jdbc连接池](assets/jdbc-connection-pool.png)
   * 搜索“**将表单数据插入DB**”
   * 指定数据库的特定属性。
      * DataSourceName：您之前配置的数据源的名称。
      * TableName — 要在其中存储AF数据的表的名称
      * FormName — 用于保存表单名称的列名称
      * ColumnName — 用于保存AF数据的列名称

   ![插入数据](assets/insertdata.PNG)

* 创建自适应表单。

* 将自适应表单与AEM工作流(StoreAFValuesinDB)关联，如以下屏幕快照所示。

* 确保在数据文件路径中指定“data.xml”，如以下屏幕快照所示

   ![提交](assets/submissionafforms.png)

* 预览表单并提交

* 如果一切顺利，您应会看到表单数据存储在您指定的表和列中



