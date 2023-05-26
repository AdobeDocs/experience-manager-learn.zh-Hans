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
exl-id: 8e64f251-e5fd-4add-880e-9d54f8e501a6
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '876'
ht-degree: 1%

---

# URL重定向

URL重定向是网站操作中一个常见的方面。 架构师和管理员需要找到最佳解决方案，了解如何以及在何处管理URL重定向，从而提供灵活性和快速的重定向部署时间。

确保您熟悉 [AEM (6.x)又称AEM Classic](https://experienceleague.adobe.com/docs/experience-manager-learn/dispatcher-tutorial/chapter-2.html#the-%E2%80%9Clegacy%E2%80%9D-setup) 和 [AEMas a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/architecture.html#runtime-architecture) 基础架构。 主要区别包括：

1. AEMas a Cloud Service具有 [内置CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html)但是，客户可以在AEM管理的CDN之前提供CDN (BYOCDN)。
1. AEM 6.x(无论是内部部署还是Adobe Managed Services (AMS))均不包含AEM管理的CDN，并且客户必须自带。

其他AEM服务（AEM Author/Publish和Dispatcher）在AEM 6.x和AEMas a Cloud Service之间在概念上是相似的。

AEM URL重定向解决方案如下所示：

|  | 作为AEM项目代码托管和部署 | 能够按营销/内容团队进行更改 | AEM与Cloud Service兼容 | 发生重定向执行的位置 |
|---------------------------------------------------|:-----------------------:|:---------------------:|:---------------------:| :---------------------:|
| [在Edge，通过自带CDN](#at-edge-via-bring-your-own-cdn) | ✘ | ✘ | ✔ | Edge/CDN |
| [Apache `mod_rewrite` 规则作为Dispatcher配置 ](#apache-mod_rewrite-module) | ✔ | ✘ | ✔ | Dispatcher |
| [ACS Commons — 重定向映射管理器](#redirect-map-manager) | ✘ | ✔ | ✘ | Dispatcher |
| [ACS Commons — 重定向管理器](#redirect-manager) | ✘ | ✔ | ✔ | AEM |
| [此 `Redirect` 页面属性](#the-redirect-page-property) | ✘ | ✔ | ✔ | AEM |


## 解决方案选项

以下是从离网站访客的浏览器较近的顺序排列的解决方案选项。

### 在Edge，通过自带CDN

某些CDN服务提供边缘级别的重定向解决方案，从而减少到原点的往返次数。 参见 [Akamai Edge重定向器](https://techdocs.akamai.com/cloudlets/docs/what-edge-redirector)， [AWS CloudFront函数](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-functions.html). 有关边缘级别重定向功能，请咨询您的CDN服务提供商。

在Edge或CDN级别管理重定向具有性能优势，但是它们不是作为AEM的一部分进行管理，而是作为离散项目进行管理。 管理和部署重定向规则的深思熟虑过程对于避免问题至关重要。


### Apache `mod_rewrite` 模块

一种常见的解决方案使用 [Apache模块mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html). 此 [AEM项目原型](https://github.com/adobe/aem-project-archetype) 为两者都提供Dispatcher项目结构 [AEM 6.x](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.ams#file-structure) 和 [AEMas a Cloud Service](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud#file-structure) 项目。 默认（不可变）和自定义重写规则定义于 `conf.d/rewrites` 文件夹打开了重写引擎 `virtualhosts` 监听端口 `80` via `conf.d/dispatcher_vhost.conf` 文件。 示例实施位于 [AEM WKND站点项目](https://github.com/adobe/aem-guides-wknd/tree/main/dispatcher/src/conf.d/rewrites).

在AEMas a Cloud Service中，这些重定向规则作为AEM代码的一部分进行管理，并通过Cloud Manager进行部署 [Web层配置管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html#web-tier-config-pipelines) 或 [全栈管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html#full-stack-pipeline). 因此，您的AEM项目特定流程将用于管理、部署和跟踪重定向规则。

大多数CDN服务都会缓存HTTP 301和302重定向，具体取决于它们的 `Cache-Control` 或 `Expires` 标头。 这有助于避免在Apache/Dispatcher处发起初始重定向后的来回。


### ACS AEM Commons

中提供了两项功能 [ACS AEM Commons](https://adobe-consulting-services.github.io/acs-aem-commons/) 管理URL重定向。 请注意，ACS AEM Commons是一个社区运营的开源项目，不受Adobe支持。

#### 重定向映射管理器

[重定向映射管理器](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-map-manager/index.html) 使AEM 6.x管理员能够轻松维护和发布 [Apache RewriteMap](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html) 文件无需直接访问Apache Web Server或要求重新启动Apache Web Server。 此功能允许用户从AEM中的控制台创建、更新和删除重定向规则，而无需开发团队或AEM部署。 重定向映射管理器为 **与AEMas a Cloud Service不兼容**.

#### 重定向管理器

[重定向管理器](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html) 允许AEM中的用户轻松地维护和发布AEM中的重定向。 该实现基于Java™ servlet过滤器，因而典型的JVM资源消耗。 此功能还可消除对AEM开发团队和AEM部署的依赖性。 重定向管理器是两者 **AEMas a Cloud Service** 和 **AEM 6.x** 兼容。 默认情况下，初始重定向请求必须命中AEM Publish服务以生成301/302（大多数） CDN的缓存301/302，从而允许后续请求重定向到edge/CDN。

### 此 `Redirect` 页面属性

开箱即用(OOTB) `Redirect` 页面属性 [“高级”选项卡](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/fundamentals/page-properties.html#advanced) 允许内容作者定义当前页面的重定向位置。 此解决方案最适合每页面重定向方案，并且没有用于查看和管理页面重定向的中心位置。

## 哪个解决方案适合实施

下面是确定正确解决方案的一些标准。 此外，贵组织的IT和营销流程应该有助于选择正确的解决方案。

1. 使营销团队或超级用户能够在没有AEM开发团队和AEM部署的情况下管理重定向规则。
1. 管理、验证、跟踪和恢复更改或降低风险的过程。
1. 可用性 _主题专业知识_ 对象 **At Edge（通过CDN服务）** 解决方案。
