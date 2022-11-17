---
title: URL重定向
description: 了解在AEM中执行URL重定向的各种选项。
version: 6.4, 6.5, Cloud Service
topic: Development, Administration
feature: Operations, Dispatcher
role: Developer, Architect
level: Intermediate
kt: 11466
last-substantial-update: 2022-10-14T00:00:00Z
index: y
source-git-commit: d5645e975aa290392348cc69d078b24921a7d13a
workflow-type: tm+mt
source-wordcount: '816'
ht-degree: 1%

---


# URL重定向

在网站操作中，URL重定向是常见的方面。 架构师和管理员面临着这样的难题：如何以及在何处管理URL重定向，以找到最佳解决方案，从而提供灵活性和快速的重定向部署时间。

确保您熟悉 [AEM 6.x)](https://experienceleague.adobe.com/docs/experience-manager-learn/dispatcher-tutorial/chapter-2.html#the-%E2%80%9Clegacy%E2%80%9D-setup) 和 [AEMas a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/architecture.html#runtime-architecture) 基础设施。 主要区别是：

1.  AEMas a Cloud Service [内置CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html)但是，客户可以在AEM管理的CDN之前提供CDN(BYOCDN)。
1.  AEM 6.x(内部部署版或Adobe Managed Services(AMS)是否不包含AEM-managed CDN，并且客户必须自行构建。

其他AEM服务（AEM创作/发布和调度程序）在概念上与AEM 6.x和AEMas a Cloud Service相似。

AEM URL重定向解决方案如下所示：

|  | 作为AEM项目代码进行管理和部署 | 能够按营销/内容团队进行更改 | AEM as Cloud Service兼容 | 执行重定向 |
|---------------------------------------------------|:-----------------------:|:---------------------:|:---------------------:| :---------------------:|
| [通过为您自带的CDN在Edge](#at-edge-via-bring-your-own-cdn) | ✘ | ✘ | ✔ | Edge/CDN |
| [Apache `mod_rewrite` 规则作为Dispatcher配置 ](#apache-mod_rewrite-module) | ✔ | ✘ | ✔ | Dispatcher |
| [ACS Commons — 重定向映射管理器](#redirect-map-manager) | ✘ | ✔ | ✘ | Dispatcher |
| [ACS Commons — 重定向管理器](#redirect-manager) | ✘ | ✔ | ✔ | AEM |


## 解决方案选项

以下是一些解决方案选项，其顺序是离网站访客的浏览器较近。

### At Edge通过为您自己的CDN提供

某些CDN服务在边缘级别提供重定向解决方案，从而减少往返原点的次数。 请参阅 [Akamai Edge重定向器](https://techdocs.akamai.com/cloudlets/docs/what-edge-redirector), [AWS CloudFront函数](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-functions.html). 请咨询您的CDN服务提供商，以获取边缘级别重定向功能。

在Edge或CDN级别管理重定向具有性能优势，但它们不是作为AEM的一部分进行管理，而是作为独立项目进行管理。 管理和部署重定向规则的深思熟虑的过程对于避免问题至关重要。


### Apache `mod_rewrite` 模块

常见的解决方案使用 [Apache模块mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html). 的 [AEM项目原型](https://github.com/adobe/aem-project-archetype) 为 [AEM 6.x](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.ams#file-structure) 和 [AEMas a Cloud Service](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud#file-structure) 项目。 默认（不可变）和自定义重写规则在 `conf.d/rewrites` 文件夹和重写引擎已为打开 `virtualhosts` 监听端口 `80` 通过 `conf.d/dispatcher_vhost.conf` 文件。 在 [AEM WKND Sites项目](https://github.com/adobe/aem-guides-wknd/tree/main/dispatcher/src/conf.d/rewrites).

在AEMas a Cloud Service中，这些重定向规则作为AEM代码的一部分进行管理，并通过Cloud Manager进行部署 [网层配置管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html#web-tier-config-pipelines) 或 [全栈管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html#full-stack-pipeline). 因此，您的AEM特定于项目的流程可用于管理、部署和跟踪重定向规则。

大多数CDN服务都会根据其 `Cache-Control` 或 `Expires` 标题。 这有助于避免在初始重定向源自Apache/Dispatcher后出现往返问题。


### ACS AEM Commons

中提供了两项功能 [ACS AEM Commons](https://adobe-consulting-services.github.io/acs-aem-commons/) 以管理URL重定向。 请注意，ACS AEM Commons是由社区运营的开源项目，不受Adobe支持。

#### 重定向映射管理器

[重定向映射管理器](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-map-manager/index.html) 允许AEM 6.x管理员轻松维护和发布 [Apache RewriteMap](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html) 文件，而无需直接访问Apache Web服务器，也无需重新启动Apache Web服务器。 此功能允许用户在AEM的控制台中创建、更新和删除重定向规则，而无需开发团队或AEM部署的帮助。 重定向映射管理器是 **不兼容AEMas a Cloud Service**.

#### 重定向管理器

[重定向管理器](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html) 允许AEM中的用户轻松地从AEM维护和发布重定向。 该实现基于Java™ Servlet过滤器，因此具有典型的JVM资源消耗。 此功能还消除了对AEM开发团队和AEM部署的依赖。 重定向管理器同时 **AEMas a Cloud Service** 和 **AEM 6.x** 兼容。 请注意，虽然初始重定向请求必须由其AEM发布服务才能生成301/302，但（大多数）CDN默认情况下会缓存301/302，从而允许后续请求在边缘/CDN进行重定向。


## 哪种解决方案适合实施

以下是确定正确解决方案的几个标准。 此外，贵组织的IT和营销流程应有助于选择正确的解决方案。

1. 使营销团队或超级用户能够在无需AEM开发团队和AEM发布、部署周期的情况下管理重定向规则。
1. 用于验证、跟踪和还原更改或风险缓解的流程。
1. 可用性 _主题专业知识_ 表示 **通过CDN服务在边缘** 解决方案。

