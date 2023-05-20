---
title: AEM Headless Web元件部署
description: 瞭解網頁元件/純JS型AEM Headless部署的部署考量事項。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10797
thumbnail: kt-10797.jpg
exl-id: 9d4aab4c-82af-4917-8c1b-3935f19691e6
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 2%

---

# AEM Headless Web元件部署

AEM Headless [網頁元件](https://developer.mozilla.org/en-US/docs/Web/Web_Components)/JS部署是在網頁瀏覽器中執行的純JavaScript應用程式，以Headless方式使用及與AEM中的內容互動。 Web元件/JS部署與 [SPA部署](./spa.md) 因為它們不使用強大的SPA架構，且預期會嵌入到任何網站的內容中，以傳遞來自AEM的內容。


## 部署設定

下列部署設定必須就Web元件/JS部署。

| 連線至的Web元件/JS應用程式 | AEM Author | AEM 发布 | AEM預覽 |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher篩選器](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [跨原始資源共用(CORS)](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [AEM主機](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## 範例Web元件

Adobe提供範例Web元件。

<div class="columns is-multiline">
    <!-- Web Component -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Web Component" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/web-component.md" title="網頁元件" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/web-component/web-component-card.png" alt="網頁元件">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/web-component.md" title="網頁元件">網頁元件</a></p>
                   <p class="is-size-6">以純JavaScript撰寫的範例Web元件會使用AEM Headless GraphQL API的內容。</p>
                   <a href="../example-apps/web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">檢視範例</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>
