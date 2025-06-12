---
title: 从外部应用程序向 AEM as a Cloud Service 进行身份验证
description: 了解外部应用程序如何使用本地开发访问令牌和服务凭据，通过 HTTP 以编程方式进行身份验证并与 AEM as a Cloud Service over HTTP 进行交互。
version: Experience Manager as a Cloud Service
feature: APIs
jira: KT-6785
thumbnail: 330460.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
doc-type: Tutorial
exl-id: 63c23f22-533d-486c-846b-fae22a4d68db
duration: 253
source-git-commit: bb4f9982263a15f18b9f39b1577b61310dfbe643
workflow-type: ht
source-wordcount: '621'
ht-degree: 100%

---

# 向 AEM as a Cloud Service 进行基于令牌的身份验证

AEM 提供了多种 HTTP 端点，支持以 Headless 方式进行交互，其中包括 GraphQL、AEM 内容服务以及资产 HTTP API 等。通常，这些 Headless 消费者需要对 AEM 进行身份认证，以访问受保护的内容或执行相应操作。为此，AEM 支持来自外部应用程序、服务或系统的 HTTP 请求的基于令牌的身份验证。

在本教程中，我们将探讨外部应用程序如何使用访问令牌，通过 HTTP 以编程方式对 AEM as a Cloud Service 进行身份验证和交互。

>[!VIDEO](https://video.tv.adobe.com/v/330460?quality=12&learn=on)

## 先决条件

在开始本教程之前，请确保具备以下条件：

1. 拥有 AEM as a Cloud Service 环境的访问权限（建议使用开发环境或沙盒环境）
1. 属于该 AEM as a Cloud Service 环境中的 Author 服务 AEM 管理员产品轮廓的角色
1. 是 Adobe IMS 组织管理员成员或可访问该角色的人员（他们需执行[服务凭据](./service-credentials.md)的一次性初始化）
1. 最新版本的 [WKND 网站](https://github.com/adobe/aem-guides-wknd)已部署至您的 Cloud Service 环境

## 外部应用程序概述

本教程使用一个[简单的 Node.js 应用程序](./assets/aem-guides_token-authentication-external-application.zip)，该应用程序从命令行运行，使用 [Assets HTTP API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html) 更新 AEM as a Cloud Service 上的资产元数据。

Node.js 应用程序的执行流程如下：

![外部应用程序](./assets/overview/external-application.png)

1. 该 Node.js 应用程序通过命令行调用
1. 命令行参数定义如下：
   + 要连接的 AEM as a Cloud Service Author 服务主机 (`aem`)
   + 已更新资产的 AEM 资产文件夹 (`folder`)
   + 需要更新的元数据属性名和属性值（`propertyName` 和 `propertyValue`）
   + 本地文件路径，其中提供访问 AEM as a Cloud Service 所需凭据 (`file`)
1. 用于认证 AEM 的访问令牌由命令行参数 `file` 提供的 JSON 文件生成

   a. 如果 JSON 文件 (`file`) 中包含非本地开发使用的服务凭据，则访问令牌会通过 Adobe IMS API 获取
1. 该应用程序使用访问令牌访问 AEM，并会列出命令行参数 `folder` 指定的文件夹中的所有资产
1. 对于文件夹中的每个资产，应用程序根据命令行参数 `propertyName` 和 `propertyValue` 指定的元数据属性名和属性值，更新该资产的元数据。

虽然该示例应用程序为 Node.js，但这些交互同样可以使用其他编程语言开发，并在其他外部系统中执行。

## 本地开发访问令牌

本地开发访问令牌是为特定的 AEM as a Cloud Service 环境生成的，并为 Author 和 Publish 服务提供访问权限。这些访问令牌具有临时性，仅供开发与 AEM 通过 HTTP 交互的外部应用程序或系统时使用。开发者无需获取和管理正式的服务凭据，即可快速自助生成临时访问令牌，方便进行集成开发。

+ [如何使用本地开发访问令牌](./local-development-access-token.md)

## 服务凭据

服务凭据是用于非开发环境（尤其是生产环境）中的正式凭证，其支持外部应用或系统通过 HTTP 认证并与 AEM as a Cloud Service 进行交互。服务凭据本身不会直接发送给 AEM 进行身份验证，外部应用会使用它们来生成 JWT，然后通过 Adobe IMS 的 API __&#x200B;交换获得访问令牌，该令牌随后用于认证发送到 AEM as a Cloud Service 的 HTTP 请求。

+ [如何使用服务凭据](./service-credentials.md)

## 其他资源

+ [下载示例应用程序](./assets/aem-guides_token-authentication-external-application.zip)
+ 创建和交换 JWT 的其他代码示例
   + [Node.js、Java、Python、C#.NET 和 PHP 代码示例](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples)
   + [基于 JavaScript/Axios 的代码示例](https://github.com/adobe/aemcs-api-client-lib)
