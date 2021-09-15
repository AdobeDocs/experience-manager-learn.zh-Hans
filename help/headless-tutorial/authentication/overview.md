---
title: 从外部应用程序验证AEM作为Cloud Service
description: 了解外部应用程序如何使用本地开发访问令牌和服务凭据通过HTTP以编程方式验证AEM身份并与其作为Cloud Service进行交互。
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
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '644'
ht-degree: 0%

---

# 基于令牌的AEM身份验证作为Cloud Service

AEM公开了各种可以无头方式交互的HTTP端点，从GraphQL、AEM内容服务到资产HTTP API。 通常，这些无头用户可能需要对AEM进行身份验证才能访问受保护的内容或操作。 为了实现此目的，AEM支持对来自外部应用程序、服务或系统的HTTP请求进行基于令牌的身份验证。

在本教程中，您将详细了解外部应用程序如何使用访问令牌以编程方式验证AEM as a HTTPCloud Service并与之进行交互。

>[!VIDEO](https://video.tv.adobe.com/v/330460/?quality=12&learn=on)

## 先决条件

在随本教程一起学习之前，请确保已完成以下操作：

1. 将am AEM作为Cloud Service环境（最好是开发环境或沙盒项目）访问
1. AEM as aCloud Service环境的创作服务AEM管理员产品配置文件中的成员资格
1. 您的Adobe IMS组织管理员的成员资格或访问权限（他们必须执行一次性初始化[服务凭据](./service-credentials.md)）
1. 部署到您的Cloud Service环境的最新[WKND Site](https://github.com/adobe/aem-guides-wknd)

## 外部应用程序概述

本教程使用从命令行运行的[简单的Node.js应用程序](./assets/aem-guides_token-authentication-external-application.zip)，以使用[Assets HTTP API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html)更新AEM上的资产元数据作为Cloud Service。

Node.js应用程序的执行流程如下所示：

![外部应用程序](./assets/overview/external-application.png)

1. Node.js应用程序可从命令行调用
1. 命令行参数定义：
   + 要连接到的AEM as a Cloud Service作者服务主机(`aem`)
   + 要更新资产的AEM资产文件夹(`folder`)
   + 要更新的元数据属性和值（`propertyName`和`propertyValue`）
   + 文件的本地路径，提供作为Cloud Service访问AEM所需的凭据(`file`)
1. 用于对AEM进行身份验证的访问令牌是从通过命令行参数`file`提供的JSON文件中派生的

   a.如果JSON文件(`file`)中提供了用于非本地开发的服务凭据，则会从Adobe IMS API中检索访问令牌
1. 应用程序使用访问令牌访问AEM，并列出命令行参数`folder`中指定的文件夹中的所有资产
1. 对于文件夹中的每个资产，应用程序会根据命令行参数`propertyName`和`propertyValue`中指定的属性名称和值更新其元数据

虽然此示例应用程序是Node.js，但这些交互可以使用不同的编程语言开发并从其他外部系统执行。

## 本地开发访问令牌

特定AEM作为Cloud Service环境并提供对创作和发布服务的访问权限时，会生成本地开发访问令牌。  这些访问令牌是临时的，仅用于开发通过HTTP与AEM交互的外部应用程序或系统。 开发人员不必获取和管理受保护的服务凭据，而是可以快速轻松地自行生成临时访问令牌，以便开发其集成。

+ [如何使用本地开发访问令牌](./local-development-access-token.md)

## 服务凭据

服务凭据是任何非开发场景（最明显是生产场景）中使用的绑定凭据，有助于外部应用程序或系统通过HTTP验证AEM作为Cloud Service并与之交互的能力。 服务凭据本身不会发送到AEM进行身份验证，而是外部应用程序会使用这些凭据来生成JWT，JWT会与Adobe IMS的API _交换，以用于_&#x200B;访问令牌，随后，该令牌可用于验证向AEM的HTTP请求作为Cloud Service。

+ [如何使用服务凭据](./service-credentials.md)

## 其他资源

+ [下载示例应用程序](./assets/aem-guides_token-authentication-external-application.zip)
+ JWT创建和交换的其他代码示例
   + [Node.js、Java、Python、C#.NET和PHP代码示例](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md)
   + [基于JavaScript/Axios的代码示例](https://github.com/adobe/aemcs-api-client-lib)
