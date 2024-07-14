---
title: 使用存储库浏览器调试AEM
description: Repository Browser是一款功能强大的工具，可显示AEM的底层数据存储，从而轻松调试AEM as a Cloud Service环境。
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-10004
thumbnail: 341464.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 88af40fc-deff-4b92-84b1-88df2dbdd90b
duration: 305
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 0%

---

# 使用存储库浏览器调试AEM as a Cloud Service

Repository Browser是一款功能强大的工具，可显示AEM的底层数据存储，从而轻松调试AEM as a Cloud Service环境。 存储库浏览器支持在生产、暂存和开发以及创作、Publish和预览服务中，以只读方式查看AEM的资源和属性。

>[!VIDEO](https://video.tv.adobe.com/v/341464?quality=12&learn=on)

存储库浏览器仅&#x200B;__在AEM as a Cloud Service环境中可用__(使用[CRXDE Lite](../aem-sdk-local-quickstart/other-tools.md#crxde-lite)调试本地AEM SDK)。

## 访问存储库浏览器

要在AEM as a Cloud Service上访问存储库浏览器，请执行以下操作：

1. 确保您的用户具有[所需的访问权限](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#access-prerequisites)
1. 登录到[Cloud Manager](https://my.cloudmanager.adobe.com)
1. 选择包含要调试的AEM as a Cloud Service环境的程序
1. 打开与AEM as a Cloud Service环境对应的[Developer Console](./developer-console.md)进行调试
1. 选择&#x200B;__存储库浏览器__&#x200B;选项卡
1. 选择要浏览的AEM服务层
   + 所有作者
   + 所有发布者
   + 所有预览
1. 选择&#x200B;__打开存储库浏览器__

存储库浏览器将以只读模式为选定的服务层(创作、Publish或预览)打开，显示用户有权访问的资源和属性。

## Publish和预览访问权限

默认情况下，对Publish或预览的访问权限有限，从而减少了存储库浏览器中的可用资源。 [要在Publish（或预览）中查看所有资源，请将用户添加到Publish（或预览）管理员角色。](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#navigate-the-hierarchy)
