---
title: AEM Headless部署
description: 瞭解AEM Headless應用程式的各種部署考量事項。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10794
thumbnail: kt-10794.jpg
last-substantial-update: 2022-08-26T00:00:00Z
exl-id: 6de58ca0-9444-4272-9487-15a9e3c89231
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '315'
ht-degree: 0%

---

# AEM Headless部署

AEM Headless使用者端部署有多種形式；AEM託管的SPA、外部SPA、網站、行動應用程式，甚至伺服器對伺服器程式。

根據使用者端和部署方式，AEM Headless部署有不同的考量因素。

## AEM服務架構

在探索部署考量之前，您必須瞭解AEM邏輯架構，以及AEMas a Cloud Service服務層的分離和角色。 AEMas a Cloud Service由兩個邏輯服務組成：

+ __AEM作者__ 是團隊建立、共同作業和發佈內容片段（和其他資產）的服務。
+ __AEM發佈__ 是已發佈的內容片段（和其他資產）已復寫以供一般使用的服務。
+ __AEM預覽__ 此服務會模擬AEM Publish的行為，但發佈內容給它，以供預覽或檢閱。 AEM預覽適用於內部對象，而非一般內容傳送。 根據所需的工作流程，AEM預覽的使用是選用的。

![AEM服務架構](./assets/overview/aem-service-architecture.png)

典型AEMas a Cloud Service無周邊部署架構_

以生產產能作業的AEM Headless使用者端通常會與AEM Publish （其中包含已核准的已發佈內容）互動。 與AEM Author互動的使用者端需要特別小心，因為AEM Author預設是安全的，所有請求都需要授權，並且可能還包含進行中的工作或未核准的內容。

## Headless客戶端部署

<div class="columns is-multiline">
    <!-- Single-page App (SPA) -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Single-page App (SPA)" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="./spa.md" title="單頁應用程式(SPA)" tabindex="-1">
                       <img class="is-bordered-r-small" src="./assets/spa/spa-card.png" alt="單頁應用程式(SPA)">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="./spa.md" title="單頁應用程式(SPA)">單頁應用程式(SPA)</a></p>
                   <p class="is-size-6">瞭解單頁應用程式(SPA)的部署考量事項。</p>
                   <a href="./spa.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">瞭解</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
<!-- Web component/JS -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Web component/JS" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="./web-component.md" title="Web元件/JS" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/web-component/web-component-card.png" alt="Web元件/JS">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./web-component.md" title="Web元件/JS">Web元件/JS</a></p>
               <p class="is-size-6">瞭解Web元件和瀏覽器型JavaScript Headless使用者的部署考量事項。</p>
               <a href="./web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">瞭解</span>
               </a>
           </div>
       </div>
   </div>
</div>
<!-- Mobile apps -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Mobile apps" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="./mobile.md" title="行動應用程式" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/mobile/mobile-card.png" alt="行動應用程式">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./mobile.md" title="行動應用程式">行動應用程式</a></p>
               <p class="is-size-6">瞭解行動應用程式的部署考量事項。</p>
               <a href="./mobile.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">瞭解</span>
               </a>
           </div>
       </div>
   </div>
</div>
<!-- Server-to-server apps -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Server-to-server apps" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="./server-to-server.md" title="伺服器對伺服器應用程式" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/server-to-server/server-to-server-card.png" alt="伺服器對伺服器應用程式">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./server-to-server.md" title="伺服器對伺服器應用程式">伺服器對伺服器應用程式</a></p>
               <p class="is-size-6">瞭解伺服器對伺服器應用程式的部署考量</p>
               <a href="./server-to-server.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">瞭解</span>
               </a>
           </div>
       </div>
   </div>
</div>
</div>
