---
title: 为AEM GraphQL部署SPA
description: 了解单页应用程序(SPA) AEM Headless部署的部署注意事项。
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
exl-id: 3fe175f7-6213-439a-a02c-af3f82b6e3b7
duration: 136
source-git-commit: 6425188da75f789b0661ec9bfb79624b5704c92b
workflow-type: tm+mt
source-wordcount: '640'
ht-degree: 5%

---

# AEM Headless SPA部署

AEM Headless单页应用程序(SPA)部署涉及基于JavaScript的应用程序，这些应用程序使用诸如React或Vue之类的框架构建，这些框架能够以无头方式使用AEM中的内容并与之交互。

部署可与AEM以Headless方式交互的SPA包括托管SPA并使其可通过Web浏览器访问。

## 托管SPA

SPA由本机Web资源的集合组成：**HTML、CSS和JavaScript**。 这些资源在&#x200B;_生成_&#x200B;过程中生成（例如，`npm run build`），并部署到主机以供最终用户使用。

根据您组织的要求，有各种&#x200B;**托管**&#x200B;选项：

1. **云提供商**，如&#x200B;**Azure**&#x200B;或&#x200B;**AWS**。

2. 在公司&#x200B;**数据中心内托管**&#x200B;内部部署&#x200B;**&#x200B;**

3. **前端托管平台**，如&#x200B;**AWS Amplify**、**Azure App Service**、**Netlify**、**Heroku**、**Vercel**&#x200B;等。

## 部署配置

在托管与AEM Headless交互的SPA时，主要考虑是通过AEM的域（或主机）访问SPA，还是将其放在其他域中。  原因是SPA是在Web浏览器中运行的Web应用程序，因此受Web浏览器安全策略的约束。

### 共享域

当最终用户从同一域访问两个SPA域时，SPA和AEM将共享这两个域。 例如：

+ 通过以下方式访问AEM：`https://wknd.site/`
+ 通过`https://wknd.site/spa`访问SPA

由于AEM和SPA都可从同一域访问，因此Web浏览器允许SPA无需CORS即可对AEM Headless端点执行XHR操作，并且允许共享HTTP Cookie(例如AEM的`login-token` Cookie)。

如何在共享域上路由SPA和AEM流量，这取决于您：具有多个源的CDN、具有反向代理的HTTP服务器、直接在AEM中托管SPA等等。

以下是SPA生产部署所需的部署配置，在与AEM相同的域上托管时。

| SPA连接到→ | AEM 作者 | AEM Publish | AEM预览 |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher筛选器](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| 跨源资源共享(CORS) | ✘ | ✘ | ✘ |
| AEM 主机 | ✘ | ✘ | ✘ |

### 不同域

当来自不同域的最终用户访问SPA和AEM时，这两个域会有所不同。 例如：

+ 通过以下方式访问AEM：`https://wknd.site/`
+ 通过`https://wknd-app.site/`访问SPA

由于AEM和SPA是从不同的域访问的，因此Web浏览器会强制实施安全策略(如[跨源资源共享(CORS)](./configurations/cors.md))，并阻止共享HTTP Cookie(如AEM的`login-token` Cookie)。

以下是SPA生产部署所需的部署配置(当托管在不同于AEM的域上时)。

| SPA连接到→ | AEM 作者 | AEM Publish | AEM预览 |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher筛选器](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [跨源资源共享(CORS)](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [AEM主机](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

#### 不同域上的SPA部署示例

在此示例中，SPA部署到Netlify域(`https://main--sparkly-marzipan-b20bf8.netlify.app/`)，并且SPA使用来自AEM发布域(`https://publish-p65804-e666805.adobeaemcloud.com`)的AEM GraphQL API。 以下屏幕截图突出显示CORS要求。

1. SPA由Netlify域提供，但会向其他域上的AEM GraphQL API发起XHR调用。 此跨站点请求需要在AEM上设置[CORS](./configurations/cors.md)，以允许来自Netlify域的请求访问其内容。

   从SPA和AEM主机提供的![SPA请求](assets/spa/cors-requirement.png)

2. 在检查对AEM GraphQL API的XHR请求时，`Access-Control-Allow-Origin`存在，这向Web浏览器表示，AEM允许来自此Netlify域的请求访问其内容。

   如果AEM [CORS](./configurations/cors.md)缺失或不包含Netlify域，则Web浏览器会导致XHR请求失败，并报告CORS错误。

   ![CORS响应标头AEM GraphQL API](assets/spa/cors-response-headers.png)

## 单页应用程序示例

Adobe提供了一个在React中编码的示例单页应用程序。

<!-- CARDS 

* ../example-apps/react-app.md

-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="React App - AEM Headless Example">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="../example-apps/react-app.md" title="React应用程序 — AEM Headless示例" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="../example-apps/assets/react-app/react-app.png" alt="React应用程序 — AEM Headless示例"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="../example-apps/react-app.md" target="_blank" rel="referrer" title="React应用程序 — AEM Headless示例">React应用程序 — AEM Headless示例</a>
                    </p>
                    <p class="is-size-6">示例应用程序是探索 Adobe Experience Manager (AEM) 的 Headless 功能的好方法。此React应用程序演示了如何使用AEM的GraphQL API通过持久化查询来查询内容。</p>
                </div>
                <a href="../example-apps/react-app.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解详情</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


