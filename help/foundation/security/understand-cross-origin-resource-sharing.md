---
title: 了解 AEM 中的跨源资源共享 (CORS)
description: Adobe Experience Manager 的跨域资源共享 (CORS) 功能支持非 AEM Web 属性通过客户端调用 AEM（包括经过身份验证和未经过身份验证的调用），以获取内容或直接与 AEM 进行交互。
version: Experience Manager 6.4, Experience Manager 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: Security, APIs
doc-type: Article
topic: Security
role: Developer
level: Intermediate
exl-id: 6009d9cf-8aeb-4092-9e8c-e2e6eec46435
duration: 240
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1011'
ht-degree: 100%

---

# 了解跨源资源共享 ([!DNL CORS])

Adobe Experience Manager 的跨域资源共享 ([!DNL CORS]) 功能支持非 AEM Web 属性通过客户端调用 AEM（包括经过身份验证和未经过身份验证的调用），以获取内容或直接与 AEM 进行交互。

本文档中概述的 OSGI 配置足以满足以下需求：

1. AEM Publish 上的单源资源共享
2. CORS 对 AEM Author 的访问

如果需要在 AEM Publish 上进行多源 CORS 访问，请参阅[此文档](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/configurations/cors.html?lang=zh-Hans#dispatcher-configuration)。

## Adobe Granite 跨域资源共享策略 OSGi 配置

在 AEM 中，CORS 配置作为 OSGi 配置工厂进行管理，其中每个策略都表示为该工厂的一个实例。

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![Adobe Granite 跨域资源共享策略 OSGi 配置](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### 策略选择

通过比较以下两项来决定使用哪一条策略：

* `Allowed Origin` 与 `Origin` 请求头是否匹配
* 以及 `Allowed Paths` 与请求路径是否匹配。

系统会使用第一条匹配的策略。如果没有匹配的策略，任何 [!DNL CORS] 请求均会被拒绝。

如果完全没有配置任何策略，只要服务器的其他模块不对 [!DNL CORS] 做出响应，[!DNL CORS] 请求也不会得到应答，因为处理程序已被禁用，因此实际上会被拒绝。

### 策略属性

#### [!UICONTROL 允许的来源]

* `"alloworigin" <origin> | *`
* 指定可能访问资源的 URI 的 `origin` 参数列表。对于没有凭据的请求，服务器可以指定 &#42; 作为通配符，从而允许任何来源访问资源。*强烈不建议在生产环境中使用 `Allow-Origin: *`，因为这会允许任意外部网站（包括攻击者）发出没有 CORS 的请求，而这些请求是浏览器严格禁止的。*

#### [!UICONTROL 允许的来源（正则表达式）]

* `"alloworiginregexp" <regexp>`
* 指定可能访问资源的 URI 的 `regexp` 正则表达式列表。*如果构建不当，正则表达式可能会导致意外匹配，从而使攻击者能够使用与策略匹配的自定义域名。* 通常建议为每个特定的来源主机名使用 `alloworigin` 设置单独的策略，即使这意味着需要重复配置其他策略属性也是如此。不同的来源往往具有不同的生命周期和要求，因此明确区分是有益的。

#### [!UICONTROL 允许的路径]

* `"allowedpaths" <regexp>`
* 指定该策略适用的资源路径的 `regexp` 正则表达式列表。

#### [!UICONTROL 暴露的标头]

* `"exposedheaders" <header>`
* 指示允许浏览器访问的响应标头的标头参数列表。对于 CORS 请求（非预检请求），如果这些值不为空，则会被复制到 `Access-Control-Expose-Headers` 响应标头中。然后，列表中的值（标头名称）将会对浏览器可见；否则，浏览器无法读取这些标头。

#### [!UICONTROL 最大缓存时间]

* `"maxage" <seconds>`
* 一个 `seconds` 参数，指示可以缓存预检请求结果的时长。

#### [!UICONTROL 支持的标头]

* `"supportedheaders" <header>`
* 指示在进行实际请求时可以使用哪些 HTTP 请求标头的 `header` 参数列表。

#### [!UICONTROL 允许的方法]

* `"supportedmethods"`
* 方法参数列表，指示在发出实际请求时可以使用哪些 HTTP 方法。

#### [!UICONTROL 支持凭据]

* `"supportscredentials" <boolean>`
* 一个 `boolean`，指示是否可以将对请求的响应暴露给浏览器。当作为对预检请求的响应的一部分时，这表示是否可以使用凭据发出实际请求。

### 示例配置

网站 1 是一个基本的、可匿名访问的只读场景，其内容通过 [!DNL GET] 请求进行消费：

```json
{
  "supportscredentials":false,
  "exposedheaders":[
    ""
  ],
  "supportedmethods":[
    "GET",
    "HEAD"
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
    "Access-Control-Request-Headers"
  ]
}
```

网站 2 更为复杂，需要经过授权以及变异的 (POST, PUT, DELETE) 请求：

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

## Dispatcher 缓存问题和配置 {#dispatcher-caching-concerns-and-configuration}

从 Dispatcher 4.1.1+ 版本开始，响应标头可以被缓存。这使得只要请求是匿名的，就可以将 [!DNL CORS] 标头与 [!DNL CORS] 请求的资源一起缓存。

通常，在 Dispatcher 缓存内容时需要考虑的因素同样适用于在 Dispatcher 缓存 CORS 响应标头。下表定义了何时可以缓存 [!DNL CORS] 头（以及相应的 [!DNL CORS] 请求）。

| 可缓存 | 环境 | 身份验证状态 | 解释 |
|-----------|-------------|-----------------------|-------------|
| 否 | AEM Publish | 已经身份验证 | AEM Author 上的 Dispatcher 缓存仅限于静态、非创作的资产。这使缓存 AEM Author 上的大多数资源（包括 HTTP 响应标头）变得困难且不切实际。 |
| 否 | AEM Publish | 已经身份验证 | 避免对经过身份验证的请求缓存 CORS 标头。这与不缓存已经过身份验证的请求的常见指导原则一致，因为很难确定请求用户的身份验证/授权状态将如何影响所投放的资源。 |
| 是 | AEM Publish | 匿名 | Dispatcher 中可缓存的匿名请求也可以缓存其响应标头，从而确保未来的 CORS 请求可以访问缓存的内容。AEM Publish 上的任何 CORS 配置更改&#x200B;**必须**&#x200B;随后清除受影响的缓存资源最佳实践要求在代码或配置部署时清理 Dispatcher 缓存，因为很难确定哪些缓存内容可能受到影响。 |

### 允许 CORS 请求标头

为了使所需的 [HTTP 请求标头能够通过并进入 AEM 进行处理](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=zh-Hans#specifying-the-http-headers-to-pass-through-clientheaders)，必须在 Disaptcher 的 `/clientheaders` 配置中允许这些请求标头。

```
/clientheaders {
   ...
   "Origin"
   "Access-Control-Request-Method"
   "Access-Control-Request-Headers"
}
```

### 缓存 CORS 请求标头

为了允许在缓存内容上缓存和提供 CORS 标头，请将以下 [/cache /headers configuration](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=zh-Hans#caching-http-response-headers) 添加到 AEM Publish `dispatcher.any`文件中。

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

在对 `dispatcher.any` 文件进行更改后，请记得&#x200B;**重新启动 Web 服务器应用程序**。

为确保在 `/cache/headers` 配置更新后的下一个请求中，标头得到适当缓存，可能需要完全清除缓存。

## CORS 故障排除

日志记录功能可在 `com.adobe.granite.cors` 下使用：

* 启用 `DEBUG` 以查看 [!DNL CORS] 请求被拒绝的具体原因
* 启用 `TRACE` 以查看通过 CORS 处理程序的所有请求的详细信息

### 提示：

* 使用 curl 手动重新创建 XHR 请求，但要确保复制所有标头信息和详细信息，因为每个信息都可能产生影响；一些浏览器控制台允许复制 curl 命令
* 验证请求是否已被 CORS 处理程序拒绝，而非身份验证、CSRF 令牌过滤器、Dispatcher 过滤器或其他安全层拒绝
   * 如果 CORS 处理程序返回 200 状态码，但响应中未包含 `Access-Control-Allow-Origin` 标头，请查看 `com.adobe.granite.cors` 中 [!DNL DEBUG] 下的拒绝日志
* 如果启用了 [!DNL CORS] 请求的 Dispatcher 缓存
   * 确保将 `/cache/headers` 配置应用于 `dispatcher.any`，并且 Web 服务器已成功重启
   * 确保在任何 OSGi 或调度器进行配置更改后，缓存已正确清除。
* 如有需要，请检查请求中是否包含身份验证凭据。

## 支持材料

* [用于跨源资源共享策略的 AEM OSGi 配置工厂](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [跨源资源共享 (W3C)](https://www.w3.org/TR/cors/)
* [HTTP 访问控制 (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
