---
title: 部署SPA for AEM GraphQL
description: 了解与AEM GraphQL、无头相关的SPA部署选项。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
source-git-commit: c7d2e69a9039cfbaa43d6d9b65b9fa6f69378716
workflow-type: tm+mt
source-wordcount: '528'
ht-degree: 0%

---


# 部署SPA

在本节中，我们将回顾一种部署SPA(React、Vue、Angular等)的方法，该方法会调用AEM GraphQL API来加载数据，但是，在此之前，我们将先了解必须部署的高级工件。

**SPA构建应用程序工件：**

由SPA框架生成的文件，通常 **HTML、CSS、JS**，即静态生成工件。 对于React应用程序，来自 `build` 目录和 `dist` 目录访问Advertising Cloud的帮助。
将使用这些内部版本工件来向SPA(例如https://HOST/my-aem-spa.html)发出请求。

**AEM GraphQL API:**

显然，此GraphQL API端点(`/graphql/execute.json/<PROJECT-CONFIG>/<PERSISTED-QUERY-NAME>`)必须在AEM域上托管。

在摘要SPA部署架构中，包含两个部分 *1. SPA 2. AEM GraphQL API层*，因此，让我们查看这两个部分的部署选项。


## 部署选项

| 部署选项 | SPA URL | AEM GraphQL API URL | 是否需要CORS配置？ |
| ---------|---------- | ---------|---------- |
| **同一域** | https://**主机**/my-aem-spa.html | https://**主机**/graphql/execute.json/... | ✘ |
| **不同域** | https://**SPA-HOST**/my-aem-spa.html | https://**AEM-HOST**/graphql/execute.json/... | ✔ |

**同一域：**\
两者兼有 *SPA和AEM GraphQL API层* 在此选项中，将部署到 **同一域**. 表示对SPA URI的请求 `/my-aem-spa.html` 和GraphQL API层 `/graphql/execute.json/` 来自完全相同的域。

**不同域：**\
两者兼有 *SPA和AEM GraphQL API层* 在此选项中，将部署到 **不同域**. 表示对SPA URI的请求 `/my-aem-spa.html` 从 **不同域** 比GraphQL API层多 `/graphql/execute.json/` 请求。 请注意，作为此部署选项的一部分，您需要 [配置CORS](cors.md) 在AEM实例上。

>[!NOTE]
>
>必须在AEM实例上正确配置CORS， [请参阅此处的步骤](cors.md).

### 在同一域上部署

在同一域上部署时，该域可能是 **主AEM域** (在AEM域上)或 **主SPA域** (在AEM域之外)和传入的SPA中，可以在任何一个部署组件（如CDN）上拆分AEM GraphQL API请求([Fastly](https://docs.fastly.com/en/guides/routing-assets-to-different-origins)、 Akamai、 [CloudFront](https://aws.amazon.com/premiumsupport/knowledge-center/cloudfront-distribution-serve-content/))、 [带有反向代理的HTTPD](https://httpd.apache.org/docs/2.4/howto/reverse_proxy.html). 换句话说，您仍然可以将SPA内部版本工件和AEM GraphQL API部署到不同的服务器，但对于最终用户而言，这些API是从单个域交付的，位于路由到其他目标或源服务器的场景后面。

此外，还可以在AEM中托管SPA内部版本对象 *但不建议这样做。*

| 相同域部署 | CDN拆分 | HTTPD +反向代理 | AEM托管的SPA工件 |
| ---------|---------- | ---------|---------- |
| **在AEM域上** | ✔ | ✔ | ✔ |
| **OFF AEM域** | ✔ | ✔ | **不适用** |


**HTTPD +反向代理**

示例配置如下所示

>[!TIP]
>
> 以下配置是示例。 确保根据项目的要求进行调整。

在AEM域上

    &quot;
    ProxyPass &quot;/${YOUR-SPA-URI}&quot; &quot;http://${SPA-HOST}/&quot;
    ProxyPassReverse &quot;/${YOUR-SPA-URI}&quot; &quot;http://${SPA-HOST}/&quot;
    &quot;

OFF AEM域

    &quot;
    ProxyPass &quot;/graphql/execute.json/&quot; &quot;http://${AEM-HOST}/&quot;
    ProxyPassReverse &quot;/graphql/execute.json/&quot; &quot;http://${AEM-HOST}/&quot;
    &quot;




### 在不同域上部署

在此方案中，SPA内部版本对象部署到与AEM GraphQL API域不同的域，因此，对于最终用户，这些内部版本对象将从两个不同的域交付 [CORS配置](cors.md) 必须在AEM上。

**SPA应用程序通过不同域的请求**

![不同域SPA交付](assets/spa/different-domain-spa-delivery.png)


**AEM GraphQL API上的CORS响应标头**

![CORS响应标头AEM GraphQL API](assets/spa/CORS-response-header-aem-graphql-api.png)


