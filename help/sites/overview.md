---
title: AEM Sites 视频和教程
description: 浏览有关 Adobe Experience Manager Sites 特点和功能的视频和教程。AEM Sites 是一个领先的体验管理平台。
solution: Experience Manager, Experience Manager Sites
sub-product: Experience Manager Sites
topic: Content Management
doc-type: Catalog
exl-id: cde4ce7f-0afe-4632-8c1c-354586f296d5
source-git-commit: 999bbe542e5c71ae537f93a4c89acf6d304a4292
workflow-type: ht
source-wordcount: '786'
ht-degree: 100%

---

# AEM Sites 视频和教程 {#overview}

{{edge-delivery-services}}

Adobe Experience Manager (AEM) Sites 是 Adobe 的体验管理平台，无论是通过网站、移动应用程序还是任何其他数字渠道，都可创作、管理和传递数字体验。

## 使用 AEM Sites 传递体验的三种方式

AEM Sites 提供了三种构建、创作和传递体验的方式。无论您要构建网站、优化边缘性能，还是为 Headless 应用程序提供支持，AEM Sites 都能提供灵活的选项来满足您的项目需求：

1. **Edge Delivery Services** 体验使用 Adobe 的 Edge Network 以高速、低延迟的方式传递内容。该服务会为使用设备、搜索引擎和生成式 AI 代理自动优化内容。作者使用 Adobe 通用编辑器或基于文档的创作方法来创建内容。
1. **Headless/API-first** 体验使用 AEM Publish 通过 HTTP API 为移动应用程序、单页应用程序 (SPA) 或其他无头客户端传递 JSON 形式的内容。作者使用内容片段编辑器或通用编辑器来创建内容。
1. **传统 AEM** 体验通过 AEM Publish 传递 HTML 网页形式的内容。作者使用 AEM Author 的页面编辑器来创建内容。此选项最适合现有项目或已迁移的项目。

这三种选项全都是非常强大的方法，选择哪一种取决于您的用例和组织需求。每种方法都允许团队在任何渠道或设备上都能大规模地快速提供极具吸引力的个性化的体验。

>[!IMPORTANT]
>
> **Edge Delivery Services** 是通过 AEM 提供网站的最新和最高级的方法。它将 Adobe Edge Network 的速度和可扩展性与各种现代创作选项相结合。Edge Delivery Services 适合用于新项目，而 AEM Sites 将继续支持传统无头方法，您可以选择最适合您需求的路径。

下图描述了使用 AEM Sites 构建体验的各种不同的选项：

![AEM-Sites-Content-Authoring-and-Experience-Delivery-Paths.png](./assets/aem-sites-authoring-and-experience-delivery-paths.png){width="700" zoomable="yes"}

### 比较使用 AEM Sites 进行构建的方式

下表对三条路径进行了大致比较。它侧重于每条路径在内容创作和体验投放方面的细微差别。

|            | Edge Delivery Services | Headless/API 优先 | 传统 AEM |
|---------------------|------------------------------|---------------------------------|---------------------------------------------|
| **最适合** | 具有高流量、高性能和高可扩展性需求的网站 | 移动应用程序、SPA 和其他无头应用程序 | 现有项目或已迁移的项目 |
| **创作工具** | 基于文档的创作、通用编辑器、页面编辑器 | 内容片段，通用编辑器 | 页面编辑器，通用编辑器 |
| **创作内容存储** | 文档或 AEM Author (JCR) | AEM Author (JCR) | AEM Author (JCR) |
| **投放** | Edge Delivery Services | AEM Publish（带 Adobe CDN + Dispatcher） | AEM Publish（带 Adobe CDN + Dispatcher） |
| **传递内容存储** | Edge Delivery Services | AEM Publish (JCR) | AEM Publish (JCR) |
| **传递格式** | HTML | JSON | HTML |
| **开发技术** | JavaScript, CSS | 任何（例如 Swift、React 等） | Java™, HTL, JavaScript, CSS |
| **支持搜索机器人和生成式 AI 代理** | 为机器人、搜索引擎和生成式 AI 代理进行了优化 | 适用于机器人和代理，但可能需要 SSR 或额外设置 | 适合机器人，但性能可能比 Edge Delivery Services 低 |

## 从 AMS 或内部部署迁移

如果您要从 AMS 或内部部署 (OTP) 迁移到 AEM as a Cloud Service，Adobe 建议您评估直接迁移到 Edge Delivery Services 的可能性。通常，这个工作量并不比迁移到 AEM as a Cloud Service Publish 更大，同时还能提供更快的性能和更大的可扩展性。如果您认为 Edge Delivery Services 目前对您来说不是适当选择，或者如果其他方法能更好地满足您的需求，这些方法会完全受支持，仍是您项目的有效选项。

## 教程

更详细地了解使用 AEM Sites 构建的三种方法。以下教程将指导您了解每个选项的工作方法、所涉及的工具以及何时使用这些工具。

<!-- CARDS

* https://www.aem.live/docs/
  {title = Edge Delivery Services - Guides}
  {description = Explore Edge Delivery Services with comprehensive guides. The Build, Publish, and Launch guides cover everything you need to get started with Edge Delivery Services.}
  {image = ./assets/edge-delivery-services.png}
  {target = _blank}
* https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/overview
  {title = Headless/API-First - Tutorials}
  {description = Learn how to build headless applications powered by AEM content. Tutorials cover frameworks like iOS, Android, and React—choose what fits your stack.}
  {image = ./assets/headless.png}
  {target = _self}
* https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview
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
                    <p class="is-size-6">通过全面指南，探索 Edge Delivery Services。 构建、发布和启动指南涵盖了您开始使用 Edge Delivery Services 所需的所有内容。</p>
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
