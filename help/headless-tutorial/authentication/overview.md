---
title: 从外部应用程序以Cloud Service身份验证AEM
description: 了解外部应用程序如何使用本地开发访问令牌和服务凭据以HTTP方式作为Cloud Service以编程方式验证身份并与AEM交互。
version: cloud-service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330460.jpg
translation-type: tm+mt
source-git-commit: c4f3d437b5ecfe6cb97314076cd3a5e31b184c79
workflow-type: tm+mt
source-wordcount: '585'
ht-degree: 0%

---


# 将基于令牌的身份验证作为Cloud Service到AEM

在本教程中，您将详细了解外部应用程序如何使用访问令牌以编程方式验证AEM并将其作为HTTP上的Cloud Service与之交互。

>[!VIDEO](https://video.tv.adobe.com/v/330460/?quality=12&learn=on)

## 先决条件

在继续学习本教程之前，请确保已做好以下准备：

1. 作为Cloud Service环境(最好是开发环境或沙箱项目)访问am AEM
1. 作为Cloud Service环境的作者服务AEM管理员产品用户档案，成为AEM的会员
1. 您的AdobeIMS组织管理员的成员资格或访问权限（他们必须执行一次性初始化[服务凭据](./service-credentials.md)）
1. 部署到您的Cloud Service环境的最新[WKND站点](https://github.com/adobe/aem-guides-wknd)

## 外部应用程序概述

本教程使用命令行中运行的[简单的Node.js应用程序](./assets/aem-guides_token-authentication-external-application.zip)将AEM上的资产元数据作为Cloud Service更新为使用[资产HTTP API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html)。

Node.js应用程序的执行流程如下：

![外部应用程序](./assets/overview/external-application.png)

1. 从命令行调用Node.js应用程序
1. 命令行参数定义：
   + AEM作为要连接到的Cloud Service作者服务主机(`aem`)
   + 将更新其资产的AEM资产文件夹(`folder`)
   + 要更新的元数据属性和值（`propertyName`和`propertyValue`）
   + 文件的本地路径，提供作为Cloud Service访问AEM所需的凭据(`file`)
1. 用于验证AEM的访问令牌源自通过命令行参数`file`提供的JSON文件

   a.如果JSON文件(`file`)中提供了用于非本地开发的服务凭据，则从AdobeIMS API检索访问令牌
1. 应用程序使用访问令牌访问AEM并列表命令行参数`folder`中指定的文件夹中的所有资源
1. 对于文件夹中的每个资产，应用程序会根据命令行参数`propertyName`和`propertyValue`中指定的属性名称和值更新其元数据

虽然此示例应用程序是Node.js，但这些交互可以使用不同的编程语言进行开发并从其他外部系统执行。

## 本地开发访问令牌

为特定AEM生成本地开发访问令牌，作为Cloud Service环境，并提供对创作和发布服务的访问。  这些访问令牌是临时的，仅用于开发通过HTTP与AEM交互的外部应用程序或系统。 开发者不必获得和管理固定服务凭据，他们可以快速而轻松地自行生成临时访问令牌，使他们能够开发其集成。

+ [如何使用本地开发访问令牌](./local-development-access-token.md)

## 服务凭据

服务凭据是任何非开发场景（最明显是生产场景）中使用的绑定凭据，这些凭据有助于外部应用程序或系统以AEM身份通过HTTPCloud Service进行身份验证并与之交互。 服务凭据本身不会发送到AEM进行身份验证，而是外部应用程序使用这些凭据生成JWT,JWT与AdobeIMS的API _交换作为访问令牌的_，然后该API可用于验证作为Cloud Service的HTTP请求。

+ [如何使用服务凭据](./service-credentials.md)

## 其他资源

+ [下载示例应用程序](./assets/aem-guides_token-authentication-external-application.zip)
+ JWT创建和交换的其他代码范例
   + [Node.js、Java、Python、C#.NET和PHP代码范例](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md)
   + [基于JavaScript/Axios的代码范例](https://github.com/adobe/aemcs-api-client-lib)
