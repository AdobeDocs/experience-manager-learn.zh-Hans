---
title: 在服务器上部署示例资源
description: 测试交互式通信的另存为草稿功能
feature: Interactive Communication
doc-type: article
version: Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
jira: KT-10208
exl-id: 9053ee29-436a-439a-b592-c3fef9852ea4
duration: 28
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '143'
ht-degree: 2%

---

# 在服务器上部署示例资源

请按照以下说明使此功能在您的AEM服务器上正常工作

* [创建数据库模式](assets/icdrafts.sql)
* [导入客户端库](assets/icdrafts.zip)
* [导入自适应表单](assets/SavedDraftsAdaptiveForm.zip)
* 创建名为&#x200B;_SaveAndContinue_&#x200B;的数据源

![创建数据Source](assets/data-source.png)

| 属性名称 | 属性值 |
|---|---|
| 数据源名称 | `SaveAndContinue` |
| JDBC驱动程序类 | `com.mysql.cj.jdbc.Driver` |
| JDBC连接URL | `jdbc:mysql://localhost:3306/aemformstutorial?autoReconnect=true&useSSL=false&characterEncoding=utf8&useUnicode=true` |

* [部署icdrafts包](assets/icdrafts.icdrafts.core-1.0-SNAPSHOT.jar)
* 请确保在OSGI配置中&#x200B;_使用CCRDocumentInstanceService_启用保存，如下所示
  ![启用草稿](assets/enable-drafts.png)
* 打开任意交互式通信。 单击另存为草稿以保存
* [查看已保存的草稿](http://localhost:4502/content/dam/formsanddocuments/saveddrafts/jcr:content?wcmmode=disabled)

>[!NOTE]
>xml文件存储在AEM服务器安装的根文件夹中。 Eclipse项目是提供给您根据您的要求自定义解决方案的。

可以从此处[&#128279;](assets/icdrafts-eclipse-project.zip)下载具有示例实施的eclipse项目
