---
title: URL重定向
description: 了解在AEM中执行URL重定向的各种选项。
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Development, Administration
feature: Operations, Dispatcher
role: Developer
level: Intermediate
jira: KT-11466
last-substantial-update: 2024-10-22T00:00:00Z
index: y
doc-type: Article
exl-id: 8e64f251-e5fd-4add-880e-9d54f8e501a6
duration: 164
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '943'
ht-degree: 0%

---

# URL重定向

URL重定向是网站操作中一个常见的方面。 架构师和管理员需要找到最佳解决方案，了解如何以及在何处管理URL重定向，从而提供灵活性和快速的重定向部署时间。

确保您熟悉[AEM (6.x)，即AEM Classic](https://experienceleague.adobe.com/en/docs/experience-manager-learn/dispatcher-tutorial/chapter-2)和[AEM as a Cloud Service](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/overview/architecture)基础架构。 主要区别包括：

1. AEM as a Cloud Service具有[内置CDN](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn)，但是，客户可以在AEM管理的CDN之前提供CDN (BYOCDN)。
1. AEM 6.x(无论是内部部署还是Adobe Managed Services (AMS))均不包含AEM管理的CDN，并且客户必须自带。

其他AEM服务(AEM Author/Publish和Dispatcher)在AEM 6.x和AEM as a Cloud Service之间的概念其他方面类似。

AEM的URL重定向解决方案如下所示：

|                                                   | 作为AEM项目代码托管和部署 | 能够按营销/内容团队进行更改 | AEM as Cloud Service兼容 | 执行重定向的位置 |
|---------------------------------------------------|:-----------------------:|:---------------------:|:---------------------:| :---------------------:|
| [在Edge，通过AEM-managed CDN](#at-edge-via-aem-managed-cdn) | ✔ | ✘ | ✔ | Edge/CDN（内置） |
| [在Edge，通过自带CDN (BYOCDN)](#at-edge-via-bring-your-own-cdn) | ✘ | ✘ | ✔ | Edge/CDN (BYOCDN) |
| [Apache `mod_rewrite`规则作为Dispatcher配置](#apache-mod_rewrite-module) | ✔ | ✘ | ✔ | Dispatcher |
| [ACS Commons — 重定向映射管理器](#redirect-map-manager) | ✘ | ✔ | ✔ | Dispatcher |
| [ACS Commons — 重定向管理器](#redirect-manager) | ✘ | ✔ | ✔ | AEM / DISPATCHER |
| [`Redirect`页面属性](#the-redirect-page-property) | ✘ | ✔ | ✔ | AEM |


## 解决方案选项

以下是从离网站访客的浏览器较近的顺序来看的一些解决方案选项。

### 在Edge，通过AEM管理的CDN {#at-edge-via-aem-managed-cdn}

此选项仅适用于AEM as a Cloud Service客户。

[AEM-managed CDN](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn)提供了Edge级别的重定向解决方案，从而减少了到原点的往返次数。 [服务器端重定向](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#server-side-redirectors)功能允许您在AEM项目代码中配置重定向规则，并使用[配置管道](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager)进行部署。 CDN配置文件(`cdn.yaml`)大小不应超过100KB。

在Edge或CDN级别管理重定向具有性能优势。

### 在Edge，通过自带CDN

某些CDN服务提供了Edge级别的重定向解决方案，因此减少了到原点的往返次数。 请参阅[Akamai Edge重定向器](https://techdocs.akamai.com/cloudlets/docs/what-edge-redirector)、[AWS CloudFront函数](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-functions.html)。 有关Edge级别重定向功能，请咨询您的CDN服务提供商。

在Edge或CDN级别管理重定向具有性能优势，但是它们不是作为AEM的一部分进行管理，而是作为离散项目进行管理。 一个明确定义的流程来管理和部署重定向规则对于避免问题至关重要。


### Apache `mod_rewrite`模块

通用解决方案使用[Apache模块mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html)。 [AEM项目原型](https://github.com/adobe/aem-project-archetype)为[Dispatcher 6.x](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.ams#file-structure)和[AEM as a Cloud Service](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud#file-structure)项目提供了AEM项目结构。 在`conf.d/rewrites`文件夹中定义了默认（不可变）和自定义重写规则，并且为通过`virtualhosts`文件侦听端口`80`的`conf.d/dispatcher_vhost.conf`打开重写引擎。 [AEM WKND Sites项目](https://github.com/adobe/aem-guides-wknd/tree/main/dispatcher/src/conf.d/rewrites)中提供了示例实现。

在AEM as a Cloud Service中，这些重定向规则作为AEM代码的一部分进行管理，并通过Cloud Manager [Web层配置管道](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines)或[全栈管道](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines)进行部署。 因此，您的AEM项目特定流程可用于管理、部署和跟踪重定向规则。

大多数CDN服务会根据其`Cache-Control`或`Expires`标头缓存HTTP 301和302重定向。 它有助于避免在Apache/Dispatcher上发起初始重定向后出现往返情况。


### ACS AEM共享资源

[ACS AEM Commons](https://adobe-consulting-services.github.io/acs-aem-commons/)中有两项功能可用于管理URL重定向。 请注意，ACS AEM Commons是一个社区运营的开源项目，不受Adobe支持。

#### 重定向映射管理器

[重定向映射管理器](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-map-manager/index.html)可帮助AEM管理员轻松维护和发布[Apache RewriteMap](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html)文件，而无需直接访问Apache Web Server或要求重新启动Apache Web Server。 此功能允许用户从AEM中的控制台创建、更新和删除重定向规则，而无需开发团队或AEM部署。 重定向映射管理器是&#x200B;**AEM as a Cloud Service**（请参阅[无管道URL重定向](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/pipeline-free-url-redirects)策略和相关的[教程](https://experienceleague.adobe.com/en/docs/experience-manager-learn/foundation/administration/implementing-pipeline-free-url-redirects#acs-commons---redirect-map-manager)）并且与&#x200B;**AEM 6.x**&#x200B;兼容。

#### 重定向管理器

[重定向管理器](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html)允许AEM中的用户轻松地维护和发布来自AEM的重定向。 该实现基于Java™ servlet过滤器，因此典型的JVM资源消耗。 此功能还消除了对于AEM开发团队和AEM部署的依赖性。 重定向管理器与&#x200B;**AEM as a Cloud Service**&#x200B;和&#x200B;**AEM 6.x**&#x200B;兼容。 默认情况下，初始重定向请求必须命中AEM Publish服务来生成301/302（大多数） CDN的缓存301/302，这样后续请求才能在Edge/CDN上重定向。

[重定向管理器](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html)还支持[AEM as a Cloud Service](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/pipeline-free-url-redirects)的&#x200B;**无管道URL重定向**&#x200B;策略（通过[将重定向编译为](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/subpages/rewritemap.html)Apache RewriteMap[的文本文件](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html)），因此它允许更新Apache Web Server中使用的重定向，而无需直接访问它或重新启动它。 有关更多详细信息，请参阅[教程](https://experienceleague.adobe.com/en/docs/experience-manager-learn/foundation/administration/implementing-pipeline-free-url-redirects#acs-commons---redirect-manager)。 在此方案中，初始重定向请求点击Apache Web Server，而不是AEM Publish服务。

### `Redirect`页面属性

`Redirect`高级选项卡[中的现成(OOTB) ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/sites-console/page-properties.html)页面属性允许内容作者定义当前页面的重定向位置。 此解决方案最适合每页面重定向方案，并且没有用于查看和管理页面重定向的中心位置。

## 哪个解决方案适合实施

下面是确定正确解决方案的一些标准。 此外，贵组织的IT和营销流程应该有助于选择正确的解决方案。

1. 使营销团队或超级用户能够在没有AEM开发团队和AEM部署的情况下管理重定向规则。
1. 管理、验证、跟踪和恢复更改或风险缓解的过程。
1. 通过CDN服务&#x200B;_解决方案为_&#x200B;的Edge提供&#x200B;**主题专业知识**。
