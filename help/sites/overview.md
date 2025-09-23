---
title: AEM Sites 视频和教程
description: 浏览有关 Adobe Experience Manager Sites 特点和功能的视频和教程。AEM Sites 是一个领先的体验管理平台。
solution: Experience Manager, Experience Manager Sites
sub-product: Experience Manager Sites
topic: Content Management
doc-type: Catalog
exl-id: cde4ce7f-0afe-4632-8c1c-354586f296d5
source-git-commit: 2b3ff1957f9da313b71a73492777700ddbd79854
workflow-type: tm+mt
source-wordcount: '786'
ht-degree: 38%

---

# AEM Sites 视频和教程 {#overview}

{{edge-delivery-services}}

Adobe Experience Manager (AEM) Sites是Adobe的体验管理平台，通过网站、移动应用程序或任何其他数字渠道，可创作、管理和交付数字体验。

## 使用AEM Sites交付体验的三种方法

AEM Sites 提供了三种构建、创作和投放体验的方式。 无论您是构建网站、优化边缘性能，还是为Headless应用程序提供支持，AEM Sites都提供了灵活的选项来满足您的项目需求：

1. **Edge Delivery Services**&#x200B;体验使用Adobe的Edge Network提供高速、低延迟的内容。 该服务会自动优化使用设备、搜索引擎和GenAI代理的内容。 作者使用Adobe通用编辑器或基于文档的创作来创建内容。
1. **Headless/API-first**&#x200B;体验使用AEM Publish通过HTTP API，以JSON形式为移动应用程序、单页应用程序(SPA)或其他Headless客户端交付内容。 作者使用内容片段编辑器或通用编辑器创建内容。
1. **传统AEM**&#x200B;体验使用AEM Publish作为HTML网页交付内容。 作者使用AEM作者的页面编辑器创建内容。 此选项最适合已迁移的现有项目或项目。

所有这三种选项都是强有力的方法，最佳选择取决于您的用例和组织需求。 每种方法都允许团队在任何渠道或设备上快速且大规模地提供个性化、引人入胜的体验。

>[!IMPORTANT]
>
> **Edge Delivery Services**&#x200B;是使用AEM提供网站的最新和最高级的方法。 它将Adobe Edge Network的速度和可扩展性与现代创作选项相结合。 虽然我们建议将Edge Delivery Services用于新项目，但AEM Sites将继续支持Headless和传统方法，因此您可以选择最适合您需求的路径。

下图描述了使用AEM Sites构建体验的各种选项：

![AEM-Sites-Content-Authoring-and-Experience-Delivery-Paths.png](./assets/aem-sites-authoring-and-experience-delivery-paths.png){width="700" zoomable="yes"}

### 比较使用 AEM Sites 进行构建的方式

下表对三条路径进行了大致比较。它侧重于每条路径在内容创作和体验投放方面的细微差别。

|            | Edge Delivery Services | Headless/API 优先 | 传统 AEM |
|---------------------|------------------------------|---------------------------------|---------------------------------------------|
| **最适合** | 具有高流量、高性能和高可扩展性需求的网站 | 移动应用程序、SPA 和其他无头应用程序 | 现有项目或迁移的项目 |
| **创作工具** | 基于文档的创作、通用编辑器、页面编辑器 | 内容片段，通用编辑器 | 页面编辑器、通用编辑器 |
| **创作内容存储** | 文档或 AEM Author (JCR) | AEM Author (JCR) | AEM Author (JCR) |
| **投放** | Edge Delivery Services | AEM Publish（带 Adobe CDN + Dispatcher） | AEM Publish（带 Adobe CDN + Dispatcher） |
| **投放内容存储** | Edge Delivery Services | AEM Publish (JCR) | AEM Publish (JCR) |
| **传递格式** | HTML | JSON | HTML |
| **开发技术** | JavaScript, CSS | 任何（例如 Swift、React 等） | Java™、HTL、JavaScript、CSS |
| **搜索机器人和GenAI代理支持** | 针对机器人、搜索引擎和GenAI代理进行了优化 | 适用于机器人和代理，但可能需要SSR或其他设置 | 适用于机器人，但性能可能低于Edge Delivery Services |

