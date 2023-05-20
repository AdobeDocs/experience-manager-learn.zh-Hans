---
title: AEM Headless行動部署
description: 瞭解行動AEM Headless部署的部署考量事項。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10796
thumbnail: KT-10796.jpg
exl-id: 1f536079-b3ce-4807-be88-804378e75d37
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 2%

---

# AEM Headless行動部署

AEM Headless行動部署是iOS、Android™等原生行動應用程式。 以Headless方式消費及與AEM中的內容互動。

行動部署需要最少的設定，因為與AEM Headless API的HTTP連線不是在瀏覽器的內容中啟動。

## 部署設定

下列部署設定必須就位以進行行動應用程式部署。

| 行動應用程式連線至 | AEM Author | AEM 发布 | AEM預覽 |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher篩選器](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| 跨原始資源共用(CORS) | ✘ | ✘ | ✘ |
| [AEM主機](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## 行動應用程式範例

Adobe提供iOS和Android™行動應用程式的範例。

<div class="columns is-multiline">
    <!-- iOS app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="iOS app" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/ios-swiftui-app.md" title="iOS應用程式" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/ios-swiftui-app/ios-app-card.png" alt="iOS應用程式">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/ios-swiftui-app.md" title="iOS應用程式">iOS應用程式</a></p>
                   <p class="is-size-6">以SwiftUI撰寫的範例iOS應用程式，會使用AEM Headless GraphQL API的內容。</p>
                   <a href="../example-apps/ios-swiftui-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">檢視範例</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
    <!-- Android app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Android app" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/android-app.md" title="Android™應用程式" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/android-java-app/android-app-card.png" alt="Android應用程式">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/android-app.md" title="Android™應用程式">Android™應用程式</a></p>
                   <p class="is-size-6">使用AEM Headless GraphQL API內容的範例Java™ Android™應用程式。</p>
                   <a href="../example-apps/android-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">檢視範例</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>
