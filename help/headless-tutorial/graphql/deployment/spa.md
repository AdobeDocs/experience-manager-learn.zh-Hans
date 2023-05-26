---
title: 部署SPA for AEM GraphQL
description: 了解单页应用程序(SPA) AEM Headless部署的部署注意事项。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
exl-id: 3fe175f7-6213-439a-a02c-af3f82b6e3b7
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '637'
ht-degree: 1%

---

# AEM Headless SPA部署

AEM无头单页应用程序(SPA)部署涉及使用框架（如React或Vue）构建的基于JavaScript的应用程序，这些框架以Headless方式使用AEM中的内容并与之交互。

部署以Headless方式与AEM交互的SPA涉及托管SPA并使其可通过Web浏览器访问。

## 托管SPA

SPA由一系列本机Web资源组成： **HTML、CSS和JavaScript**. 这些资源是在以下期间生成的 _生成_ 进程(例如， `npm run build`)，并部署到主机以供最终用户使用。

有多种类型 **托管** 选项取决于贵组织的要求：

1. **云提供商** 例如 **Azure** 或 **AWS**.

2. **内部部署** 在公司中托管 **数据中心**

3. **前端托管平台** 例如 **AWS放大**， **Azure应用程序服务**， **Netlify**， **赫罗库**， **韦尔塞尔**&#x200B;等。

## 部署配置

在托管与AEM Headless交互的SPA时，主要考虑是通过AEM域（或主机）访问SPA，还是将其放在其他域中。  原因是SPA是在Web浏览器中运行的Web应用程序，因此受Web浏览器安全策略的约束。

### 共享域

当SPA和AEM均由来自同一域的最终用户访问时，它们会共享域。 例如：

+ 通过以下方式访问AEM： `https://wknd.site/`
+ 通过以下方式访问SPA `https://wknd.site/spa`

由于AEM和SPA都可从同一域访问，因此Web浏览器允许SPA对AEM Headless端点执行XHR而不需要CORS，并允许共享HTTP Cookie(例如AEM) `login-token` Cookie)。

如何在共享域上路由SPA和AEM流量，具体取决于您：具有多个源的CDN、具有反向代理的HTTP服务器、直接在AEM中托管SPA等等。

以下是SPA生产部署所需的部署配置，当托管在与AEM相同的域上时。

| SPA连接到 | AEM Author | AEM 发布 | AEM预览 |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher过滤器](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| 跨源资源共享(CORS) | ✘ | ✘ | ✘ |
| AEM主机 | ✘ | ✘ | ✘ |

### 不同的域

当最终用户从不同域访问SPA和AEM时，这两个域各不相同。 例如：

+ 通过以下方式访问AEM： `https://wknd.site/`
+ 通过以下方式访问SPA `https://wknd-app.site/`

由于AEM和SPA可从不同域访问，因此Web浏览器会强制实施安全策略，例如 [跨源资源共享(CORS)](./configurations/cors.md)，并阻止共享HTTP Cookie(例如AEM `login-token` Cookie)。

以下是SPA生产部署所需的部署配置(当托管在不同于AEM的域上时)。

| SPA连接到 | AEM Author | AEM 发布 | AEM预览 |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher过滤器](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [跨源资源共享(CORS)](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [AEM主机](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

#### 不同域上的SPA部署示例

在此示例中，SPA部署到Netlify域(`https://main--sparkly-marzipan-b20bf8.netlify.app/`)和SPA使用AEM发布域中的AEM GraphQL API (`https://publish-p65804-e666805.adobeaemcloud.com`)。 以下屏幕截图突出显示CORS要求。

1. SPA从Netlify域提供，但会向其他域上的AEM GraphQL API发起XHR调用。 此跨站点请求需要 [CORS](./configurations/cors.md) AEM ，以允许来自Netlify域的请求访问其内容。

   ![从SPA &amp; AEM主机提供的SPA请求 ](assets/spa/cors-requirement.png)

2. 检查对AEM GraphQL API的XHR请求， `Access-Control-Allow-Origin` 存在，指示AEM允许来自此Netlify域的请求访问其内容。

   如果AEM [CORS](./configurations/cors.md) 缺失或不包含Netlify域，Web浏览器会失败XHR请求，并报告CORS错误。

   ![CORS响应标头AEM GraphQL API](assets/spa/cors-response-headers.png)

## 单页应用程序示例

Adobe提供了一个在React中编码的示例单页应用程序。

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
               <p class="is-size-6">一个使用AEM Headless GraphQL API内容的示例单页应用程序，使用React编写。</p>
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
               <p class="is-size-6">以Next.js编写的示例单页应用程序，使用AEM Headless GraphQL API中的内容。</p>
               <a href="../example-apps/next-js.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">查看示例</span>
               </a>
           </div>
       </div>
   </div>
</div>
</div>
