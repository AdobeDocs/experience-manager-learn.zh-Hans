---
title: AEM服务凭据
description: 从AEM开发人员控制台下载服务凭据。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-9980
exl-id: 4c5173f1-d57d-43ac-83e6-399ce4ead203
duration: 455
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '106'
ht-degree: 0%

---

# 服务凭据

与AEMas a Cloud Service的集成必须能够安全地向AEM进行身份验证。 AEM开发人员控制台生成服务凭据，外部应用程序、系统和服务使用这些凭据以编程方式通过HTTP与AEM Author或Publish服务交互。

>[!VIDEO](https://video.tv.adobe.com/v/330519?quality=12&learn=on)

下载的服务凭据文件作为名为service_token.json的资源文件存储在提供的eclipse中。 service_token文件中的值用于生成JWT并将JWT交换为访问令牌。 实用程序类GetServiceCredentials用于从service_token.json资源文件中获取属性值。
