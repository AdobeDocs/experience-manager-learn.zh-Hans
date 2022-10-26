---
title: 使用AEM生成Adobe Experience Platform FPID
description: 了解如何使用AEM生成或刷新Adobe Experience Platform FPID Cookie。
version: Cloud Service
feature: Integrations, APIs, Dispatcher
topic: Integrations, Personalization, Development
role: Developer
level: Beginner
last-substantial-update: 2022-10-20T00:00:00Z
kt: 11336
thumbnail: kt-11336.jpeg
source-git-commit: aeeed85ec05de9538b78edee67db4d632cffaaab
workflow-type: tm+mt
source-wordcount: '1027'
ht-degree: 0%

---

# 使用AEM生成Experience PlatformFPID

将Adobe Experience Manager(AEM)与Adobe Experience Platform(AEP)集成时，需要AEM生成并维护一个唯一的第一方设备ID(FPID)Cookie，以便唯一跟踪用户活动。

请阅读支持文档以 [了解第一部分设备ID和Experience CloudID如何协同工作的详细信息](https://experienceleague.adobe.com/docs/platform-learn/data-collection/edge-network/generate-first-party-device-ids.html?lang=en).

以下是将AEM用作Web主机时FPID的工作方式概述。

![FPID和ECID与AEM](./assets/aem-platform-fpid-architecture.png)

## 使用AEM生成并保留FPID

AEM发布服务会在CDN和AEM Dispatcher缓存中，通过尽可能多地缓存请求来优化性能。

生成每用户唯一FPID Cookie并返回FPID值的HTTP请求必须从不进行缓存，并直接从AEM发布中提供，这些请求可以实施逻辑以确保唯一性。

避免在网页或其他可缓存资源的请求上生成FPID Cookie，因为FPID的唯一性要求的组合会使这些资源变得不可缓存。

下图描述了AEM发布服务如何管理FPID。

![FPID和AEM流程图](./assets/aem-fpid-flow.png)

1. Web浏览器请求由AEM托管的网页。 可以使用从CDN或AEM Dispatcher缓存缓存缓存的网页的缓存副本来提供请求。
1. 如果网页无法从CDN或AEM Dispatcher缓存获取，则请求将到达AEM发布服务，该服务将生成请求的网页。
1. 然后，网页返回到Web浏览器，填充无法提供该请求的缓存。 对于AEM，预期CDN和AEM Dispatcher缓存点击率大于90%。
1. 该网页包含JavaScript，该JavaScript可向AEM发布服务中的自定义FPID Servlet发出不可执行的异步XHR(AJAX)请求。 由于这是一个不可执行的请求（由于它是随机查询参数和Cache-Control标头），因此CDN或AEM Dispatcher从不会缓存该请求，并始终访问AEM发布服务以生成响应。
1. AEM发布服务中的自定义FPID Servlet处理请求，在未找到现有FPID Cookie时生成新的FPID，或延长任何现有FPID Cookie的生命周期。 Servlet还会在响应正文中返回FPID，以供客户端JavaScript使用。 幸运的是，自定义FPID Servlet逻辑非常轻量，可防止此请求影响AEM发布服务性能。
1. XHR请求的响应会返回到包含FPID Cookie的浏览器，并在响应正文中将FPID作为JSON，以供Platform Web SDK使用。

## 代码示例

可以将以下代码和配置部署到AEM发布服务，以创建可生成或延长现有FPID Cookie生命周期并将FPID返回为JSON的端点。

### AEM FPID Cookie Servlet

必须创建AEM HTTP端点，才能使用 [Sling servlet](https://sling.apache.org/documentation/the-sling-engine/servlets.html#registering-a-servlet-using-java-annotations-1).

+ Servlet绑定到 `/bin/aem/fpid` 因为访问它不需要身份验证。 如果需要身份验证，请绑定到Sling资源类型。
+ Servlet接受HTTPGET请求。 响应标记为 `Cache-Control: no-store` 为防止缓存，但也应使用唯一的cache-busting查询参数来请求此端点。

当HTTP请求到达Servlet时，Servlet会检查请求中是否存在FPID Cookie:

+ 如果存在FPID Cookie，请延长Cookie的生命周期，并收集其值以写入响应。
+ 如果FPID Cookie不存在，请生成新的FPID Cookie，并保存值以写入响应。

然后，Servlet将FPID作为JSON对象以下形式写入响应： `{ fpid: "<FPID VALUE>" }`.

由于标记了FPID Cookie，因此务必向主体中的客户端提供FPID `HttpOnly`，这表示只有服务器可以读取其值，而客户端JavaScript则无法读取。

响应正文中的FPID值用于使用Platform Web SDK参数化调用。

以下是AEM servlet端点的示例代码(可通过 `HTTP GET /bin/aep/fpid`)生成或刷新FPID Cookie，并将FPID返回为JSON。

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

必须将自定义客户端JavaScript添加到页面中，才能异步调用Servlet、生成或刷新FPID Cookie并在响应中返回FPID。

通过以下方法之一，键入此JavaScript脚本会添加到页面：

+ [Adobe Experience Platform中的标记](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [AEM客户端库](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/clientlibs.html?lang=en)

对自定义AEM FPID servlet的XHR调用虽然是异步的，但是速度很快，因此用户可以访问AEM提供的网页，并在请求完成之前导航离开。
如果发生这种情况，则同一进程将在从AEM加载的网页的下一页面时重新尝试。

指向AEM FPID servlet的HTTPGET(`/bin/aep/fpid`)已使用随机查询参数进行参数化，以确保浏览器和AEM发布服务之间的任何基础架构不会缓存请求的响应。
同样， `Cache-Control: no-store` 为支持避免缓存，添加了请求标头。

调用AEM FPID servlet时，将从JSON响应中检索FPID，并由 [平台Web SDK](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/tags-configuration/install-web-sdk.html?lang=en) 发送到Experience PlatformAPI。

有关 [在identityMap中使用FPID](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/first-party-device-ids.html#identityMap)

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

最后，必须允许通过AEM Dispatcher的 `filter.any` 配置。

如果此Dispatcher配置未正确实施，则HTTPGET会请求 `/bin/aep/fpid` 结果是404。

+ `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
/1099 { /type "allow" /method "GET" /url "/bin/aep/fpid" }
```

## Experience Platform资源

查看以下Experience Platform文档，了解第一方设备ID(FPID)，并使用Platform Web SDK管理身份数据。

+ [生成第一方设备ID](https://experienceleague.adobe.com/docs/platform-learn/data-collection/edge-network/generate-first-party-device-ids.html)
+ [平台Web SDK中的第一方设备ID](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/first-party-device-ids.html)
+ [平台Web SDK中的身份数据](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html)


