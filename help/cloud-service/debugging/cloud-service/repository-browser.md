---
title: 使用存储库浏览器调试AEM
description: Repository Browser是一款功能强大的工具，可显示AEM底层数据存储，从而轻松调试AEMas a Cloud Service环境。
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-10004
thumbnail: 341464.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 88af40fc-deff-4b92-84b1-88df2dbdd90b
duration: 318
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 0%

---

# 使用存储库浏览器调试AEMas a Cloud Service

Repository Browser是一款功能强大的工具，可显示AEM底层数据存储，从而轻松调试AEMas a Cloud Service环境。 存储库浏览器支持在生产、暂存和开发以及创作、发布和预览服务中，以只读方式查看AEM的资源和属性。

>[!VIDEO](https://video.tv.adobe.com/v/341464?quality=12&learn=on)

存储库浏览器是 __仅__ 在AEMas a Cloud Service环境中可用(使用 [CRXDE Lite](../aem-sdk-local-quickstart/other-tools.md#crxde-lite) 调试本地AEM SDK)。

## 访问存储库浏览器

要在AEMas a Cloud Service上访问“存储库浏览器”，请执行以下操作：

1. 确保您的用户具有 [所需的访问权限](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#access-prerequisites)
1. 登录 [Cloud Manager](https://my.cloudmanager.adobe.com)
1. 选择包含要调试的AEMas a Cloud Service环境的程序
1. 打开 [开发人员控制台](./developer-console.md) 与要调试的AEMas a Cloud Service环境相对应
1. 选择 __存储库浏览器__ 选项卡
1. 选择要浏览的AEM服务层
   + 所有作者
   + 所有发布者
   + 所有预览
1. 选择 __打开存储库浏览器__

存储库浏览器将以只读模式为选定的服务层（创作、发布或预览）打开，显示用户有权访问的资源和属性。

## 发布和预览访问权限

默认情况下，对发布或预览的访问权限有限，从而减少了存储库浏览器中的可用资源。 [要查看发布（或预览）上的所有资源，请将用户添加到发布（或预览）管理员角色。](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#navigate-the-hierarchy)
