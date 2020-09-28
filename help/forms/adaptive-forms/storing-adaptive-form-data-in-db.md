---
title: 存储自适应表单数据
seo-title: 存储自适应表单数据
description: 将自适应表单数据存储到DataBase中，作为AEM工作流程的一部分
seo-description: 将自适应表单数据存储到DataBase中，作为AEM工作流程的一部分
feature: adaptive-forms,workflow
topics: integrations
audience: implementer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '390'
ht-degree: 1%

---


# 将自适应表单提交存储在数据库中

在您选择的数据库中存储已提交表单数据的方法很多。 JDBC数据源可用于将数据直接存储到数据库中。 可以编写自定义OSGI捆绑包，将数据存储到数据库中。 本文使用AEM工作流中的自定义流程步骤来存储数据。
用例是在提交自适应表单时触发AEM工作流，工作流中的步骤将提交的数据存储到数据库中。

**请按照以下步骤操作，使系统能够正常工作**

* [下载Zip文件并将其内容解压缩到硬盘上](assets/storeafdataindb.zip)

   * 使用包管理器将StoreAFInDBWorkflow.zip导入AEM。 包具有将AF数据存储到数据库的示例工作流。 打开工作流模型。 该工作流只有一个步骤。 此步骤调用打包中写入的代码，将AF数据存储到数据库中。 我在给这个过程传递一个论点。 这是要保存其数据的自适应表单的名称。
   * 使用Felix Web控制台部署insertdata.core-0.0.1-SNAPSHOT.jar。 此捆绑包具有将提交的表单数据写入数据库的代码

* 转到 [ConfigMgr](http://localhost:4502/system/console/configMgr)

   * 搜索“JDBC连接池”。 新建一个Day Commons JDBC连接池。 指定特定于数据库的设置。

   * ![jdbc连接池](assets/jdbc-connection-pool.png)
   * 搜索“将表&#x200B;**单数据插入数据库**”
   * 指定数据库的特定属性。
      * DataSourceName：您之前配置的数据源的名称。
      * TableName —— 要在其中存储AF数据的表的名称
      * FormName —— 用于保存表单名称的列名称
      * ColumnName —— 用于保存AF数据的列名

   ![插入数据](assets/insertdata.PNG)

* 创建自适应表单。

* 将自适应表单与AEM工作流(StoreAFValueriseDB)关联，如下面的屏幕截图所示。

* 确保在数据文件路径中指定“data.xml”，如下面的屏幕截图所示

   ![提交](assets/submissionafforms.png)

* 预览表单并提交

* 如果一切顺利，您应看到表单数据存储在您指定的表和列中



