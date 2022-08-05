---
title: AEM Headless部署
description: 了解AEM Headless应用程序的各种部署注意事项。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10794
thumbnail: kt-10794.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '315'
ht-degree: 0%

---


# AEM Headless部署

AEM无头客户端部署采用多种形式；AEM托管的SPA、外部SPA、网站、移动设备应用程序，甚至服务器到服务器进程。

根据客户端及其部署方式，AEM Headless部署有不同的注意事项。

## AEM服务架构

在探索部署注意事项之前，必须了解AEM逻辑架构以及AEMas a Cloud Service服务层的分离和角色。 AEMas a Cloud Service由两个逻辑服务组成：

+ __AEM作者__ 是团队可在其中创建、协作和发布内容片段（和其他资产）的服务。
+ __AEM发布__ 是已发布的内容片段（和其他资产）已复制以用于常规使用的服务。
+ __AEM预览__ 是一种模拟AEM发布行为，但将内容发布到该服务以进行预览或查看的服务。 AEM预览适用于内部受众，而不适用于内容的常规交付。 根据所需的工作流，可以选择使用AEM预览。

![AEM服务架构](./assets/overview/aem-service-architecture.png)

典型AEMas a Cloud Service无头部署架构_

以生产容量运行的AEM无头客户端通常会与AEM发布进行交互，该发布包含已批准的已发布内容。 与AEM作者交互的客户端需要特别注意，因为AEM作者默认是安全的，需要对所有请求授权，并且可能包含正在进行的工作或未批准的内容。

## 无外设客户端部署

<div class="columns is-multiline">
    <!-- Single-page App (SPA) -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Single-page App (SPA)" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="./spa.md" title="单页应用程序(SPA)" tabindex="-1">
                       <img class="is-bordered-r-small" src="./assets/spa/spa-card.png" alt="单页应用程序(SPA)">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="./spa.md" title="单页应用程序(SPA)">单页应用程序(SPA)</a></p>
                   <p class="is-size-6">了解单页应用程序(SPA)的部署注意事项。</p>
                   <a href="./spa.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">学习</span>
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
               <a href="./web-component.md" title="Web组件/JS" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/web-component/web-component-card.png" alt="Web组件/JS">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./web-component.md" title="Web组件/JS">Web组件/JS</a></p>
               <p class="is-size-6">了解Web组件和基于浏览器的JavaScript无头用户的部署注意事项。</p>
               <a href="./web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">学习</span>
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
               <a href="./mobile.md" title="移动设备应用程序" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/mobile/mobile-card.png" alt="移动设备应用程序">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./mobile.md" title="移动设备应用程序">移动设备应用程序</a></p>
               <p class="is-size-6">了解移动设备应用程序的部署注意事项。</p>
               <a href="./mobile.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">学习</span>
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
               <a href="./server-to-server.md" title="服务器到服务器应用程序" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/server-to-server/server-to-server-card.png" alt="服务器到服务器应用程序">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./server-to-server.md" title="服务器到服务器应用程序">服务器到服务器应用程序</a></p>
               <p class="is-size-6">了解服务器到服务器应用程序的部署注意事项</p>
               <a href="./server-to-server.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">学习</span>
               </a>
           </div>
       </div>
   </div>
</div>
</div>