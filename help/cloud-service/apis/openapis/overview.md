---
title: 基于OpenAPI的AEM API
description: 了解基于OpenAPI的AEM API，包括身份验证支持、关键概念以及如何访问Adobe API。
version: Experience Manager as a Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Architect, Developer, Leader
level: Beginner
doc-type: Article
jira: KT-16515
thumbnail: KT-16515.jpeg
last-substantial-update: 2025-02-28T00:00:00Z
duration: 0
exl-id: 0eb0054d-0c0a-4ac0-b7b2-fdaceaa6479b
source-git-commit: 182d7b76411ab885ab93b7724b97bf75cc52323c
workflow-type: tm+mt
source-wordcount: '1002'
ht-degree: 1%

---

# 基于OpenAPI的AEM API

>[!IMPORTANT]
>
>基于OpenAPI的AEM API仅在AEM as a Cloud Service中可用，与AEM 6.X不兼容。

了解基于OpenAPI的AEM API，包括身份验证支持、关键概念以及如何访问Adobe API。

[OpenAPI规范](https://swagger.io/specification/)（以前称为Swagger）是用于定义RESTful API的广泛使用的标准。 AEM as a Cloud Service提供了多个基于OpenAPI规范的API(或简单地说是基于开放的OpenAPI的AEM API)，使您能够更轻松地创建与AEM的创作或发布服务类型交互的自定义应用程序。 以下是一些示例：

**站点**

- [站点API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/sites/)：用于处理内容片段的API。

**Assets**

- [文件夹API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/)：用于处理文件夹（如创建、列出和删除文件夹）的API。

- [Assets创作API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/)：用于处理资源及其元数据的API。

**Forms**

- [Forms通信API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/)：用于处理表单和文档的API。

在未来的版本中，将添加更多基于OpenAPI的AEM API，以支持其他用例。

## 身份验证支持{#authentication-support}

基于OpenAPI的AEM API支持OAuth 2.0身份验证，包括以下授权类型：

- **OAuth服务器到服务器凭据**：非常适用于需要无需用户交互即可访问API的后端服务。 它使用&#x200B;_client_credentials_&#x200B;授权类型，在服务器级别启用安全访问管理。 有关详细信息，请参阅[OAuth服务器到服务器凭据](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/#oauth-server-to-server-credential)。

- **OAuth Web应用程序凭据**：适用于具有代表用户访问AEM API的前端和&#x200B;_后端_&#x200B;组件的Web应用程序。 它使用&#x200B;_authorization_code_&#x200B;授权类型，后端服务器可在此类型中安全地管理密钥和令牌。 有关详细信息，请参阅[OAuth Web App凭据](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation/#oauth-web-app-credential)。

- **OAuth单页应用程序凭据**：专为浏览器中运行的SPA设计，该SPA需要代表没有后端服务器的用户访问API。 它使用&#x200B;_authorization_code_&#x200B;授权类型，并依赖使用PKCE（代码交换的验证密钥）的客户端安全机制来保护授权代码流。 有关详细信息，请参阅[OAuth单页应用程序凭据](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation/#oauth-single-page-app-credential)。

## OAuth服务器到服务器与Web应用程序与单页应用程序凭据的区别{#difference-between-oauth-server-to-server-vs-web-app-vs-single-page-app-credentials}

下表总结了基于OpenAPI的AEM API支持的三种OAuth身份验证方法之间的差异：

|  | OAuth服务器到服务器 | OAuth Web应用程序 | OAuth单页面应用程序(SPA) |
| --- | --- | --- | --- |
| **身份验证目的** | 专为机器到机器的交互而设计。 | 专为具有&#x200B;_后端_&#x200B;的Web应用中的用户驱动交互而设计。 | 专为&#x200B;_客户端JavaScript应用程序_&#x200B;中的用户驱动交互而设计。 |
| **令牌行为** | 发出表示客户端应用程序本身的访问令牌。 | 通过后端&#x200B;_为经过身份验证的用户_&#x200B;颁发访问令牌。 | 通过仅前端流程&#x200B;_代表经过身份验证的用户_&#x200B;发出访问令牌。 |
| **用例** | 无需用户交互即需要API访问的后端服务。 | 具有代表用户访问API的前端和后端组件的Web应用程序。 | 纯前端(JavaScript)应用程序，代表没有后端的用户访问API。 |
| **安全注意事项** | 在后端系统中安全地存储敏感凭据(`client_id`， `client_secret`)。 | 用户身份验证后，通过后端调用&#x200B;_授予他们自己的_&#x200B;临时访问令牌。 在后端系统中安全地存储敏感凭据(`client_id`， `client_secret`)，以交换访问令牌的授权代码。 | 用户身份验证后，通过前端调用&#x200B;_授予他们自己的_&#x200B;临时访问令牌。 不使用`client_secret`，因为存储在前端应用是不安全的。 依靠PKCE交换访问令牌的授权码。 |
| **授予类型** | _client_credentials_ | _authorization_code_ | 具有&#x200B;**PKCE**&#x200B;的&#x200B;_authorization_code_ |
| **Adobe Developer Console凭据类型** | OAuth服务器到服务器 | OAuth Web应用程序 | OAuth单页应用程序 |
| **教程** | [使用服务器到服务器身份验证调用API](./use-cases/invoke-api-using-oauth-s2s.md) | [使用Web应用程序身份验证调用API](./use-cases/invoke-api-using-oauth-web-app.md) | [使用单页应用程序身份验证调用API](./use-cases/invoke-api-using-oauth-single-page-app.md) |

## 访问Adobe API和相关概念{#accessing-adobe-apis-and-related-concepts}

在访问Adobe API之前，必须了解以下关键结构：

- **[Adobe Developer Console](https://developer.adobe.com/)**：用于访问Adobe API、SDK、实时事件、无服务器函数等的开发人员中心。 请注意，它不同于用于调试AEM应用程序的&#x200B;_AEM_ Developer Console。

- **[Adobe Developer Console项目](https://developer.adobe.com/developer-console/docs/guides/projects/)**：管理API集成、事件和运行时函数的中心位置。 在这里，您可以配置API、设置身份验证并生成所需的凭据。

- **[产品配置文件](https://helpx.adobe.com/cn/enterprise/using/manage-product-profiles.html)**：产品配置文件提供权限预设，可让您控制用户或应用程序对Adobe产品(如AEM、Adobe Target、Adobe Analytics等)的访问权限。 每个Adobe产品都有与其关联的预定义产品配置文件。

- **服务**：服务定义实际权限并与产品配置文件关联。 要减少或增加权限预设，您可以取消选择或选择与产品配置文件关联的服务。 因此，允许您控制对产品及其API的访问权限级别。 在AEM as a Cloud Service中，服务表示具有存储库节点预定义访问控制列表(ACL)的用户组，从而允许精细的权限管理。

## 开始使用

了解如何设置AEM as a Cloud Service环境和一个Adobe Developer Console项目，以启用对基于OpenAPI的AEM API的访问。 还可使用浏览器访问AEM API，以验证设置并查看请求和响应。

<!-- CARDS
{target = _self}

* ./setup.md
  {title = Set up OpenAPI-based AEM APIs}
  {description = Learn how to set up your AEM as a Cloud Service environment to enable access to the OpenAPI-based AEM APIs.}
  {image = ./assets/setup/OpenAPI-Setup.png}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Set up OpenAPI-based AEM APIs">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup.md" title="设置基于OpenAPI的AEM API" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/setup/OpenAPI-Setup.png" alt="设置基于OpenAPI的AEM API"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup.md" target="_self" rel="referrer" title="设置基于OpenAPI的AEM API">设置基于OpenAPI的AEM API</a>
                    </p>
                    <p class="is-size-6">了解如何设置AEM as a Cloud Service环境，以便能够访问基于OpenAPI的AEM API。</p>
                </div>
                <a href="./setup.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解详情</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


## API教程

了解如何使用基于开放式API的AEM API，并使用不同的OAuth身份验证方法：

<!-- CARDS
{target = _self}

* ./use-cases/invoke-api-using-oauth-s2s.md
  {title = Invoke API using Server-to-Server authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom NodeJS application using OAuth Server-to-Server authentication.}
  {image = ./assets/s2s/OAuth-S2S.png}
* ./use-cases/invoke-api-using-oauth-web-app.md
  {title = Invoke API using Web App authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom web application using OAuth Web App authentication.}
  {image = ./assets/web-app/OAuth-WebApp.png}
* ./use-cases/invoke-api-using-oauth-single-page-app.md
  {title = Invoke API using Single Page App authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom Single Page App (SPA) using OAuth Single Page App authentication.}
  {image = ./assets/spa/OAuth-SPA.png}  
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Server-to-Server authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/invoke-api-using-oauth-s2s.md" title="使用服务器到服务器身份验证调用API" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/s2s/OAuth-S2S.png" alt="使用服务器到服务器身份验证调用API"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/invoke-api-using-oauth-s2s.md" target="_self" rel="referrer" title="使用服务器到服务器身份验证调用API">使用服务器到服务器身份验证调用API</a>
                    </p>
                    <p class="is-size-6">了解如何使用OAuth服务器到服务器身份验证从自定义NodeJS应用程序调用基于OpenAPI的AEM API。</p>
                </div>
                <a href="./use-cases/invoke-api-using-oauth-s2s.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解详情</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Web App authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/invoke-api-using-oauth-web-app.md" title="使用Web应用程序身份验证调用API" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/web-app/OAuth-WebApp.png" alt="使用Web应用程序身份验证调用API"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/invoke-api-using-oauth-web-app.md" target="_self" rel="referrer" title="使用Web应用程序身份验证调用API">使用Web应用程序身份验证调用API</a>
                    </p>
                    <p class="is-size-6">了解如何使用OAuth Web应用程序身份验证，从自定义Web应用程序调用基于OpenAPI的AEM API。</p>
                </div>
                <a href="./use-cases/invoke-api-using-oauth-web-app.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解详情</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Single Page App authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/invoke-api-using-oauth-single-page-app.md" title="使用单页应用程序身份验证调用API" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/spa/OAuth-SPA.png" alt="使用单页应用程序身份验证调用API"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/invoke-api-using-oauth-single-page-app.md" target="_self" rel="referrer" title="使用单页应用程序身份验证调用API">使用单页应用程序身份验证调用API</a>
                    </p>
                    <p class="is-size-6">了解如何使用OAuth单页应用程序身份验证，从自定义单页应用程序(SPA)调用基于OpenAPI的AEM API。</p>
                </div>
                <a href="./use-cases/invoke-api-using-oauth-single-page-app.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解详情</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
