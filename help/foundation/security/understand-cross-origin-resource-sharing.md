---
title: 了解与AEM的跨源资源共享(CORS)
description: Adobe Experience Manager的跨域资源共享(CORS)可帮助非AEM Web属性对AEM进行客户端调用（经过身份验证和未经身份验证）以获取内容或直接与AEM交互。
version: 6,4, 6.5
sub-product: foundation, content-services, sites
topics: security, development, content-delivery
activity: understand
audience: architect, developer
doc-type: article
topic: Security
role: Developer
level: Intermediate
exl-id: 6009d9cf-8aeb-4092-9e8c-e2e6eec46435
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '914'
ht-degree: 1%

---

# 了解跨源资源共享([!DNL CORS])

Adobe Experience Manager的跨域资源共享([!DNL CORS])可帮助非AEM Web属性对AEM进行客户端调用（无论是经过身份验证还是未经身份验证）以获取内容或直接与AEM交互。

## AdobeGranite跨源资源共享策略OSGi配置

CORS配置在AEM中作为OSGi配置工厂进行管理，每个策略都表示为工厂的一个实例。

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![AdobeGranite跨源资源共享策略OSGi配置](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### 策略选择

通过比较

* `Allowed Origin` 和 `Origin` 请求头
* 和 `Allowed Paths` 和请求路径。

将使用与这些值匹配的第一个策略。 如果未找到，则任意 [!DNL CORS] 请求将被拒绝。

如果根本未配置策略， [!DNL CORS] 请求也不会得到应答，因为处理程序将被禁用并因此被有效拒绝 — 只要服务器的其他模块没有响应 [!DNL CORS].

### 策略属性

#### [!UICONTROL 允许的源]

* `"alloworigin" <origin> | *`
* 列表 `origin` 参数指定可访问资源的URI。 对于没有凭据的请求，服务器可以指定 &#42; 作为通配符，从而允许任何源访问资源。 *绝对不建议使用 `Allow-Origin: *` ，因为它允许每个外国（即攻击者）网站发出浏览器严格禁止没有CORS的请求。*

#### [!UICONTROL 允许的源(Regexp)]

* `"alloworiginregexp" <regexp>`
* 列表 `regexp` 指定可访问资源的URI的正则表达式。 *如果未仔细构建正则表达式，则可能会导致意外匹配，从而允许攻击者使用自定义域名，该域名也与策略匹配。* 通常建议使用 `alloworigin`，即使这意味着重复配置其他策略属性也是如此。 不同的起源往往具有不同的生命周期和要求，因此从明确的分离中受益。

#### [!UICONTROL 允许的路径]

* `"allowedpaths" <regexp>`
* 列表 `regexp` 正则表达式，指定策略所适用的资源路径。

#### [!UICONTROL 公开的标题]

* `"exposedheaders" <header>`
* 标头参数列表，指示允许浏览器访问的请求标头。

#### [!UICONTROL 最大年龄]

* `"maxage" <seconds>`
* A `seconds` 参数，指示可缓存预检请求结果的时长。

#### [!UICONTROL 支持的标题]

* `"supportedheaders" <header>`
* 列表 `header` 参数，指示在发出实际请求时可以使用哪些HTTP标头。

#### [!UICONTROL 允许的方法]

* `"supportedmethods"`
* 方法参数列表，指示在发出实际请求时可以使用哪些HTTP方法。

#### [!UICONTROL 支持凭据]

* `"supportscredentials" <boolean>`
* A `boolean` 指示对请求的响应是否可以公开给浏览器。 当用作对飞行前请求的响应的一部分时，这表示实际请求是否可以使用凭据进行。

### 配置示例

网站1是可通过以下方式使用内容的基本、可匿名访问的只读方案 [!DNL GET] 请求：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
    jcr:primaryType="sling:OsgiConfig"
    alloworigin="[https://site1.com]"
    alloworiginregexp="[]"
    allowedpaths="[/content/site1/.*]"
    exposedheaders="[]"
    maxage="{Long}1800"
    supportedheaders="[Origin,Accept,X-Requested-With,Content-Type,
Access-Control-Request-Method,Access-Control-Request-Headers]"
    supportedmethods="[GET]"
    supportscredentials="{Boolean}false"
