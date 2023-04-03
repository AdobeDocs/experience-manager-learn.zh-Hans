---
title: AEM Forms服务凭据
description: 从AEM开发人员控制台下载服务凭据。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 8192
thumbnail: 330519.jpg
exl-id: 74cb8c30-4c41-426c-a1b5-fc595a3167c8
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '112'
ht-degree: 0%

---

# AEM Forms服务凭据

与AEMas a Cloud Service的集成必须能够安全地对AEM进行身份验证。 AEM Developer Console会生成服务凭据，外部应用程序、系统和服务使用这些凭据，以编程方式通过HTTP与AEM创作或发布服务进行交互。

>[!VIDEO](https://video.tv.adobe.com/v/330519?quality=12&learn=on)

下载的服务凭据文件将作为名为service_token.json的资源文件存储在提供的Eclipse中。 service_token文件中的值用于生成JWT并将JWT交换为访问令牌。 实用程序类GetServiceCredentials用于从service_token.json资源文件中获取属性值。
