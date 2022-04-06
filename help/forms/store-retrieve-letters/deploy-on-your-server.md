---
title: 在服务器上部署示例资产
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
kt: 10208
source-git-commit: 0a52ea9f5a475814740bb0701a09f1a6735c6b72
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 0%

---

# 在服务器上部署示例资产

请按照以下说明操作，以便在AEM Server上使用此功能

* 在c驱动器中创建名为icdrafts的文件夹
* [创建数据库模式](assets/icdrafts.sql)
* [导入客户端库](assets/icdrafts.zip)
* [导入自适应表单](assets/SavedDraftsAdaptiveForm.zip)
* 创建数据源(名为 _SaveAndContinue_

![创建数据源](assets/data-source.png)

* [部署icdrafts包](assets/icdrafts.icdrafts.core-1.0-SNAPSHOT.jar)
* 确保 _使用CCRDocumentInstanceService启用保存_ ，如下所示
   ![启用草稿](assets/enable-drafts.png)
* 打开任何交互式通信。 单击另存为草稿以保存
* [查看保存的草稿](http://localhost:4502/content/dam/formsanddocuments/saveddrafts/jcr:content?wcmmode=disabled)

>[!NOTE]
>xml文件存储在AEM服务器安装的根文件夹中。 Eclipse项目将为您提供，以根据您的要求自定义解决方案。

包含实施示例的Eclipse项目可以 [从此处下载](assets/icdrafts-eclipse-project.zip)
