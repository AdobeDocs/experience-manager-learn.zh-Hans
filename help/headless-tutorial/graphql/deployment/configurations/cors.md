---
title: 适用于AEM GraphQL的CORS配置
description: 了解如何配置跨域资源共享(CORS)以与AEM GraphQL一起使用。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10830
thumbnail: KT-10830.jpg
source-git-commit: 6f1000db880c3126a01fa0b74abdb39ffc38a227
workflow-type: tm+mt
source-wordcount: '572'
ht-degree: 1%

---


# 跨源资源共享(CORS)

Adobe Experience Manager as a Cloud Service的跨域资源共享(CORS)可帮助非AEM Web属性对AEM GraphQL API进行基于浏览器的客户端调用。

>[!TIP]
>
> 以下配置是示例。 确保根据项目的要求进行调整。

## CORS要求

基于浏览器的AEM GraphQL API连接需要CORS，因为连接到AEM的客户端并非从AEM的同一源（也称为主机或域）提供。

| 客户端类型 | [单页应用程序(SPA)](../spa.md) | [Web组件/JS](../web-component.md) | [移动设备](../mobile.md) | [服务器到服务器](../server-to-server.md) |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| 需要CORS配置 | ✔ | ✔ | ✘ | ✘ |

## OSGi配置

AEM CORS OSGi配置工厂定义了接受CORS HTTP请求的允许条件。

| 客户端连接到 | AEM Author | AEM 发布 | AEM预览 |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| 需要CORS OSGi配置 | ✔ | ✔ | ✔ |


以下示例为AEM发布定义了OSGi配置(`../config.publish/..`)，但可以添加到 [任何支持的运行模式文件夹](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#runmode-resolution).

关键配置属性包括：

+ `alloworigin` 和/或 `alloworiginregexp` 指定连接到AEM web的客户端运行的源。
+ `allowedpaths` 指定从指定源允许的URL路径模式。
   + 要支持AEM GraphQL持久查询，请添加以下模式： `/graphql/execute.json.*`
   + 要支持体验片段，请添加以下模式： `/content/experience-fragments/.*`
+ `supportedmethods` 为CORS请求指定允许的HTTP方法。 添加 `GET`，以支持AEM GraphQL持久查询（和体验片段）。

[进一步了解CORS OSGi配置。](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html)

此示例配置支持使用AEM GraphQL持久查询。 要使用客户端定义的GraphQL查询，请在 `allowedpaths` 和 `POST` to `supportedmethods`.

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

```json
{
  "alloworigin":[
    "https://spa.external.com/"
  ],
  "alloworiginregexp":[
    "http://localhost:.*"
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

访问需要授权的AEM GraphQL API（通常是AEM发布上的AEM作者或受保护内容）时，请确保CORS OSGi配置具有其他值：

+ `supportedheaders` 还有列表 `"Authorization"`
+ `supportscredentials` 设置为 `true`

对AEM GraphQL API授权的请求需要CORS配置，这种情况并不常见，因为通常情况下，会在 [服务器到服务器应用程序](../server-to-server.md) 因此，不需要CORS配置。 需要CORS配置的基于浏览器的应用程序，例如 [单页应用程序](../spa.md) 或 [Web组件](../web-component.md)，通常使用授权，因为很难保护凭据。

例如，在 `CORSPolicyImpl` OSGi工厂配置：

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

#### OSGi配置示例

+ [OSGi配置的示例可在WKND项目中找到。](https://github.com/adobe/aem-guides-wknd/blob/main/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json)

## Dispatcher 配置

必须将AEM发布（和预览）服务的调度程序配置为支持CORS。

| 客户端连接到 | AEM作者 | AEM 发布 | AEM预览 |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| 需要Dispatcher CORS配置 | ✘ | ✔ | ✔ |

### 允许HTTP请求上的CORS头

更新 `clientheaders.any` 允许HTTP请求头的文件 `Origin`,  `Access-Control-Request-Method`和 `Access-Control-Request-Headers` 传递到AEM，从而允许HTTP请求由 [AEM CORS配置](#osgi-configuration).

`dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any`

```
# Allowing CORS headers to be passed through to the publish tier to support headless and SPA Editor use cases.
# https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_request_headers

"Origin"
"Access-Control-Request-Method"
"Access-Control-Request-Headers"

$include "./default_clientheaders.any"
```

#### Dispatcher配置示例

+ [Dispatcher的示例 _客户端头_ 可在WKND项目中找到配置。](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any#L10-L12)


### 交付CORS HTTP响应头

将Dispatcher场配置为缓存 **CORS HTTP响应头** 以确保在从AEM GraphQL缓存通过添加 `Access-Control-...` 标头。

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

#### Dispatcher配置示例

+ [Dispatcher的示例 _CORS HTTP响应头_ 可在WKND项目中找到配置。](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/available_farms/wknd.farm#L109-L114)
