---
title: AEM Headless伺服器對伺服器部署
description: 瞭解伺服器對伺服器AEM Headless部署的部署考量事項。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10798
thumbnail: kt-10798.jpg
exl-id: d4ae08d9-dc43-4414-ab75-26853186a301
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 1%

---

# AEM Headless伺服器對伺服器部署

AEM Headless伺服器對伺服器部署涉及伺服器端應用程式或程式，這些應用程式或程式會以Headless方式使用並與AEM中的內容互動。

伺服器對伺服器的部署需要最少的設定，因為與AEM Headless API的HTTP連線不會在瀏覽器的內容中啟動。

## 部署設定

伺服器對伺服器應用程式部署必須有以下部署設定。

| 伺服器對伺服器應用程式連線至 | AEM Author | AEM 发布 | AEM預覽 |
|---------------------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher篩選器](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| 跨原始資源共用(CORS) | ✘ | ✘ | ✘ |
| [AEM主機](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## 授權需求

對AEM GraphQL API的授權請求通常會發生在伺服器對伺服器應用程式的內容中，因為其他應用程式型別，例如 [單頁應用程式](./spa.md)， [行動裝置](./mobile.md)，或 [Web元件](./web-component.md)通常使用授權，因為很難保護認證。

將請求授權給AEMas a Cloud Service時，請使用 [服務認證型權杖驗證](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html). 若要進一步瞭解向AEMas a Cloud Service驗證請求，請檢閱 [權杖型驗證教學課程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html). 本教學課程會探索使用下列專案進行權杖型驗證： [AEM ASSETS HTTP API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets.html) 但這些概念和方法同樣適用於與AEM Headless GraphQL API互動的應用程式。

## 範例伺服器對伺服器應用程式

Adobe提供Node.js中編碼的伺服器對伺服器應用程式範例。

<div class="columns is-multiline">
    <!-- Server-to-server app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Server-to-server app" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/server-to-server-app.md" title="伺服器對伺服器應用程式" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/server-to-server-app/server-to-server-card.png" alt="伺服器對伺服器應用程式">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/server-to-server-app.md" title="伺服器對伺服器應用程式">伺服器對伺服器應用程式</a></p>
                   <p class="is-size-6">以Node.js撰寫的範例伺服器對伺服器應用程式，會使用AEM Headless GraphQL API的內容。</p>
                   <a href="../example-apps/server-to-server-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">檢視範例</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>
