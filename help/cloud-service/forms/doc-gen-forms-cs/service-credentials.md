---
title: AEM Forms服务凭据
description: 从AEM的Developer Console下载服务凭据。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-8192
thumbnail: 330519.jpg
exl-id: 74cb8c30-4c41-426c-a1b5-fc595a3167c8
duration: 453
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '109'
ht-degree: 0%

---

# AEM Forms服务凭据

与AEM as a Cloud Service的集成必须能够安全地向AEM进行身份验证。 AEM的Developer Console生成服务凭据，外部应用程序、系统和服务使用这些凭据以编程方式通过HTTP与AEM创作或发布服务交互。

>[!VIDEO](https://video.tv.adobe.com/v/342217?quality=12&learn=on&captions=chi_hans)

下载的服务凭据文件作为名为service_token.json的资源文件存储在提供的eclipse中。 service_token文件中的值用于生成JWT并将JWT交换为访问令牌。 实用程序类GetServiceCredentials用于从service_token.json资源文件中获取属性值。
