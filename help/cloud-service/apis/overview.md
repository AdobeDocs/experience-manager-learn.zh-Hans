---
title: AEM API概述
description: 了解Adobe Experience Manager (AEM)中不同类型的API，并大致了解基于OpenAPI规范的API(通常称为基于OpenAPI的AEM API)。
version: Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Architect, Developer, Leader
level: Beginner
doc-type: Article
jira: KT-16515
thumbnail: KT-16515.jpeg
last-substantial-update: 2024-11-20T00:00:00Z
duration: 0
source-git-commit: 6b8a8dc5cdcddfa2d8572bfd195bc67906882f67
workflow-type: tm+mt
source-wordcount: '880'
ht-degree: 1%

---


# AEM API概述{#aem-apis-overview}

了解Adobe Experience Manager (AEM) as a Cloud Service中的各种API类型，并大致了解基于[OpenAPI Specification (OAS)](https://swagger.io/specification/)的AEM API，通常称为基于OpenAPI的AEM API。

AEM as a Cloud Service提供各种API用于创建、读取、更新和删除内容、资源和表单。 开发人员可以使用这些API创建与AEM交互的自定义应用程序。

让我们来探索AEM中的各种API类型，并了解访问AdobeAPI的关键概念。

## AEM API的类型{#types-of-aem-apis}

AEM提供了旧版和现代API，以便与其创作和发布服务类型交互。

- **旧版API**：在早期AEM版本中引入旧版API，为了向后兼容，仍支持旧版API。

- **现代API**：根据REST、OpenAPI规范，这些API遵循当前API设计最佳实践，建议用于新集成。


| AEM API类型 | 规范 | 可用性 | 用例 | 示例 |
| --- | --- | --- | --- | --- |
| 传统（非RESTful） API | Sling Servlet | AEM 6.X， AEM as a Cloud Service | 旧版集成，向后兼容性 | [查询生成器API](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/search/query-builder-api)及其他 |
| RESTful API | HTTP， JSON | AEM 6.X， AEM as a Cloud Service | CRUD操作，现代应用程序 | [Assets HTTP API](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets)、[工作流REST API](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/developing/extending-aem/extending-workflows/workflows-program-interaction#using-the-workflow-rest-api)、[内容服务的JSON导出程序](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/components-templates/json-exporter)等 |
| GRAPHQL API | GraphQL | AEM 6.X， AEM as a Cloud Service | Headless CMS、SPA、移动应用程序 | [GraphQL API](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments) |
| 基于OpenAPI的AEM API | REST， OpenAPI | **仅限AEM as a Cloud Service** | API优先开发，现代应用程序 | [Assets创作API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/)、[文件夹API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/)、[AEM Sites API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/sites/delivery/)、[Forms Acrobat服务](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/)等 |

>[!IMPORTANT]
>
>基于OpenAPI的AEM API仅在AEM as a Cloud Service中可用，与AEM 6.X不兼容。

有关AEM API的详细信息，请参阅[Adobe Experience Manager as a Cloud Service API](https://developer.adobe.com/experience-cloud/experience-manager-apis/)。

让我们更详细地了解一下基于OpenAPI的AEM API以及访问AdobeAPI的重要概念。

## 基于OpenAPI的AEM API{#openapi-based-aem-apis}

>[!AVAILABILITY]
>
>基于OpenAPI的AEM API作为早期访问计划的一部分提供。 如果您有兴趣访问它们，我们建议您通过电子邮件向[aem-apis@adobe.com](mailto:aem-apis@adobe.com)发送用例说明。

[OpenAPI规范](https://swagger.io/specification/)（以前称为Swagger）是用于定义RESTful API的广泛使用的标准。 AEM as a Cloud Service提供了多个基于OpenAPI规范的API(或简单地说，基于OpenAPI的AEM API)，使您能够更轻松地创建与AEM的创作或发布服务类型交互的自定义应用程序。 以下是一些示例：

**站点**

- [站点API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/sites/delivery/)：用于处理内容片段的API。

**Assets**

- [文件夹API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/)：用于处理文件夹（如创建、列出和删除文件夹）的API。

- [Assets创作API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/)：用于处理资源及其元数据的API。

**Forms**

- [Forms Acrobat服务](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/)：用于处理表单和文档的API。

在未来的版本中，将添加更多基于OpenAPI的AEM API以支持其他用例。

## 身份验证支持{#authentication-support}

基于OpenAPI的AEM API支持以下身份验证方法：

- **OAuth服务器到服务器凭据**：非常适用于需要无需用户交互即可访问API的后端服务。 它使用&#x200B;_client_credentials_&#x200B;授权类型，在服务器级别启用安全访问管理。 有关详细信息，请参阅[OAuth服务器到服务器凭据](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/#oauth-server-to-server-credential)。

- **OAuth Web应用程序凭据**：适用于具有代表用户访问AEM API的前端和&#x200B;_后端_&#x200B;组件的Web应用程序。 它使用&#x200B;_authorization_code_&#x200B;授权类型，后端服务器可在此类型中安全地管理密钥和令牌。 有关详细信息，请参阅[OAuth Web App凭据](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation/#oauth-web-app-credential)。

- **OAuth单页应用程序凭据**：专为浏览器中运行的SPA而设计，该浏览器需要代表没有后端服务器的用户访问API。 它使用&#x200B;_authorization_code_&#x200B;授权类型，并依赖使用PKCE（代码交换的验证密钥）的客户端安全机制来保护授权代码流。 有关详细信息，请参阅[OAuth单页应用程序凭据](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation/#oauth-single-page-app-credential)。

## 访问AdobeAPI和相关概念{#accessing-adobe-apis-and-related-concepts}

在访问AdobeAPI之前，必须了解以下关键概念：

- **[Adobe Developer Console](https://developer.adobe.com/)**：用于访问AdobeAPI、SDK、实时事件、无服务器函数等的开发人员中心。 请注意，这与用于调试AEM应用程序的&#x200B;_AEM_ Developer Console不同。

- **[Adobe Developer Console项目](https://developer.adobe.com/developer-console/docs/guides/projects/)**：管理API集成、事件和运行时函数的中心位置。 在这里，您可以配置API、设置身份验证并生成所需的凭据。

- **[产品配置文件](https://helpx.adobe.com/cn/enterprise/using/manage-product-profiles.html)**：产品配置文件提供权限预设，可让您控制用户或应用程序对Adobe产品(如AEM、Adobe Target、Adobe Analytics等)的访问权限。 每个Adobe产品都有与其关联的预定义产品配置文件。

- **服务**：服务定义实际权限并与产品配置文件关联。 要减少或增加权限预设，您可以取消选择或选择与产品配置文件关联的服务。 因此，允许您控制对产品及其API的访问权限级别。 在AEM as a Cloud Service中，服务表示具有存储库节点预定义访问控制列表(ACL)的用户组，从而允许精细的权限管理。

## 后续步骤{#next-steps}

了解不同的AEM API类型，包括
基于OpenAPI的AEM API以及访问AdobeAPI的关键概念，您现在可以开始构建与AEM交互的自定义应用程序。

让我们开始了解[如何调用基于OpenAPI的AEM API](invoke-openapi-based-aem-apis.md)教程。
