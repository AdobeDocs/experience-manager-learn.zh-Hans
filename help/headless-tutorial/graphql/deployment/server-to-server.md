---
title: AEM无头服务器到服务器部署
description: 了解服务器到服务器AEM无头部署的部署注意事项。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10798
thumbnail: kt-10798.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 1%

---


# AEM无头服务器到服务器部署

AEM无头服务器到服务器部署涉及服务器端应用程序或进程，这些应用程序或进程以无头方式使用AEM中的内容并与之交互。

由于到AEM无头API的HTTP连接未在浏览器上下文中启动，因此服务器到服务器部署需要的配置最少。

## 部署配置

以下部署配置必须就地才能进行服务器到服务器应用程序部署。

| 服务器到服务器应用程序连接到 | AEM Author | AEM 发布 | AEM预览 |
|---------------------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher过滤器](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| 跨源资源共享(CORS) | ✘ | ✘ | ✘ |
| [AEM主机](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## 授权要求

对AEM GraphQL API的授权请求通常在服务器到服务器应用程序的上下文中发生，因为其他应用程序类型(例如 [单页应用程序](./spa.md), [移动设备](./mobile.md)或 [Web组件](./web-component.md)，通常使用授权，因为很难保护凭据。

对AEMas a Cloud Service的请求授权时，请使用 [基于服务凭据的令牌身份验证](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html). 要了解有关对AEMas a Cloud Service进行身份验证请求的更多信息，请查看 [基于令牌的身份验证教程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html). 本教程探索使用 [AEM Assets HTTP API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets.html) 但是，与AEM Headless GraphQL API交互的应用程序也适用相同的概念和方法。

## 服务器到服务器应用程序示例

Adobe提供了一个在Node.js中编码的服务器到服务器应用程序示例。

<div class="columns is-multiline">
    <!-- Server-to-server app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Server-to-server app" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/server-to-server-app.md" title="服务器到服务器应用程序" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/server-to-server-app/server-to-server-card.png" alt="服务器到服务器应用程序">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/server-to-server-app.md" title="服务器到服务器应用程序">服务器到服务器应用程序</a></p>
                   <p class="is-size-6">使用Node.js编写的示例服务器到服务器应用程序，使用AEM无头GraphQL API中的内容。</p>
                   <a href="../example-apps/server-to-server-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">查看示例</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>