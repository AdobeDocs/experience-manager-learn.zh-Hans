---
title: AEM无头Web组件部署
description: 了解有关Web组件/纯JS的AEM Headless部署的部署注意事项。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10797
thumbnail: kt-10797.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 2%

---


# AEM无头Web组件部署

AEM Headless [Web组件](https://developer.mozilla.org/en-US/docs/Web/Web_Components)/JS部署是纯JavaScript应用程序，在Web浏览器中运行，以无头方式使用AEM中的内容并与其进行交互。 Web组件/JS部署与 [SPA部署](./spa.md) 因为它们不使用强大的SPA框架，并且预期会嵌入到任何网站的上下文中，交付内容以显示来自AEM的内容。


## 部署配置

Web组件/JS部署必须就地配置以下部署配置。

| Web组件/JS应用程序连接到 | AEM Author | AEM 发布 | AEM预览 |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher过滤器](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [跨源资源共享(CORS)](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [AEM主机](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## 示例Web组件

Adobe提供了Web组件示例。

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
                   <p class="is-size-6">使用AEM无头GraphQL API中的内容的纯JavaScript编写的示例Web组件。</p>
                   <a href="../example-apps/web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">查看示例</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>
