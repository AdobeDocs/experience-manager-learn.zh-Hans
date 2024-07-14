---
title: 在本地部署资产
description: 在本地AEM实例上部署教程资源
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-01-04T00:00:00Z
exl-id: d23b51ba-1efb-4505-b5b3-44a02177e467
duration: 27
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 0%

---

# 在您的系统上部署

请按照下面列出的步骤，使此用例在您的本地AEM实例上运行。

* [部署包含在zip文件中的DevelopingWithServiceUser包](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip)。

* 使用[configMgr](http://localhost:4502/system/console/configMgr)在Apache Sling服务用户映射器服务&#x200B;**DevelopingWithServiceUser.core：getformsresourceresolver=fd-service**&#x200B;中添加以下条目。

* [部署新闻稿包](assets/Newsletters.core-1.0.0-SNAPSHOT.jar)。 此包中包含用于列出文件夹内容并汇编选定新闻稿的代码。

* [使用包管理器导入包](assets/newsletter.zip)。 该软件包包含用于测试解决方案的客户端库和示例pdf文件。

* [导入自适应表单示例](assets/sample-adaptive-form.zip)。 此表单将列出可选择的新闻稿。

* [预览表单](http://localhost:4502/content/dam/formsanddocuments/downloadarchivednewsletters/jcr:content?wcmmode=disabled)。
选择要下载的新闻稿。所选的新闻稿将合并为一个PDF文件并返回给您。
