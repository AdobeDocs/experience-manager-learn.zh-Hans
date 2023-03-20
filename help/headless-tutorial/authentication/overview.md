---
title: 从外部应用程序对AEMas a Cloud Service进行身份验证
description: 了解外部应用程序如何使用本地开发访问令牌和服务凭据通过HTTP以编程方式验证AEMas a Cloud Service并与之交互。
version: Cloud Service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330460.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
exl-id: 63c23f22-533d-486c-846b-fae22a4d68db
source-git-commit: 8fc36698f06fea0eaaf818867c7e713453e0452d
workflow-type: tm+mt
source-wordcount: '641'
ht-degree: 0%

---

# 基于令牌的AEMas a Cloud Service身份验证

AEM公开了各种可以无头方式进行交互的HTTP端点，从GraphQL、AEM内容服务到资产HTTP API。 通常，这些无头用户可能需要对AEM进行身份验证才能访问受保护的内容或操作。 为了实现此目的，AEM支持对来自外部应用程序、服务或系统的HTTP请求进行基于令牌的身份验证。

在本教程中，您将详细了解外部应用程序如何使用访问令牌通过HTTP以编程方式对AEMas a Cloud Service进行身份验证并与之进行交互。

>[!VIDEO](https://video.tv.adobe.com/v/330460/?quality=12&learn=on)

## 先决条件

在随本教程一起学习之前，请确保已完成以下操作：

1. 对AEMas a Cloud Service环境（最好是开发环境或沙盒项目）的访问
1. AEMas a Cloud Service环境的创作服务AEM管理员产品配置文件中的成员资格
1. 您的Adobe IMS组织管理员的成员资格或访问权限(他们必须对 [服务凭据](./service-credentials.md))
1. 最新 [WKND站点](https://github.com/adobe/aem-guides-wknd) 部署到Cloud Service环境

## 外部应用程序概述

本教程使用 [简单Node.js应用程序](./assets/aem-guides_token-authentication-external-application.zip) 从命令行中运行，以在AEM as a Cloud Service上使用 [资产HTTP API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html).

Node.js应用程序的执行流程如下所示：

![外部应用程序](./assets/overview/external-application.png)

1. Node.js应用程序可从命令行调用
1. 命令行参数定义：
   + 要连接到的AEMas a Cloud Service创作服务主机(`aem`)
   + 更新资产的AEM资产文件夹(`folder`)
   + 要更新的元数据属性和值(`propertyName` 和 `propertyValue`)
   + 提供访问AEMas a Cloud Service所需凭据的文件的本地路径(`file`)
1. 用于对AEM进行身份验证的访问令牌是从通过命令行参数提供的JSON文件派生的 `file`

   a.如果在JSON文件(`file`)，则访问令牌将从Adobe IMS API中检索
1. 应用程序使用访问令牌访问AEM，并在命令行参数中指定的文件夹中列出所有资产 `folder`
1. 对于文件夹中的每个资产，应用程序会根据命令行参数中指定的属性名称和值更新其元数据 `propertyName` 和 `propertyValue`

虽然此示例应用程序是Node.js，但这些交互可以使用不同的编程语言开发并从其他外部系统执行。

## 本地开发访问令牌

为特定AEMas a Cloud Service环境生成本地开发访问令牌，以便提供对创作和发布服务的访问权限。  这些访问令牌是临时的，仅用于开发通过HTTP与AEM交互的外部应用程序或系统。 开发人员不必获取和管理受保护的服务凭据，而是可以快速轻松地自行生成临时访问令牌，以便开发其集成。

+ [如何使用本地开发访问令牌](./local-development-access-token.md)

## 服务凭据

服务凭据是任何非开发场景（最明显是生产场景）中使用的绑定凭据，它有助于外部应用程序或系统通过HTTPas a Cloud Service验证并与之交互的能力。 服务凭据本身不会发送到AEM进行身份验证，而是外部应用程序会使用这些凭据来生成JWT，JWT将与Adobe IMS的API交换 _表示_ 访问令牌，随后可用于对AEMas a Cloud Service的HTTP请求进行身份验证。

+ [如何使用服务凭据](./service-credentials.md)

## 其他资源

+ [下载示例应用程序](./assets/aem-guides_token-authentication-external-application.zip)
+ JWT创建和交换的其他代码示例
   + [Node.js、Java、Python、C#.NET和PHP代码示例](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples/)
   + [基于JavaScript/Axios的代码示例](https://github.com/adobe/aemcs-api-client-lib)
