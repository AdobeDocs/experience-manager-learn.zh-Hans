---
title: AEM服务凭据
description: 从AEM的Developer Console下载服务凭据。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-9980
exl-id: 4c5173f1-d57d-43ac-83e6-399ce4ead203
duration: 453
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '106'
ht-degree: 0%

---

# 服务凭据

与AEM as a Cloud Service的集成必须能够安全地向AEM进行身份验证。 AEM的Developer Console会生成服务凭据，外部应用程序、系统和服务使用这些凭据以编程方式通过HTTP与AEM创作或Publish服务进行交互。

>[!VIDEO](https://video.tv.adobe.com/v/330519?quality=12&learn=on)

下载的服务凭据文件作为名为service_token.json的资源文件存储在提供的eclipse中。 service_token文件中的值用于生成JWT并将JWT交换为访问令牌。 实用程序类GetServiceCredentials用于从service_token.json资源文件中获取属性值。
