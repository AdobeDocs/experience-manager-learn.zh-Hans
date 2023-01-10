---
title: 选择并下载DAM文件夹内容
description: 在本地AEM实例上部署教程资产
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-01-04T00:00:00Z
source-git-commit: a2bbb26751c9182056b4fe6d36eeeec964001df8
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 0%

---

# 在您的系统上部署

请按照下面列出的步骤操作，以便在本地AEM实例上使用此用例。

* [按照本文中所述的步骤配置以使用fd-service用户](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=en). 确保已部署DevelopingWithServiceUser包。

* [部署新闻稿包](assets/Newsletters.core-1.0.0-SNAPSHOT.jar). 此包包含用于列出文件夹内容和组合所选新闻稿的代码。

* [使用包管理器导入包](assets/newsletter.zip). 此包包含客户端库和用于测试解决方案的示例pdf文件。

* [导入示例自适应表单](assets/sample-adaptive-form.zip). 此表单将列出可选的新闻稿。

* [预览表单](http://localhost:4502/content/dam/formsanddocuments/downloadarchivednewsletters/jcr:content?wcmmode=disabled).
选择要下载的几个新闻稿。选定的新闻稿将合并为一个pdf并返回给您。




