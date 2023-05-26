---
title: 在本地部署资源
description: 在本地AEM实例上部署教程资源
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-01-04T00:00:00Z
exl-id: d23b51ba-1efb-4505-b5b3-44a02177e467
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 3%

---

# 在您的系统上部署

请按照下面列出的步骤操作，以使此用例可在您的本地AEM实例上运行。

* [部署DevelopingWithServiceUser捆绑包](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip) 包含在zip文件中。

* 在Apache Sling服务用户映射器服务中添加以下条目 **DevelopingWithServiceUser.core：getformsresourceresolver=fd-service** 使用 [configMgr](http://localhost:4502/system/console/configMgr).

* [部署新闻稿捆绑包](assets/Newsletters.core-1.0.0-SNAPSHOT.jar). 此捆绑包包含用于列出文件夹内容和汇编所选新闻稿的代码。

* [使用包管理器导入包](assets/newsletter.zip). 此软件包包含用于测试解决方案的客户端库和示例pdf文件。

* [导入自适应表单示例](assets/sample-adaptive-form.zip). 此表单将列出可选择的新闻稿。

* [预览表单](http://localhost:4502/content/dam/formsanddocuments/downloadarchivednewsletters/jcr:content?wcmmode=disabled).
选择要下载的新闻稿。所选的新闻稿将合并为一个PDF并返回给您。
