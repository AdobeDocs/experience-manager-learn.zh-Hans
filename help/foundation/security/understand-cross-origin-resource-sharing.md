---
title: 了解与AEM的跨源资源共享(CORS)
description: Adobe Experience Manager的跨源资源共享(CORS)使非AEM Web资产能够对AEM进行客户端调用（无论是否经过身份验证），以获取内容或直接与AEM交互。
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
topics: security, development, content-delivery
feature: Security, APIs
activity: understand
audience: architect, developer
doc-type: article
topic: Security
role: Developer
level: Intermediate
exl-id: 6009d9cf-8aeb-4092-9e8c-e2e6eec46435
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: tm+mt
source-wordcount: '913'
ht-degree: 1%

---

# 了解跨源资源共享([!DNL CORS])

Adobe Experience Manager的跨源资源共享([!DNL CORS])便于非AEM Web资产对AEM进行客户端调用（无论是否经过身份验证），以获取内容或直接与AEM交互。

## AdobeGranite跨源资源共享策略OSGi配置

CORS配置在AEM中作为OSGi配置工厂进行管理，每个策略表示为一个工厂实例。

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![AdobeGranite跨源资源共享策略OSGi配置](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### 策略选择

通过比较

* `Allowed Origin` 使用 `Origin` 请求标头
* 和 `Allowed Paths` 请求路径。

使用匹配这些值的第一个策略。 如果未找到，则任何 [!DNL CORS] 请求被拒绝。

如果完全没有配置策略， [!DNL CORS] 由于处理程序被禁用并因此被有效拒绝，只要服务器的其他模块没有响应，请求也不会被应答 [!DNL CORS].

### 策略属性

#### [!UICONTROL 允许的源]

* `"alloworigin" <origin> | *`
* 列表 `origin` 指定可以访问资源的URI的参数。 对于没有凭据的请求，服务器可以指定 &#42; 作为通配符，从而允许任何源访问资源。 *绝对不建议使用 `Allow-Origin: *` 因为它允许所有外国（即攻击者）网站发出不带CORS的请求被浏览器严格禁止。*

#### [!UICONTROL 允许的源（正则表达式）]

* `"alloworiginregexp" <regexp>`
* 列表 `regexp` 指定可以访问资源的URI的正则表达式。 *如果不仔细构建，正则表达式可能会导致意外匹配，使得攻击者使用也匹配策略的自定义域名。* 通常建议为每个特定的源主机名使用单独的策略 `alloworigin`，即使这意味着重复配置其他策略属性也是如此。 不同的起源往往有不同的生命周期和要求，从而有利于清晰的分离。

#### [!UICONTROL 允许的路径]

* `"allowedpaths" <regexp>`
* 列表 `regexp` 指定策略适用的资源路径的正则表达式。

#### [!UICONTROL 公开的标头]

* `"exposedheaders" <header>`
* 标头参数列表，指示允许浏览器访问的请求标头。

#### [!UICONTROL 最长使用期限]

* `"maxage" <seconds>`
* A `seconds` 指示可缓存预检请求结果的时间的参数。

#### [!UICONTROL 支持的标头]

* `"supportedheaders" <header>`
* 列表 `header` 指示发出实际请求时可以使用哪些HTTP标头的参数。

#### [!UICONTROL 允许的方法]

* `"supportedmethods"`
* 方法参数列表，指示在发出实际请求时可以使用哪些HTTP方法。

#### [!UICONTROL 支持凭据]

* `"supportscredentials" <boolean>`
* A `boolean` 指示对请求的响应是否可以向浏览器公开。 当用作对飞行前请求的响应的一部分时，这指示是否可以使用凭据发出实际请求。

### 示例配置

站点1是一个基本的、可匿名访问的只读方案，其中内容通过以下方式使用 [!DNL GET] 请求：

```json
{
  "supportscredentials":false,
  "exposedheaders":[
    ""
  ],
  "supportedmethods":[
    "GET",
    "HEAD",
    "OPTIONS"
  ],
  "alloworigin":[
    "http://127.0.0.1:3000",
    "https://site1.com"
    
  ],
  "maxage:Integer": 1800,
  "alloworiginregexp":[
    "http://localhost:.*"
    "https://.*\.site1\.com"
  ],
  "allowedpaths":[
    "/content/_cq_graphql/site1/endpoint.json",
    "/graphql/execute.json.*",
    "/content/site1/.*"
  ],
  "supportedheaders":[
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers",
  ]
}
```

站点2更加复杂，需要进行授权和异构(POST、PUT、DELETE)请求：

```json
{
  "supportscredentials":true,
  "exposedheaders":[
    ""
  ],
  "supportedmethods":[
    "GET",
    "HEAD"
    "POST",
    "DELETE",
    "OPTIONS",
    "PUT"
  ],
  "alloworigin":[
    "http://127.0.0.1:3000",
    "https://site2.com"
    
  ],
  "maxage:Integer": 1800,
  "alloworiginregexp":[
    "http://localhost:.*"
    "https://.*\.site2\.com"
  ],
  "allowedpaths":[
    "/content/site2/.*",
    "/libs/granite/csrf/token.json",
  ],
  "supportedheaders":[
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers",
    "Authorization",
    "CSRF-Token"
  ]
}
```

## Dispatcher缓存顾虑和配置 {#dispatcher-caching-concerns-and-configuration}

从Dispatcher 4.1.1开始，可以缓存响应标头。 这使得可以缓存 [!DNL CORS] 标头 [!DNL CORS] — 请求的资源，只要该请求是匿名的。

通常，在Dispatcher中缓存内容的相同考虑因素也可以应用于在Dispatcher中缓存CORS响应标头。 下表定义何时 [!DNL CORS] 标头(以及这样 [!DNL CORS] 请求)。

| 可缓存 | 环境 | 身份验证状态 | 解释 |
|-----------|-------------|-----------------------|-------------|
| 否 | AEM 发布 | 已验证 | AEM Author上的Dispatcher缓存限制为静态的非创作资产。 这使得在AEM Author上缓存大多数资源（包括HTTP响应标头）变得困难和不现实。 |
| 否 | AEM 发布 | 已验证 | 避免在经过身份验证的请求上缓存CORS标头。 这符合不缓存经过身份验证的请求的常见指南，因为很难确定请求用户的身份验证/授权状态将如何影响投放的资源。 |
| 是 | AEM 发布 | 匿名 | 可在Dispatcher中缓存的匿名请求也可以缓存其响应标头，从而确保未来的CORS请求可以访问缓存的内容。 AEM Publish上的任何CORS配置更改 **必须** 随后使受影响的缓存资源失效。 最佳实践要求在代码或配置部署中清除Dispatcher缓存，因为很难确定哪些缓存内容可能会受到影响。 |

要允许缓存CORS标头，请将以下配置添加到所有支持的AEM Publish dispatcher.any文件。

```
/myfarm { 
  ...
  /headers {
      "Origin"
      "Access-Control-Allow-Origin"
      "Access-Control-Expose-Headers"
      "Access-Control-Max-Age"
      "Access-Control-Allow-Credentials"
      "Access-Control-Allow-Methods"
      "Access-Control-Allow-Headers"
  }
  ...
}
```

请记住 **重新启动Web服务器应用程序** 在对进行更改之后 `dispatcher.any` 文件。

可能需要完全清除缓存，以确保在之后的下一个请求中正确缓存标头 `/cache/headers` 配置更新。

## CORS故障排除

日志记录位于 `com.adobe.granite.cors`：

* 启用 `DEBUG` 查看有关原因 [!DNL CORS] 请求被拒绝
* 启用 `TRACE` 查看有关通过CORS处理程序处理的所有请求的详细信息

### 提示：

* 使用curl手动重新创建XHR请求，但请确保复制所有标题和详细信息，因为每个标题和详细信息都可以发挥作用；某些浏览器控制台允许复制curl命令
* 验证请求是否被CORS处理程序拒绝，而不是被身份验证、CSRF令牌过滤器、调度程序过滤器或其他安全层拒绝
   * 如果CORS处理程序使用200进行响应，但是 `Access-Control-Allow-Origin` 响应中不存在标头，请查看日志中的拒绝信息 [!DNL DEBUG] 在 `com.adobe.granite.cors`
* 如果Dispatcher缓存 [!DNL CORS] 请求已启用
   * 确保 `/cache/headers` 配置应用于 `dispatcher.any` 并且Web服务器已成功重新启动
   * 确保在任何OSGi或dispatcher.any配置更改后正确清除缓存。
* 如果需要，检查请求中是否存在身份验证凭据。

## 支持材料

* [用于跨源资源共享策略的AEM OSGi配置工厂](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [跨源资源共享(W3C)](https://www.w3.org/TR/cors/)
* [HTTP访问控制(Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
