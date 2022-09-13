---
title: 部署SPA for AEM GraphQL
description: 了解单页应用程序(SPA)AEM Headless部署的部署注意事项。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
source-git-commit: b2bf2a8e454d7ccd09819f2a38e58f7c303cb066
workflow-type: tm+mt
source-wordcount: '637'
ht-degree: 1%

---


# AEM Headless SPA部署

AEM无头单页应用程序(SPA)部署涉及基于JavaScript的应用程序，这些应用程序是使用React或Vue等框架构建的，它们以无头方式使用AEM中的内容并与之交互。

部署以无头方式交互AEM的SPA包括托管SPA并通过Web浏览器访问。

## 托管SPA

SPA由本机Web资源的集合组成： **HTML、CSS和JavaScript**. 这些资源在 _构建_ 流程(例如， `npm run build`)并部署到主机以供最终用户使用。

有多种 **托管** 选项取决于贵组织的要求：

1. **云提供商** 例如 **Azure** 或 **AWS**.

2. **内部部署** 在企业中托管 **数据中心**

3. **前端托管平台** 例如 **AWS Amplify**, **Azure应用程序服务**, **Netlify**, **赫罗库**, **韦尔塞尔**&#x200B;等。

## 部署配置

托管与AEM无头交互的SPA时，主要考虑的事项是：是通过AEM域（或主机）访问SPA，还是在其他域上访问。  原因是SPA是在Web浏览器中运行的Web应用程序，因此受Web浏览器安全策略的约束。

### 共享域

当SPA和AEM都由同一域的最终用户访问时，它们会共享域。 例如：

+ AEM的访问方式： `https://wknd.site/`
+ SPA通过 `https://wknd.site/spa`

由于AEM和SPA都从同一域访问，因此Web浏览器允许SPA在不需要CORS的情况下将XHR生成到AEM无头端点，并允许共享HTTP Cookie(例如AEM) `login-token` cookie)。

SPA和AEM流量如何在共享域上路由，取决于您：具有多个源的CDN、具有反向代理的HTTP服务器、直接在AEM中托管SPA等。

以下是SPA生产部署(在与AEM相同的域上托管)所需的部署配置。

| SPA连接到 | AEM Author | AEM 发布 | AEM预览 |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher过滤器](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| 跨源资源共享(CORS) | ✘ | ✘ | ✘ |
| AEM主机 | ✘ | ✘ | ✘ |

### 不同的域

最终用户从不同的域访问SPA和AEM时，它们会有不同的域。 例如：

+ AEM的访问方式： `https://wknd.site/`
+ SPA通过 `https://wknd-app.site/`

由于AEM和SPA是从不同的域访问的，因此Web浏览器会强制执行安全策略，例如 [跨源资源共享(CORS)](./configurations/cors.md)，并阻止共享HTTP Cookie(例如AEM `login-token` cookie)。

以下是SPA生产部署在与AEM不同的域上托管时所需的部署配置。

| SPA连接到 | AEM作者 | AEM 发布 | AEM预览 |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher过滤器](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [跨源资源共享(CORS)](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [AEM主机](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

#### 不同域上的SPA部署示例

在本例中，SPA被部署到Netlify域(`https://main--sparkly-marzipan-b20bf8.netlify.app/`),SPA会从AEM发布域(`https://publish-p65804-e666805.adobeaemcloud.com`)。 以下屏幕截图突出显示了CORS要求。

1. SPA来自Netlify域，但会在其他域上对AEM GraphQL API进行XHR调用。 此跨站点请求需要 [CORS](./configurations/cors.md) 在AEM上进行设置，以允许来自Netlify域的访问其内容的请求。

   ![SPA主机从SPA和AEM提供的请求 ](assets/spa/cors-requirement.png)

2. 检查对AEM GraphQL API的XHR请求， `Access-Control-Allow-Origin` 存在，表示AEM允许来自此Netlify域的请求访问其内容的Web浏览器。

   如果AEM [CORS](./configurations/cors.md) 缺少或未包含Netlify域，则Web浏览器将失败XHR请求，并报告CORS错误。

   ![CORS响应标头AEM GraphQL API](assets/spa/cors-response-headers.png)

## 单页应用程序示例

Adobe提供了一个在React中编码的单页应用程序示例。

<div class="columns is-multiline">
<!-- React app -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="React app" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="../example-apps/react-app.md" title="React应用程序" tabindex="-1">
                   <img class="is-bordered-r-small" src="../example-apps/assets/react-app/react-app-card.png" alt="React应用程序">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/react-app.md" title="React应用程序">React应用程序</a></p>
               <p class="is-size-6">以React编写的单页应用程序示例，用于使用AEM Headless GraphQL API中的内容。</p>
               <a href="../example-apps/react-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">查看示例</span>
               </a>
           </div>
       </div>
   </div>
</div>
<!-- Next.js app -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Next.js app" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="../example-apps/next-js.md" title="Next.js应用程序" tabindex="-1">
                   <img class="is-bordered-r-small" src="../example-apps/assets/next-js/next-js-card.png" alt="Next.js应用程序">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/next-js.md" title="Next.js应用程序">Next.js应用程序</a></p>
               <p class="is-size-6">使用Next.js编写的单页应用程序示例，用于使用AEM无头GraphQL API中的内容。</p>
               <a href="../example-apps/next-js.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">查看示例</span>
               </a>
           </div>
       </div>
   </div>
</div>
</div>
