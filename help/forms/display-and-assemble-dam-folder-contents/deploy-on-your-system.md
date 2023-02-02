---
title: 在本地部署资产
description: 在本地AEM实例上部署教程资产
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-01-04T00:00:00Z
source-git-commit: ddef90067d3ae4a3c6a705b5e109e474bab34f6d
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 3%

---

# 在您的系统上部署

请按照下面列出的步骤操作，以便在本地AEM实例上使用此用例。

* [部署DevelopingWithServiceUser包](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip) 包含在zip文件中。

* 在Apache Sling Service用户映射器服务中添加以下条目 **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** 使用 [configMgr](http://localhost:4502/system/console/configMgr).

* [部署新闻稿包](assets/Newsletters.core-1.0.0-SNAPSHOT.jar). 此包包含用于列出文件夹内容和组合所选新闻稿的代码。

* [使用包管理器导入包](assets/newsletter.zip). 此包包含客户端库和用于测试解决方案的示例pdf文件。

* [导入示例自适应表单](assets/sample-adaptive-form.zip). 此表单将列出可选的新闻稿。

* [预览表单](http://localhost:4502/content/dam/formsanddocuments/downloadarchivednewsletters/jcr:content?wcmmode=disabled).
选择要下载的几个新闻稿。选定的新闻稿将合并为一个pdf并返回给您。




