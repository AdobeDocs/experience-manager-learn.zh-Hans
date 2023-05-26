---
title: AEM GraphQL的CORS配置
description: 了解如何配置跨源资源共享(CORS)以用于AEM GraphQL。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10830
thumbnail: KT-10830.jpg
exl-id: 394792e4-59c8-43c1-914e-a92cdfde2f8a
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '619'
ht-degree: 1%

---

# 跨源资源共享(CORS)

Adobe Experience Manager as a Cloud Service的跨源资源共享(CORS)有助于非AEM Web资产对AEM GraphQL API进行基于浏览器的客户端调用。

以下文章介绍如何配置 _单源_ 通过CORS访问一组特定的AEM Headless端点。 单源意味着仅单个非AEM域访问AEM，例如https://app.example.com连接到https://www.example.com。 出于缓存方面的考虑，使用此方法时可能不适用于多源访问。

>[!TIP]
>
> 以下配置是示例。 确保调整它们以符合项目的要求。

## CORS要求

当连接到AEM GraphQL API的客户端未从与AEM相同的来源（也称为主机或域）提供服务时，基于浏览器的连接AEM API需要CORS。

| 客户端类型 | [单页应用程序(SPA)](../spa.md) | [Web组件/JS](../web-component.md) | [移动设备](../mobile.md) | [服务器到服务器](../server-to-server.md) |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| 需要CORS配置 | ✔ | ✔ | ✘ | ✘ |

## OSGi配置

AEM CORS OSGi配置工厂定义了接受CORS HTTP请求的允许标准。

| 客户端连接到 | AEM Author | AEM 发布 | AEM预览 |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| 需要CORS OSGi配置 | ✔ | ✔ | ✔ |


以下示例为AEM发布定义OSGi配置(`../config.publish/..`)，但可以添加到 [任何支持的运行模式文件夹](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#runmode-resolution).

关键配置属性包括：

+ `alloworigin` 和/或 `alloworiginregexp` 指定连接到AEM Web运行的客户端的源位置。
+ `allowedpaths` 指定允许来自指定源的URL路径模式。
   + 要支持AEM GraphQL持久查询，请添加以下模式： `/graphql/execute.json.*`
   + 要支持体验片段，请添加以下模式： `/content/experience-fragments/.*`
+ `supportedmethods` 为CORS请求指定允许的HTTP方法。 添加 `GET`，以支持AEM GraphQL持久查询（和体验片段）。

[了解有关CORS OSGi配置的更多信息。](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html)

此示例配置支持使用AEM GraphQL持久查询。 要使用客户端定义的GraphQL查询，请添加GraphQL端点URL `allowedpaths` 和 `POST` 到 `supportedmethods`.

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

```json
{
  "alloworigin":[
    "https://spa.external.com/"
  ],
  "alloworiginregexp":[
  ],
  "allowedpaths": [
    "/graphql/execute.json.*",
    "/content/experience-fragments/.*"
  ],
  "supportedheaders": [
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers"
  ],
  "supportedmethods":[
    "GET",
    "HEAD",
    "OPTIONS"
  ],
  "maxage:Integer": 1800,
  "supportscredentials": false,
  "exposedheaders":[ "" ]
}
```

### 授权的AEM GraphQL API请求

访问需要授权的AEM GraphQL API时（通常是AEM Author或AEM Publish上的受保护内容），请确保CORS OSGi配置具有额外的值：

+ `supportedheaders` 另列出 `"Authorization"`
+ `supportscredentials` 设置为 `true`

向AEM GraphQL API发出的授权请求需要配置CORS的情况不常见，因为它们通常发生在 [服务器到服务器应用程序](../server-to-server.md) 因此，不需要CORS配置。 基于浏览器的应用程序，这些应用程序需要CORS配置，例如 [单页应用程序](../spa.md) 或 [Web组件](../web-component.md)，通常使用授权，因为凭据安全很困难。

例如，这两个设置在 `CORSPolicyImpl` OSGi工厂配置：

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

```json
{ 
  ...
  "supportedheaders": [
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers",
    "Authorization"
  ],
  ...
  "supportscredentials": true,
  ...
}
```

#### 示例OSGi配置

+ [在WKND项目中可以找到OSGi配置的示例。](https://github.com/adobe/aem-guides-wknd/blob/main/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json)

## Dispatcher 配置

必须将AEM Publish (and Preview)服务的Dispatcher配置为支持CORS。

| 客户端连接到 | AEM Author | AEM 发布 | AEM预览 |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| 需要调度程序CORS配置 | ✘ | ✔ | ✔ |

### 允许HTTP请求上的CORS标头

更新 `clientheaders.any` 文件以允许HTTP请求标头 `Origin`，  `Access-Control-Request-Method`、和 `Access-Control-Request-Headers` 传递到AEM，从而允许处理HTTP请求 [AEM CORS配置](#osgi-configuration).

`dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any`

```
# Allowing CORS headers to be passed through to the publish tier to support headless and SPA Editor use cases.
# https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_request_headers

"Origin"
"Access-Control-Request-Method"
"Access-Control-Request-Headers"

$include "./default_clientheaders.any"
```

#### 示例Dispatcher配置

+ [Dispatcher示例 _客户端标头_ 可在WKND项目中找到配置。](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any#L10-L12)


### 交付CORS HTTP响应标头

配置Dispatcher场以缓存 **CORS HTTP响应标头** 为了确保在从Dispatcher缓存提供AEM GraphQL持久查询时包含这些查询，请添加 `Access-Control-...` 标头放入缓存标头列表。

+ `dispatcher/src/conf.dispatcher.d/available_farms/wknd.farm`

```
/publishfarm {
    ...
    /cache {
        ...
        # CORS HTTP response headers
        # https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_response_headers
        /headers {
            ...
            "Access-Control-Allow-Origin"
            "Access-Control-Expose-Headers"
            "Access-Control-Max-Age"
            "Access-Control-Allow-Credentials"
            "Access-Control-Allow-Methods"
            "Access-Control-Allow-Headers"
        }
    ...
    }
...
}
```

#### 示例Dispatcher配置

+ [Dispatcher示例 _CORS HTTP响应标头_ 可在WKND项目中找到配置。](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/available_farms/wknd.farm#L109-L114)
