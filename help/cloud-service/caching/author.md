---
title: AEM Author服务缓存
description: AEMas a Cloud Service创作服务缓存的一般概述。
version: Cloud Service
feature: Developer Tools
topic: Performance
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-08-28T00:00:00Z
jira: KT-13858
thumbnail: KT-13858.jpeg
source-git-commit: 6cbd8f3c49d44e75337715c35c198008da8ae7b9
workflow-type: tm+mt
source-wordcount: '319'
ht-degree: 3%

---


# AEM Author

由于AEM Author提供的内容具有高动态性和权限敏感特性，因此其缓存有限。 通常，不建议为AEM Author自定义缓存，而是依赖Adobe提供的缓存配置来确保性能体验。

![AEM创作缓存概述图](./assets/author/author-all.png){align="center"}

我们建议不要在AEM Author上自定义缓存，但是了解AEM Author具有Adobe管理的CDN，但是没有AEM Dispatcher会很有帮助。 请记住，AEM Author上会忽略所有AEM Dispatcher配置，因为它没有AEM Dispatcher。

## CDN

AEM Author服务使用CDN，但其目的是增强产品资源的交付，因此不应对其进行广泛配置，而应使其按原样工作。

![AEM发布缓存概述图](./assets/author/author-cdn.png){align="center"}

AEM Author CDN位于最终用户（通常是营销人员或内容作者）和AEM Author之间。 它会缓存不可变文件(例如支持AEM创作体验的静态资源)，而不是创作内容。

AEM Author的CDN确实缓存了多种可能有用的资源，包括 [持久查询上的可自定义TTL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?author-instances)，和 [自定义客户端库的长TTL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#client-side-libraries).

### 默认缓存期限

以下面向客户的资源由AEM Author CDN缓存，并具有以下默认缓存期限：

| 内容类型 | 默认CDN缓存期限 |
|:------------ |:---------- |
| [持久查询(JSON)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?author-instances) | 1分钟 |
| [客户端库(JS/CSS)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#client-side-libraries) | 30 天 |
| [其他所有条件](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#other-content) | 未缓存 |


## AEM Dispatcher

AEM Author服务不包含AEM Dispatcher，而是仅使用 [CDN](#cdn) 用于缓存。

