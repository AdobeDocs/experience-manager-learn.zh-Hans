---
title: AEM Headless服务器到服务器部署
description: 了解服务器到服务器AEM Headless部署的部署注意事项。
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10798
thumbnail: kt-10798.jpg
exl-id: d4ae08d9-dc43-4414-ab75-26853186a301
duration: 48
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 1%

---

# AEM Headless服务器到服务器部署

AEM Headless服务器到服务器部署涉及服务器端应用程序或流程，这些应用程序或流程以Headless方式使用AEM中的内容并与之交互。

服务器到服务器部署需要最少的配置，因为到AEM Headless API的HTTP连接不是在浏览器的上下文中启动的。

## 部署配置

必须为服务器到服务器应用程序部署就地以下部署配置。

| 服务器到服务器应用程序连接到→ | AEM 作者 | AEM 发布 | AEM预览 |
|---------------------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher筛选器](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| 跨源资源共享(CORS) | ✘ | ✘ | ✘ |
| [AEM主机](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## 授权要求

向AEM GraphQL API发出的授权请求通常发生在服务器到服务器应用程序的上下文中，因为其他应用程序类型（如[单页应用程序](./spa.md)、[移动设备](./mobile.md)或[Web组件](./web-component.md)）通常使用授权，因为保护凭据很困难。

向AEM as a Cloud Service授权请求时，使用[基于服务凭据的令牌身份验证](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html?lang=zh-Hans)。 要了解有关向AEM as a Cloud Service验证请求的更多信息，请查看[基于令牌的身份验证教程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=zh-Hans)。 本教程探讨了使用[AEM Assets HTTP API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets.html?lang=zh-Hans)的基于令牌的身份验证，但相同的概念和方法适用于与AEM Headless GraphQL API交互的应用程序。

## 示例服务器到服务器应用程序

Adobe提供了一个在Node.js中编码的示例服务器到服务器应用程序。

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
                   <p class="is-size-6">以Node.js编写的示例服务器到服务器应用程序，它使用AEM Headless GraphQL API中的内容。</p>
                   <a href="../example-apps/server-to-server-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">查看示例</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>
