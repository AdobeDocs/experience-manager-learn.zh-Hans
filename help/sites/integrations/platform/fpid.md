---
title: 使用AEM Sites生成Adobe Experience Platform FPID
description: 了解如何使用AEM Sites生成或刷新Adobe Experience Platform FPID Cookie。
version: Cloud Service
feature: Integrations, APIs, Dispatcher
topic: Integrations, Personalization, Development
role: Developer
level: Beginner
last-substantial-update: 2022-10-20T00:00:00Z
jira: KT-11336
thumbnail: kt-11336.jpeg
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Sitesas a Cloud Service、AEM Sites 6.5" before-title="false"
exl-id: 18a22f54-da58-4326-a7b0-3b1ac40ea0b5
duration: 316
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '982'
ht-degree: 0%

---

# 使用AEM Sites生成Experience PlatformFPID

将Adobe Experience Manager (AEM) Sites与Adobe Experience Platform (AEP)集成需要AEM生成和维护唯一的第一方设备ID (FPID) Cookie，以便唯一跟踪用户活动。

请阅读支持文档，以便 [了解第一部分设备ID和Experience CloudID如何配合使用的详细信息](https://experienceleague.adobe.com/docs/platform-learn/data-collection/edge-network/generate-first-party-device-ids.html?lang=en).

以下是使用AEM作为Web主机时FPID的工作方式概述。

![带有AEM的FPID和ECID](./assets/aem-platform-fpid-architecture.png)

## 使用AEM生成并保留FPID

AEM Publish服务通过尽可能多地在CDN和AEM Dispatcher缓存中缓存请求来优化性能。

绝不会缓存生成每用户独特FPID Cookie并返回FPID值的HTTP请求，而是直接从可实现逻辑以保证唯一性的AEM Publish提供服务。

避免在网页或其他可缓存资源的请求中生成FPID Cookie，因为FPID的唯一性要求组合会导致这些资源无法缓存。

下图描述了AEM Publish服务如何管理FPID。

![FPID和AEM流程图](./assets/aem-fpid-flow.png)

1. Web浏览器请求由AEM托管的网页。 可以使用来自CDN或AEM Dispatcher缓存的网页缓存副本来提供请求。
1. 如果网页无法从CDN提供服务，或者AEM Dispatcher缓存，则该请求会到达AEM Publish服务，该服务将生成所请求的网页。
1. 随后，该网页将返回到Web浏览器，并填充无法为请求提供服务的缓存。 使用AEM时，CDN和AEM Dispatcher缓存命中率应大于90%。
1. 该网页包含的JavaScript向AEM Publish服务中的自定义FPID servlet发出不可缓存的异步XHR (AJAX)请求。 由于这是一个不可缓存的请求（由于其随机查询参数和缓存控制标头），因此CDN或AEM Dispatcher从不缓存它，而是始终会访问AEM Publish服务以生成响应。
1. AEM Publish服务中的自定义FPID servlet处理请求，在没有找到现有FPID Cookie时生成新的FPID，或者延长任何现有FPID Cookie的生命周期。 此servlet还会返回响应正文中的FPID，以供客户端JavaScript使用。 幸运的是，自定义FPID servlet逻辑是轻量级的，这可防止此请求影响AEM Publish服务性能。
1. XHR请求的响应将返回给浏览器，并在响应正文中将FPID Cookie和FPID作为JSON，以供Platform Web SDK使用。

## 代码示例

以下代码和配置可以部署到AEM发布服务，以创建端点，该端点生成或延长现有FPID Cookie的生命周期，并将FPID作为JSON返回。

### AEM FPID Cookie servlet

必须创建AEM HTTP端点，以使用 [Sling servlet](https://sling.apache.org/documentation/the-sling-engine/servlets.html#registering-a-servlet-using-java-annotations-1).

+ 此servlet绑定到 `/bin/aem/fpid` 因为访问它不需要身份验证。 如果需要进行身份验证，请绑定到Sling资源类型。
+ 此servlet接受HTTPGET请求。 响应标有 `Cache-Control: no-store` 以防止缓存，但也应使用唯一的缓存无效查询参数请求此端点。

当HTTP请求到达servlet时，此servlet检查请求上是否存在FPID Cookie：

+ 如果存在FPID Cookie，请延长Cookie的生命周期，并收集其值以写入响应。
+ 如果FPID Cookie不存在，请生成新的FPID Cookie并保存该值以写入响应。

然后，Servlet将FPID作为JSON对象写入响应，格式为： `{ fpid: "<FPID VALUE>" }`.

在正文中向客户端提供FPID很重要，因为FPID Cookie已标记 `HttpOnly`，这意味着只有服务器才能读取其值，客户端JavaScript无法读取。

响应正文中的FPID值用于通过Platform Web SDK将调用参数化。

以下是AEM servlet端点的示例代码(可通过以下网站获取： `HTTP GET /bin/aep/fpid`)生成或刷新FPID Cookie，并将FPID作为JSON返回。

+ `core/src/main/java/com/adobe/aem/guides/wkndexamples/core/aep/impl/FpidServlet.java`

```java
package com.adobe.aem.guides.wkndexamples.core.aep.impl;

import com.google.gson.JsonObject;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.servlet.Servlet;
import javax.servlet.http.Cookie;
import java.io.IOException;
import java.util.UUID;

import static org.apache.sling.api.servlets.ServletResolverConstants.SLING_SERVLET_PATHS;
import static org.apache.sling.api.servlets.ServletResolverConstants.SLING_SERVLET_METHODS;

@Component(
        service = {Servlet.class},
        property = {
                SLING_SERVLET_PATHS + "=/bin/aep/fpid",
                SLING_SERVLET_METHODS + "=GET"
        }
)
public class FpidServlet extends SlingAllMethodsServlet {
    private static final Logger log = LoggerFactory.getLogger(FpidServlet.class);
    private static final String COOKIE_NAME = "FPID";
    private static final String COOKIE_PATH = "/";
    private static final int COOKIE_MAX_AGE = 60 * 60 * 24 * 30 * 13;
    private static final String JSON_KEY = "fpid";

    @Override
    protected final void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
        // Try to get an existing FPID cookie, this will give us the user's current FPID if it exists
        final Cookie existingCookie = request.getCookie(COOKIE_NAME);

        String cookieValue;

        if (existingCookie == null) {
            //  If no FPID cookie exists, Create a new FPID UUID
            cookieValue = UUID.randomUUID().toString();
        } else {
            // If a FPID cookie exists. get its FPID UUID so it's life can be extended
            cookieValue = existingCookie.getValue();
        }

        // Add the newly generate FPID value, or the extended FPID value to the response
        // Use addHeader(..), as we need to set SameSite=Lax (and addCoookie(..) does not support this)
        response.addHeader("Set-Cookie",
                COOKIE_NAME + "=" + cookieValue + "; " +
                        "Max-Age=" + COOKIE_MAX_AGE + "; " +
                        "Path=" + COOKIE_PATH + "; " +
                        "HttpOnly; " +
                        "Secure; " +
                        "SameSite=Lax");
        
        // Avoid caching the response in any cache
        response.addHeader("Cache-Control", "no-store");

        // Since the FPID is HttpOnly, JavaScript cannot read it (only the server can)
        // Write the FPID to the response as JSON so client JavaScript can access it.
        final JsonObject json = new JsonObject();
        json.addProperty(JSON_KEY, cookieValue);
        
        // The JSON `{ fpid: "11111111-2222-3333-4444-55555555" }` is returned in the response
        response.setContentType("application/json");
        response.getWriter().write(json.toString());
    }
}
```

### HTML脚本

必须向页面添加自定义客户端JavaScript才能异步调用servlet，生成或刷新FPID Cookie并在响应中返回FPID。

通常将此JavaScript脚本添加到页面使用以下方法之一：

+ [Adobe Experience Platform中的标记](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [AEM客户端库](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/clientlibs.html?lang=en)

对自定义AEM FPID servlet的XHR调用非常快（尽管是异步调用），因此用户可能会访问AEM提供的网页，并在请求完成之前导航离开。
如果发生这种情况，在从AEM加载网页的下一页时，将重新尝试执行相同的过程。

AEM FPID servlet的HTTPGET(`/bin/aep/fpid`)使用随机查询参数进行参数化，以确保浏览器和AEM Publish服务之间的任何基础架构都不会缓存请求的响应。
同样地， `Cache-Control: no-store` 添加了请求标头以支持避免缓存。

在调用AEM FPID servlet时，将从JSON响应中检索FPID，并将其用于 [平台Web SDK](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/tags-configuration/install-web-sdk.html?lang=en) 以将其发送到Experience PlatformAPI。

有关更多详细信息，请参阅Experience Platform文档 [在identityMap中使用FPID](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/first-party-device-ids.html#identityMap)

```javascript
...
<script>
    // Invoke the AEM FPID servlet, and then do something with the response

    fetch(`/bin/aep/fpid?_=${new Date().getTime() + '' + Math.random()}`, { 
            method: 'GET',
            headers: {
                'Cache-Control': 'no-store'
            }
        })
        .then((response) => response.json())
        .then((data) => { 
            // Get the FPID from JSON returned by AEM's FPID servlet
            console.log('My FPID is: ' + data.fpid);

            // Send the `data.fpid` to Experience Platform APIs            
        });
</script>
```

### Dispatcher允许过滤器

GET最后，必须通过AEM Dispatcher的 `filter.any` 配置。

如果未正确实施此Dispatcher配置，则HTTPGET请求 `/bin/aep/fpid` 结果为404。

+ `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
/1099 { /type "allow" /method "GET" /url "/bin/aep/fpid" }
```

## Experience Platform资源

请查看以下Experience Platform文档，了解第一方设备ID (FPID)以及如何通过Platform Web SDK管理身份数据。

+ [生成第一方设备Id](https://experienceleague.adobe.com/docs/platform-learn/data-collection/edge-network/generate-first-party-device-ids.html)
+ [Platform Web SDK中的第一方设备ID](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/first-party-device-ids.html)
+ [Platform Web SDK中的身份数据](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html)
