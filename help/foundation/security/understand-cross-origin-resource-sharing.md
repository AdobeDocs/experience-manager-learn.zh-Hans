---
title: 了解与AEM的跨来源资源共享(CORS)
description: Adobe Experience Manager的跨来源资源共享(CORS)方便了非AEM Web属性向AEM进行客户端调用，包括经过身份验证和未经身份验证，以获取内容或直接与AEM交互。
version: 6.3, 6,4, 6.5
sub-product: 基础，内容服务，站点
feature: null
topics: security, development, content-delivery
activity: understand
audience: architect, developer
doc-type: article
translation-type: tm+mt
source-git-commit: ecbd4d21c5f41b2bc6db3b409767b767f00cc5d1
workflow-type: tm+mt
source-wordcount: '917'
ht-degree: 1%

---


# 了解跨来源资源共享([!DNL CORS])

Adobe Experience Manager的跨来源资源共享([!DNL CORS])方便了非AEM Web属性向AEM（经过身份验证和未经身份验证）发出客户端调用，以获取内容或直接与AEM交互。

## AdobeGranite跨来源资源共享策略OSGi配置

CORS配置在AEM中作为OSGi配置工厂进行管理，每个策略都表示为工厂的一个实例。

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![AdobeGranite跨来源资源共享策略OSGi配置](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### 策略选择

通过比较

* `Allowed Origin` 和请 `Origin` 求标题
* 和 `Allowed Paths` 请求路径。

将使用与这些值匹配的第一个策略。 如果找不到任何请求， [!DNL CORS] 则会拒绝任何请求。

如果根本未配置任何策略，则也 [!DNL CORS] 不会应答请求，因为将禁用处理程序，从而有效地拒绝该处理程序——只要服务器的其他模块没有响应 [!DNL CORS]。

### 策略属性

#### [!UICONTROL 允许的来源]

* `"alloworigin" <origin> | *`
* 列表 `origin` 指定可访问资源的URI的参数。 对于无凭据的请求，服务器可指定*作为通配符，从而允许任何来源访问资源。 *绝不建议在生产中使`Allow-Origin: *`用，因为它允许每个外国（即攻击者）网站发出浏览器严格禁止不使用CORS的请求。*

#### [!UICONTROL 允许的来源(Regexp)]

* `"alloworiginregexp" <regexp>`
* 常规 `regexp` 表达式的列表，指定可访问资源的URI。 *常规表达式如果不仔细构建，就可能导致意外匹配，从而使攻击者能够使用同样匹配策略的自定义域名。* 通常建议对每个特定来源主机名使用不同的策略，即 `alloworigin`使这意味着重复配置其他策略属性。 不同的来源往往具有不同的生命周期和要求，因此从明确的分离中受益。

#### [!UICONTROL 允许的路径]

* `"allowedpaths" <regexp>`
* 常规 `regexp` 表达式的列表，指定应用策略的资源路径。

#### [!UICONTROL 公开的标题]

* `"exposedheaders" <header>`
* 标题参数的列表，指示允许浏览器访问的请求标头。

#### [!UICONTROL 最大年龄]

* `"maxage" <seconds>`
* 一 `seconds` 个参数，指示可缓存预检请求结果的时间。

#### [!UICONTROL 支持的标题]

* `"supportedheaders" <header>`
* 列表 `header` 参数，指示发出实际请求时可以使用哪个HTTP头。

#### [!UICONTROL 允许的方法]

* `"supportedmethods"`
* 列表方法参数，指示在发出实际请求时可以使用哪些HTTP方法。

#### [!UICONTROL 支持凭据]

* `"supportscredentials" <boolean>`
* 指 `boolean` 示对请求的响应是否可以暴露给浏览器。 当用作对预检请求的响应的一部分时，这表示实际请求是否可以使用凭据进行。

### 配置示例

站点1是通过请求使用内容的基本、可匿名访问的只读 [!DNL GET] 方案：

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

从Dispatcher 4.1.1+响应头开始，可以缓存。 这样，只要请求为 [!DNL CORS] 匿名请求， [!DNL CORS]就可以在请求资源上缓存头。

通常，在Dispatcher中缓存内容的注意事项同样适用于在调度程序中缓存CORS响应头。 下表定义何时可 [!DNL CORS] 以缓存标头( [!DNL CORS] 从而缓存请求)。

| 可缓存 | 环境 | 身份验证状态 | 说明 |
|-----------|-------------|-----------------------|-------------|
| 否 | AEM 发布 | 已验证 | AEM作者上的调度程序缓存仅限于静态、未创作的资产。 这使得在AEM作者上缓存大多数资源（包括HTTP响应头）变得困难且不切实际。 |
| 否 | AEM 发布 | 已验证 | 避免在经过身份验证的请求上缓存CORS头。 这符合不缓存已验证请求的常见指导，因为很难确定请求用户的身份验证／授权状态将如何影响已交付的资源。 |
| 是 | AEM 发布 | 匿名 | 调度程序处可缓存的匿名请求也可以缓存其响应头，从而确保将来的CORS请求可以访问缓存的内容。 AEM发布上的任何CORS配置 **更改** 后，受影响的缓存资源必须失效。 最佳实践是清除调度程序缓存的代码或配置部署，因为很难确定哪些缓存内容可能会受到影响。 |

要允许缓存CORS头，请将以下配置添加到所有支持AEM Publish dispatcher.any的文件。

```
/cache { 
  ...
  /headers {
      "Access-Control-Allow-Origin",
      "Access-Control-Expose-Headers",
      "Access-Control-Max-Age",
      "Access-Control-Allow-Credentials",
      "Access-Control-Allow-Methods",
      "Access-Control-Allow-Headers"
  }
  ...
}
```

在对文 **件进行更改后** ，请记住重新启动Web服务器应 `dispatcher.any` 用程序。

可能需要完全清除缓存，以确保在配置更新后在下一个请求上正确缓存 `/headers` 标头。

## CORS疑难解答

日志记录可在 `com.adobe.granite.cors`:

* 启 `DEBUG` 用查看拒绝请求 [!DNL CORS] 的详细信息
* 启用 `TRACE` 查看有关通过CORS处理程序的所有请求的详细信息

### 提示：

* 使用curl手动重新创建XHR请求，但确保复制所有标题和详细信息，因为每个标题和详细信息都会有所不同；某些浏览器控制台允许复制curl命令
* 验证请求是否被CORS处理程序拒绝，而不是由身份验证、CSRF令牌过滤器、调度程序过滤器或其他安全层拒绝
   * 如果CORS处理程序以200作出响 `Access-Control-Allow-Origin` 应，但响应中缺少标题，请查看中的拒绝 [!DNL DEBUG] 日志 `com.adobe.granite.cors`
* 如果已启用请求的 [!DNL CORS] 调度程序缓存
   * 确保将 `/headers` 配置应用于 `dispatcher.any` 并成功重新启动Web服务器
   * 确保在任何OSGi或调度程序发生任何配置更改后正确清除缓存。
* 如果需要，检查请求中是否存在身份验证凭据。

## 支持材料

* [AEM OSGi跨来源资源共享策略的配置工厂](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [跨来源资源共享(W3C)](https://www.w3.org/TR/cors/)
* [HTTP访问控制(Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
