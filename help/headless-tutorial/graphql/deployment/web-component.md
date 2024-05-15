---
title: AEM Headless Web组件部署
description: 了解基于Web组件/纯JS的AEM Headless部署的部署注意事项。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10797
thumbnail: kt-10797.jpg
exl-id: 9d4aab4c-82af-4917-8c1b-3935f19691e6
duration: 31
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '164'
ht-degree: 2%

---

# AEM Headless Web组件部署

AEM Headless [Web组件](https://developer.mozilla.org/en-US/docs/Web/Web_Components)/JS部署是在Web浏览器中运行的纯JavaScript应用程序，它们以Headless方式在AEM中使用内容并与之交互。 Web组件/JS部署与 [SPA部署](./spa.md) 因为它们不使用强大的SPA框架，并且预期嵌入在任何网站上下文中，交付，以从AEM呈现内容。


## 部署配置

必须为Web组件/JS部署就地以下部署配置。

| 连接到的Web组件/JS应用程序 | AEM Author | AEM 发布 | AEM预览 |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher过滤器](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [跨源资源共享(CORS)](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [AEM主机](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## 示例Web组件

Adobe提供了一个示例Web组件。

<div class="columns is-multiline">
    <!-- Web Component -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Web Component" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/web-component.md" title="Web组件" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/web-component/web-component-card.png" alt="Web组件">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/web-component.md" title="Web组件">Web组件</a></p>
                   <p class="is-size-6">以纯JavaScript编写的示例Web组件使用AEM Headless GraphQL API中的内容。</p>
                   <a href="../example-apps/web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">查看示例</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>