## 从AMS或内部部署迁移

如果您要从AMS或内部部署(OTP)迁移到AEM as a Cloud Service，Adobe建议您评估直接迁移到Edge Delivery Services的情况。 通常，迁移至AEM as a Cloud Service Publish的工作量并不比迁移更小，与此同时还可提供更快的性能和更好的可扩展性。 如果您认为Edge Delivery Services目前不是您的正确选择，或者如果其他方法更好地满足您的需求，则这些方法仍会为您的项目提供完全受支持的有效选项。

## 教程

更详细地探索使用AEM Sites构建的三种方法。 以下教程将指导您了解每个选项的工作方式、所涉及的工具以及何时使用这些工具。

<!-- CARDS

* https://www.aem.live/docs/
  {title = Edge Delivery Services - Guides}
  {description = Explore Edge Delivery Services with comprehensive guides. The Build, Publish, and Launch guides cover everything you need to get started with Edge Delivery Services.}
  {image = ./assets/edge-delivery-services.png}
  {target = _blank}
* https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/getting-started-with-aem-headless/overview
  {title = Headless/API-First - Tutorials}
  {description = Learn how to build headless applications powered by AEM content. Tutorials cover frameworks like iOS, Android, and React—choose what fits your stack.}
  {image = ./assets/headless.png}
  {target = _self}
* https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview
  {title = Traditional AEM - WKND Tutorial}
  {description = Learn how to build a sample AEM Sites project using the WKND tutorial. This guide walks you through project setup, Core Components, Editable Templates, client-side libraries, and component development.}
  {image = ./assets/aem-wknd-spa-editor-tutorial.png}
  {target = _self}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Edge Delivery Services - Guides">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://www.aem.live/docs/" title="Edge Delivery Services - 指南" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/edge-delivery-services.png" alt="Edge Delivery Services - 指南"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://www.aem.live/docs/" target="_blank" rel="referrer" title="Edge Delivery Services - 指南">Edge Delivery Services - 指南</a>
                    </p>
                    <p class="is-size-6">通过全面指南，探索 Edge Delivery Services。 构建、发布和Launch指南涵盖了开始使用Edge Delivery Services所需的一切。</p>
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
                    <a href="https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/getting-started-with-aem-headless/overview" title="Headless/API 优先 - 教程" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/headless.png" alt="Headless/API 优先 - 教程"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/getting-started-with-aem-headless/overview" target="_self" rel="referrer" title="Headless/API 优先 - 教程">Headless/API 优先 - 教程</a>
                    </p>
                    <p class="is-size-6">了解如何构建由 AEM 内容提供支持的 Headless 应用程序。 教程涵盖 iOS、Android 和 React 等框架——选择适合您技术栈的框架。</p>
                </div>
                <a href="https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/getting-started-with-aem-headless/overview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解详情</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Traditional AEM - WKND Tutorial">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" title="传统 AEM - WKND 教程" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/aem-wknd-spa-editor-tutorial.png" alt="传统 AEM - WKND 教程"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" target="_self" rel="referrer" title="传统 AEM - WKND 教程">传统 AEM - WKND 教程</a>
                    </p>
                    <p class="is-size-6">了解如何使用 WKND 教程构建示例 AEM Sites 项目。 本指南将带您了解项目设置、核心组件、可编辑模板、客户端库以及组件开发。</p>
                </div>
                <a href="https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解详情</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


## 其他资源

* [AEM Sites 创作文档](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-65/content/sites/authoring/essentials/first-steps)
* [AEM Sites 开发文档](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-65/content/implementing/developing/introduction/getting-started)
* [AEM Sites 管理文档](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-65/content/sites/administering/home)
* [AEM Sites 部署文档](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-65/content/implementing/deploying/introduction/platform)
* [AEM as a Cloud Service 教程](/help/cloud-service/overview.md)
* [AEM Assets 教程](/help/assets/overview.md)
* [AEM Forms 教程](/help/forms/overview.md)
* [AEM Foundation 教程](/help/foundation/overview.md)
