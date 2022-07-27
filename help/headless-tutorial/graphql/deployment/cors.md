---
title: AEM GraphQL的CORS配置
description: 了解如何配置跨域资源共享(CORS)以与AEM GraphQL一起使用。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
source-git-commit: 56831a30a442388e34d8388546787ed22e7577f5
workflow-type: tm+mt
source-wordcount: '362'
ht-degree: 2%

---


# 跨源资源共享(CORS)

Adobe Experience Manager as a Cloud Service的跨域资源共享(CORS)有助于实现非AEM Web属性，从而对AEM GraphQL API进行基于浏览器的客户端调用。

>[!TIP]
>
> 以下配置是示例。 确保根据项目的要求进行调整。



## CORS要求

基于浏览器的AEM GraphQL API连接需要CORS，因为连接到AEM的客户端并非从AEM的同一源（也称为主机或域）提供。

| 客户端类型 | 单页应用程序(SPA) | Web组件/JS | 移动设备 | 服务器到服务器 |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| 需要CORS配置 | ✔ | ✔ | ✘ | ✘ |

## OSGi配置

AEM CORS OSGi配置工厂定义了接受CORS HTTP请求的允许条件。

| 客户端连接到 | AEM Author | AEM 发布 | AEM预览 |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| 需要CORS OSGi配置 | ✔ | ✔ | ✔ |


以下示例为AEM发布定义了OSGi配置(`../config.publish/..`)，但可以添加到 [任何支持的runmode文件夹](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#runmode-resolution).

关键配置属性包括：

+ `alloworigin` 和/或 `alloworiginregexp` 指定连接到AEM web的客户端运行时所在的源。
+ `allowedpaths` 指定从指定的源允许的URL路径模式。 要支持AEM GraphQL持久查询，请使用以下模式： `"/graphql/execute.json.*"`
+ `supportedmethods` 为CORS请求指定允许的HTTP方法。 添加 `GET`，以支持AEM GraphQL持久查询。

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
    "/graphql/execute.json.*"
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
    "HEAD"
  ],
  "maxage:Integer": 1800,
  "supportscredentials": false,
  "exposedheaders":[ "" ]
}
```


## 调度程序配置

必须将AEM发布（和预览）服务的调度程序配置为支持CORS。

| 客户端连接到 | AEM作者 | AEM 发布 | AEM预览 |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| 需要Dispatcher CORS配置 | ✘ | ✔ | ✔ |

### 允许HTTP请求上的CORS头

更新 `clientheaders.any` 允许HTTP请求头的文件 `Origin`,  `Access-Control-Request-Method` 和 `Access-Control-Request-Headers` 传递到AEM，从而允许HTTP请求由 [AEM CORS配置](#osgi-configuration).

`dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any`

```
# Allowing CORS headers to be passed through to the publish tier to support headless and SPA Editor use cases.
# https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_request_headers

"Origin"
"Access-Control-Request-Method"
"Access-Control-Request-Headers"

$include "./default_clientheaders.any"
```

### 交付CORS HTTP响应头

将调度程序场配置为缓存 **CORS HTTP响应头** 以确保在从AEM GraphQL持久查询(通过添加 `Access-Control-...` 标头。

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
