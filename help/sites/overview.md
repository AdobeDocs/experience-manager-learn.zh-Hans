---
title: AEM Sites 视频和教程
description: 浏览有关Adobe Experience Manager Sites特性和功能的视频和教程。 AEM Sites是一个领先的体验管理平台。
solution: Experience Manager, Experience Manager Sites
sub-product: Experience Manager Sites
topic: Content Management
doc-type: Catalog
exl-id: cde4ce7f-0afe-4632-8c1c-354586f296d5
source-git-commit: 85472bce41e7d2911bcb15d1ec70be2b094012fc
workflow-type: tm+mt
source-wordcount: '512'
ht-degree: 6%

---

# AEM Sites视频和教程 {#overview}

{{edge-delivery-services}}

Adobe Experience Manager (AEM) Sites是一个领先的体验管理平台。 本用户指南包含有关AEM Sites的众多特性和功能的视频和教程。

## 使用AEM Sites构建的三种方法

AEM Sites提供三种构建、创作和交付体验的方式。 无论您是构建完整页面、优化边缘性能，还是为Headless应用程序提供支持，AEM Sites都提供了灵活的选项来满足您的项目需求：

1. **传统站点**&#x200B;使用AEM作者的页面编辑器来创作内容，这些内容随后会被激活，并作为HTML网页由AEM发布交付给最终用户。
1. **Edge Delivery Services**&#x200B;网站利用基于文档的创作或Adobe Universal Editor创作内容，然后激活内容，再通过Edge Delivery Services作为HTML网页交付给最终用户。
1. **Headless/API-first** Web体验使用内容片段编辑器或通用编辑器创作内容，然后激活这些内容，并由AEM Publish以JSON形式提供。

这些选项旨在满足营销组织的各种需求，以在任何渠道或设备上快速且大规模地提供个性化、引人入胜的体验。

下图说明了不同的路径：

![AEM-Sites-Content-Authoring-and-Experience-Delivery-Paths.png](./assets/aem-sites-authoring-and-experience-delivery-paths.png){width="700" zoomable="yes"}

### 比较使用AEM Sites构建的方法

下表提供了三个路径的高级比较。 它侧重于内容创作和体验交付中每个路径的细微差别。

|            | 传统站点 | Edge Delivery Services | Headless/API优先 |
|---------------------|------------------------------|---------------------------------|---------------------------------------------|
| **创作工具** | 页面编辑器 | 基于文档的创作，通用编辑器 | 内容片段，通用编辑器 |
| **创作内容存储** | AEM创作(JCR) | 文档或AEM作者(JCR) | AEM创作(JCR) |
| **投放** | AEM发布(包含Adobe CDN + Dispatcher) | Edge Delivery Services | AEM发布(包含Adobe CDN + Dispatcher) |
| **投放内容存储** | AEM发布(JCR) | Edge Delivery Services | AEM发布(JCR) |
| **传递格式** | HTML | HTML | JSON |
| **开发技术** | Java™、JavaScript、CSS | JavaScript， CSS | 任意（例如Swift、React等） |

## 教程

请通过以下教程了解使用AEM Sites构建的三条路径中的每一条路径：

<!-- CARDS

* https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview
  {title = Traditional Sites - WKND Tutorial}
  {description = Learn how to build a sample AEM Sites project using the WKND tutorial. This guide walks you through project setup, Core Components, Editable Templates, client-side libraries, and component development.}
  {image = ./assets/aem-wknd-spa-editor-tutorial.png}
  {target = _self}
* https://www.aem.live/docs/
  {title = Edge Delivery Services - Guides}
  {description = Explore Edge Delivery Services with comprehensive guides. The Build, Publish, and Launch guides cover everything you need to get started with EDS.}
  {image = ./assets/edge-delivery-services.png}
  {target = _blank}
* https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/getting-started-with-aem-headless/overview
  {title = Headless/API-First - Tutorials}
  {description = Learn how to build headless applications powered by AEM content. Tutorials cover frameworks like iOS, Android, and React—choose what fits your stack.}
  {image = ./assets/headless.png}
  {target = _self}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Traditional Sites - WKND Tutorial">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" title="传统站点 — WKND教程" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/aem-wknd-spa-editor-tutorial.png" alt="传统站点 — WKND教程"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" target="_self" rel="referrer" title="传统站点 — WKND教程">传统站点 — WKND教程</a>
                    </p>
                    <p class="is-size-6">了解如何使用WKND教程构建示例AEM Sites项目。 本指南将指导您完成项目设置、核心组件、可编辑模板、客户端库和组件开发。</p>
                </div>
                <a href="https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解详情</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Edge Delivery Services - Guides">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://www.aem.live/docs/" title="Edge Delivery Services — 指南" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/edge-delivery-services.png" alt="Edge Delivery Services — 指南"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://www.aem.live/docs/" target="_blank" rel="referrer" title="Edge Delivery Services — 指南">Edge Delivery Services — 指南</a>
                    </p>
                    <p class="is-size-6">使用全面的指南探索Edge Delivery Services。 构建、发布和Launch指南涵盖了您开始使用EDS所需的一切。</p>
                </div>
                <a href="https://www.aem.live/docs/" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解详情</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Headless/API-First - Tutorials">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/getting-started-with-aem-headless/overview" title="Headless/API — 优先 — 教程" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/headless.png" alt="Headless/API — 优先 — 教程"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/getting-started-with-aem-headless/overview" target="_self" rel="referrer" title="Headless/API — 优先 — 教程">Headless/API优先 — 教程</a>
                    </p>
                    <p class="is-size-6">了解如何构建由AEM内容提供支持的Headless应用程序。 教程涵盖iOS、Android™和React等框架 — 选择适合您栈栈的内容。</p>
                </div>
                <a href="https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/getting-started-with-aem-headless/overview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解详情</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


## 其他资源

* [AEM Sites创作文档](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-65/content/sites/authoring/essentials/first-steps)
* [AEM Sites开发文档](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-65/content/implementing/developing/introduction/getting-started)
* [AEM Sites管理文档](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-65/content/sites/administering/home)
* [AEM Sites部署文档](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-65/content/implementing/deploying/introduction/platform)
* [AEM as a Cloud Service教程](/help/cloud-service/overview.md)
* [AEM Assets教程](/help/assets/overview.md)
* [AEM Forms教程](/help/forms/overview.md)
* [AEM Foundation教程](/help/foundation/overview.md)