/>
```

站点2更复杂，需要授权且不安全的请求：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
    jcr:primaryType="sling:OsgiConfig"
    alloworigin="[https://site2.com]"
    alloworiginregexp="[]"
    allowedpaths="[/content/site2/.*,/libs/granite/csrf/token.json]"
    exposedheaders="[]"
    maxage="{Long}1800"
    supportedheaders="[Origin,Accept,X-Requested-With,Content-Type,
Access-Control-Request-Method,Access-Control-Request-Headers,Authorization,CSRF-Token]"
    supportedmethods="[GET,HEAD,POST,DELETE,OPTIONS,PUT]"
    supportscredentials="{Boolean}true"
/>
```

## 调度程序缓存问题和配置 {#dispatcher-caching-concerns-and-configuration}

从Dispatcher 4.1.1+响应标头开始，可以缓存。 这样可以缓存 [!DNL CORS] 标头 [!DNL CORS] — 请求的资源，只要请求是匿名的。

通常，在Dispatcher中缓存内容的注意事项同样适用于在Dispatcher中缓存CORS响应标头。 下表定义了 [!DNL CORS] 标题(因此 [!DNL CORS] 请求)。

| 可缓存 | 环境 | 身份验证状态 | 说明 |
|-----------|-------------|-----------------------|-------------|
| 否 | AEM 发布 | 已验证 | AEM作者上的Dispatcher缓存仅限于静态的未创作资产。 因此，很难且不切实际地在AEM创作上缓存大多数资源，包括HTTP响应标头。 |
| 否 | AEM 发布 | 已验证 | 避免在经过身份验证的请求上缓存CORS标头。 这与不缓存已验证请求的常见指导一致，因为很难确定请求用户的身份验证/授权状态将如何影响交付的资源。 |
| 是 | AEM 发布 | 匿名 | 在Dispatcher处可缓存的匿名请求也可以缓存其响应标头，从而确保将来的CORS请求可以访问缓存的内容。 AEM发布中的任何CORS配置更改 **必须** 随后，受影响的缓存资源将失效。 最佳实践会规定在代码部署或配置部署中清除调度程序缓存，因为很难确定会对哪些缓存内容产生影响。 |

要允许缓存CORS标头，请将以下配置添加到所有支持的AEM Publish dispatcher.any文件中。

```
/cache { 
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

记住 **重新启动web服务器应用程序** 更改后 `dispatcher.any` 文件。

可能需要完全清除缓存，以确保在 `/cache/headers` 配置更新。

## CORS疑难解答

日志记录位于 `com.adobe.granite.cors`:

* 启用 `DEBUG` 查看 [!DNL CORS] 请求被拒绝
* 启用 `TRACE` 查看有关通过CORS处理程序的所有请求的详细信息

### 提示：

* 使用curl手动重新创建XHR请求，但请确保复制所有标头和详细信息，因为每个标头和详细信息都可能产生影响；某些浏览器控制台允许复制curl命令
* 验证请求是否被CORS处理程序拒绝，而不是被身份验证、CSRF令牌过滤器、调度程序过滤器或其他安全层拒绝
   * 如果CORS处理程序以200进行响应，但 `Access-Control-Allow-Origin` 响应中缺少标头，请查看下拒绝的日志 [!DNL DEBUG] in `com.adobe.granite.cors`
* 如果调度程序缓存 [!DNL CORS] 请求已启用
   * 确保 `/cache/headers` 配置应用于 `dispatcher.any` 并且Web服务器已成功重新启动
   * 确保在任何OSGi或dispatcher.any配置发生更改后，缓存已正确清除。
* 如果需要，请检查请求上是否存在身份验证凭据。

## 辅助材料

* [AEM OSGi跨源资源共享策略配置工厂](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [跨源资源共享(W3C)](https://www.w3.org/TR/cors/)
* [HTTP访问控制(Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
