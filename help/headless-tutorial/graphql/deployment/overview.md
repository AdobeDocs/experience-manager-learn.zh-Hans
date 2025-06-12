---
title: AEM Headless 部署
description: 了解 AEM Headless 应用的各种部署注意事项。
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10794
thumbnail: kt-10794.jpg
last-substantial-update: 2022-08-26T00:00:00Z
exl-id: 6de58ca0-9444-4272-9487-15a9e3c89231
duration: 59
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '315'
ht-degree: 100%

---

# AEM Headless 部署

AEM Headless 客户端部署形式多样，包括 AEM 托管的 SPA、外部 SPA、网站、移动应用，甚至服务器间的进程。

根据客户端类型及其部署方式，AEM Headless 部署需要考虑不同的因素。

## AEM 服务架构

在探讨部署注意事项之前，必须先了解 AEM 的逻辑架构，以及 AEM as a Cloud Service 各服务层的划分与职责。AEM as a Cloud Service 由两个逻辑服务组成：

+ __AEM Author__ 是团队创建、协作和发布内容片段（及其他资产）的服务。
+ __AEM Publish__  是已发布的内容片段（及其他资产）被复制以供公众使用的服务。
+ __AEM Preview__ 是模拟 AEM Publish 行为的服务，相关内容会发布到该服务以供预览或审核使用。AEM Preview 仅供内部受众使用，不适用于面向大众的内容传递。AEM Preview 的使用是可选的，具体取决于所需的工作流程。

![AEM 服务架构](./assets/overview/aem-service-architecture.png)

典型的 AEM as a Cloud Service 的 Headless 部署架构

在生产环境中运行的 AEM Headless 客户端通常与包含已批准、已发布内容的 AEM Publish 进行交互。与 AEM Author 交互的客户端需特别注意，因 AEM Author 默认具备安全保护，所有请求均需授权，同时可能包含正在编辑中的内容或未批准的内容。

## Headless 客户端部署

<div class="columns is-multiline">
    <!-- Single-page App (SPA) -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Single-page App (SPA)" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="./spa.md" title="单页应用程序 (SPA)" tabindex="-1">
                       <img class="is-bordered-r-small" src="./assets/spa/spa-card.png" alt="单页应用程序 (SPA)">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="./spa.md" title="单页应用程序 (SPA)">单页应用程序 (SPA)</a></p>
                   <p class="is-size-6">了解单页应用程序 (SPA) 的部署注意事项。</p>
                   <a href="./spa.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解</span>
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
               <a href="./web-component.md" title="Web 组件/JS" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/web-component/web-component-card.png" alt="Web 组件/JS">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./web-component.md" title="Web 组件/JS">Web 组件/JS</a></p>
               <p class="is-size-6">了解 Web 组件和基于浏览器的 JavaScript Headless 客户端的部署注意事项。</p>
               <a href="./web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解</span>
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
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解</span>
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
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解</span>
               </a>
           </div>
       </div>
   </div>
</div>
</div>
