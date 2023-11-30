---
title: 在服务器上部署示例资源
description: 测试交互式通信的另存为草稿功能
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Intermediate
jira: KT-10208
exl-id: 9053ee29-436a-439a-b592-c3fef9852ea4
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 2%

---

# 在服务器上部署示例资源

请按照以下说明使此功能在您的AEM服务器上正常工作

* [创建数据库模式](assets/icdrafts.sql)
* [导入客户端库](assets/icdrafts.zip)
* [导入自适应表单](assets/SavedDraftsAdaptiveForm.zip)
* 创建名为的数据源 _SaveAndContinue_

![创建数据源](assets/data-source.png)

| 属性名称 | 属性值 |
|---|---|
| 数据源名称 | SaveAndContinue |
| JDBC驱动程序类 | com.mysql.cj.jdbc.Driver |
| JDBC连接URL | jdbc:mysql://localhost：3306/aemformstutorial？autoReconnect=true&amp;useSSL=false&amp;characterEncoding=utf8&amp;useUnicode=true |

* [部署icdrafts包](assets/icdrafts.icdrafts.core-1.0-SNAPSHOT.jar)
* 确保您 _使用CCRDocumentInstanceService启用保存_ 在OSGi配置中，如下所示
  ![启用草稿](assets/enable-drafts.png)
* 打开任意交互式通信。 单击另存为草稿以保存
* [查看已保存的草稿](http://localhost:4502/content/dam/formsanddocuments/saveddrafts/jcr:content?wcmmode=disabled)

>[!NOTE]
>xml文件存储在AEM Server安装的根文件夹中。 Eclipse项目是提供给您根据您的要求自定义解决方案的。

示例实施的eclipse项目可以是 [从此处下载](assets/icdrafts-eclipse-project.zip)
