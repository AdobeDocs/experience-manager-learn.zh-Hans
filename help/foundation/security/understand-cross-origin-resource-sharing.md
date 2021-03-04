---
title: 了解与AEM的跨来源资源共享(CORS)
description: Adobe Experience Manager的跨来源资源共享(CORS)简化了非AEM Web属性，使客户端调用AEM（验证和未验证）获取内容或直接与AEM交互。
version: 6.3, 6,4, 6.5
sub-product: 基础，内容服务，站点
feature: null
topics: security, development, content-delivery
activity: understand
audience: architect, developer
doc-type: article
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '917'
ht-degree: 1%

---


# 了解跨来源资源共享([!DNL CORS])

Adobe Experience Manager的跨来源资源共享([!DNL CORS])使非AEM Web属性能够向AEM发出客户端调用（验证和未验证）以获取内容或直接与AEM交互。

## Adobe Granite跨来源资源共享策略OSGi配置

在AEM中，CORS配置作为OSGi配置工厂进行管理，每个策略都表示为工厂的一个实例。

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![Adobe Granite跨来源资源共享策略OSGi配置](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### 策略选择

通过比较

* `Allowed Origin` 带有 `Origin` request header
* 和`Allowed Paths`。

将使用与这些值匹配的第一个策略。 如果未找到，则任何[!DNL CORS]请求都将被拒绝。

如果未配置任何策略，则[!DNL CORS]请求也不会得到应答，因为处理函数将被禁用，因此会被有效拒绝 — 只要服务器的其他模块不响应[!DNL CORS]。

### 策略属性

#### [!UICONTROL 允许的来源]

* `"alloworigin" <origin> | *`
* 列表`origin`参数，这些参数指定了可访问资源的URI。 对于没有凭据的请求，服务器可将*指定为通配符，从而允许任何来源访问资源。 *绝不建议在生产中使 `Allow-Origin: *` 用，因为它允许每个外国（即攻击者）网站发出浏览器严格禁止不使用CORS的请求。*

#### [!UICONTROL 允许的来源(Regexp)]

* `"alloworiginregexp" <regexp>`
* `regexp`常规表达式的列表，指定可访问资源的URI。 *如果不仔细构建，常规表达式可能导致意外匹配，从而使攻击者能够使用与策略匹配的自定义域名。* 通常建议对每个特定来源主机名使用不同的策略，即 `alloworigin`使这意味着重复配置其他策略属性。不同的来源往往具有不同的生命周期和要求，因此从明确的分离中受益。

#### [!UICONTROL 允许的路径]

* `"allowedpaths" <regexp>`
* `regexp`常规表达式的列表，指定策略所应用的资源路径。

#### [!UICONTROL 暴露的标题]

* `"exposedheaders" <header>`
* 列表标头参数，指示允许浏览器访问的请求标头。

#### [!UICONTROL 最大年龄]

* `"maxage" <seconds>`
* 一个`seconds`参数，指示可缓存预检请求结果的时间。

#### [!UICONTROL 支持的标题]

* `"supportedheaders" <header>`
* 列表`header`参数，指示发出实际请求时可使用的HTTP头。

#### [!UICONTROL 允许的方法]

* `"supportedmethods"`
* 列表方法参数，指示在发出实际请求时可以使用哪些HTTP方法。

#### [!UICONTROL 支持凭据]

* `"supportscredentials" <boolean>`
* 一个`boolean`，指示对请求的响应是否可以暴露到浏览器。 当用作对预检请求的响应的一部分时，这表示实际请求是否可以使用凭据进行。

### 配置示例

站点1是一个基本的、可匿名访问的只读方案，其中内容通过[!DNL GET]请求使用：

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

## 调度程序缓存问题和配置{#dispatcher-caching-concerns-and-configuration}

从Dispatcher 4.1.1+响应头开始，可以缓存。 这样，只要请求为匿名请求，就可以沿[!DNL CORS]请求的资源缓存[!DNL CORS]标头。

通常，在Dispatcher中缓存内容的注意事项也可用于在Dispatcher中缓存CORS响应标头。 下表定义何时可以缓存[!DNL CORS]标头（因此[!DNL CORS]请求）。

| 可缓存 | 环境 | 身份验证状态 | 解释 |
|-----------|-------------|-----------------------|-------------|
| 否 | AEM 发布 | 已验证 | AEM作者上的调度程序缓存仅限于静态的、未创作的资产。 这使得在AEM作者上缓存大多数资源（包括HTTP响应标头）变得困难和不现实。 |
| 否 | AEM 发布 | 已验证 | 避免将CORS头缓存到已验证的请求上。 这符合不缓存已验证请求的常见指导，因为很难确定请求用户的身份验证/授权状态将如何影响已交付的资源。 |
| 是 | AEM 发布 | 匿名 | 调度程序处可缓存的匿名请求也可以缓存其响应标头，从而确保将来的CORS请求可以访问缓存的内容。 AEM发布&#x200B;**上的任何CORS配置更改必须**&#x200B;后跟受影响缓存资源的无效。 最佳实践是清除调度程序缓存的代码或配置部署，因为很难确定可能影响哪些缓存内容。 |

要允许缓存CORS头，请将以下配置添加到所有支持AEM Publish dispatcher.any文件。

```
/cache { 
  ...
  /clientheaders {
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

在对`dispatcher.any`文件进行更改后，请记住&#x200B;**重新启动Web服务器应用程序**。

可能需要完全清除缓存，以确保在`/clientheaders`配置更新后在下一个请求上正确缓存标头。

## CORS疑难解答

日志记录位于`com.adobe.granite.cors`下：

* 启用`DEBUG`以查看有关拒绝[!DNL CORS]请求的原因的详细信息
* 启用`TRACE`可查看有关通过CORS处理程序的所有请求的详细信息

### 提示：

* 使用curl手动重新创建XHR请求，但确保复制所有标题和详细信息，因为每个标题和详细信息都可以起到作用；某些浏览器控制台允许复制curl命令
* 验证请求是否被CORS处理程序拒绝，而不是由身份验证、CSRF令牌过滤器、调度程序过滤器或其他安全层拒绝
   * 如果CORS处理函数以200做出响应，但响应中缺少`Access-Control-Allow-Origin`标头，请查看`com.adobe.granite.cors`中[!DNL DEBUG]下的拒绝日志
* 如果[!DNL CORS]请求的调度程序缓存已启用
   * 确保将`/clientheaders`配置应用于`dispatcher.any`并成功重新启动Web服务器
   * 确保在任何OSGi或调度程序发生任何配置更改后正确清除缓存。
* 如果需要，请检查请求中是否存在身份验证凭据。

## 辅助材料

* [AEM OSGi Configuration Factory for Cross-来源 Resource Sharing Policies](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [跨来源资源共享(W3C)](https://www.w3.org/TR/cors/)
* [HTTP访问控制(Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
