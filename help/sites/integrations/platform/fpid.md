---
title: 使用AEM產生Adobe Experience Platform FPID
description: 瞭解如何使用AEM產生或重新整理Adobe Experience Platform FPID Cookie。
version: Cloud Service
feature: Integrations, APIs, Dispatcher
topic: Integrations, Personalization, Development
role: Developer
level: Beginner
last-substantial-update: 2022-10-20T00:00:00Z
kt: 11336
thumbnail: kt-11336.jpeg
exl-id: 18a22f54-da58-4326-a7b0-3b1ac40ea0b5
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '1027'
ht-degree: 0%

---

# 使用AEM產生Experience PlatformFPID

將Adobe Experience Manager (AEM)與Adobe Experience Platform (AEP)整合需要AEM產生和維護唯一的第一方裝置ID (FPID) Cookie，以專門追蹤使用者活動。

閱讀支援檔案至 [瞭解第一部分裝置ID和Experience CloudID如何搭配運作的詳細資訊](https://experienceleague.adobe.com/docs/platform-learn/data-collection/edge-network/generate-first-party-device-ids.html?lang=en).

以下為使用AEM做為Web主機時，FPID運作方式的概觀。

![使用AEM的FPID和ECID](./assets/aem-platform-fpid-architecture.png)

## 使用AEM產生並保留FPID

AEM Publish服務會透過快取要求(儘可能多地在CDN和AEM Dispatcher快取中)來最佳化效能。

絕對不會快取產生每位使用者不重複FPID Cookie並傳回FPID值的HTTP請求，並直接從可實作邏輯以確保唯一性的AEM Publish提供服務。

請避免在網頁或其他可快取資源的請求上產生FPID Cookie，因為FPID的唯一性要求組合會使這些資源無法快取。

下圖說明AEM Publish服務如何管理FPID。

![FPID和AEM流程圖](./assets/aem-fpid-flow.png)

1. 網頁瀏覽器會要求使用AEM託管的網頁。 可以使用來自CDN或AEM Dispatcher快取的網頁快取復本來提供請求。
1. 如果網頁無法從CDN或AEM Dispatcher快取提供服務，請求會送達AEM Publish服務，該服務會產生請求的網頁。
1. 然後網頁會傳回至網頁瀏覽器，並填入無法處理請求的快取。 使用AEM時，預計CDN和AEM Dispatcher快取命中率將大於90%。
1. 此網頁包含的JavaScript可對AEM Publish服務中的自訂FPID servlet發出不可快取的非同步XHR (AJAX)請求。 由於這是無法快取的要求（因其隨機查詢引數和快取控制標頭），CDN或AEM Dispatcher永遠不會快取，且一律會送達AEM Publish服務以產生回應。
1. AEM Publish服務中的自訂FPID servlet會處理請求、在找不到現有FPID Cookie時產生新的FPID，或延長任何現有FPID Cookie的生命週期。 此servlet也會傳回回應本文中的FPID，以供使用者端JavaScript使用。 幸運的是，自訂FPID servlet邏輯是輕量級的，防止此請求影響AEM Publish服務效能。
1. XHR要求的回應會傳回瀏覽器，並在回應本文中以JSON格式顯示FPID Cookie和FPID，以供Platform Web SDK使用。

## 程式碼範例

下列程式碼和設定可部署至AEM Publish服務，以建立端點，產生或延長現有FPID Cookie的生命週期，並將FPID傳回JSON。

### AEM FPID Cookie servlet

必須建立AEM HTTP端點，才能使用產生或擴充FPID Cookie [Sling servlet](https://sling.apache.org/documentation/the-sling-engine/servlets.html#registering-a-servlet-using-java-annotations-1).

+ 此servlet繫結到 `/bin/aem/fpid` 因為存取它不需要驗證。 如果需要驗證，則繫結到Sling資源型別。
+ 此servlet接受HTTPGET要求。 回應會標示為 `Cache-Control: no-store` 以防止快取，但亦應使用唯一的cache-busting查詢引數來請求此端點。

當HTTP要求到達servlet時，此servlet會檢查要求上是否存在FPID Cookie：

+ 如果存在FPID Cookie，請延長Cookie的生命週期，並收集其值以寫入回應。
+ 如果FPID Cookie不存在，請產生新的FPID Cookie，並儲存值以寫入回應。

此servlet接著會將FPID以JSON物件的形式寫入回應，格式如下： `{ fpid: "<FPID VALUE>" }`.

請務必在正文中將FPID提供給使用者端，因為FPID Cookie已標籤 `HttpOnly`，表示只有伺服器可讀取其值，使用者端JavaScript則無法讀取。

回應本文中的FPID值可用來引數化Platform Web SDK的呼叫。

以下是AEM servlet端點的範常式式碼(可透過 `HTTP GET /bin/aep/fpid`)會產生或重新整理FPID Cookie，並將FPID傳回JSON。

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

### HTML指令碼

必須新增自訂使用者端JavaScript至頁面才能非同步叫用servlet，產生或重新整理FPID Cookie並在回應中傳回FPID。

此JavaScript指令碼通常會新增至頁面使用下列其中一種方法：

+ [Adobe Experience Platform中的標籤](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [AEM使用者端資源庫](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/clientlibs.html?lang=en)

對自訂AEM FPID servlet的XHR呼叫雖然不同步，但速度很快，因此使用者可以造訪AEM提供的網頁，並在完成請求之前導覽離開。
如果發生這種情況，相同的程式將在從AEM載入網頁的下一頁時重新嘗試。

AEM FPID servlet的HTTPGET(`/bin/aep/fpid`)以隨機查詢引數引數引數化，以確保瀏覽器和AEM Publish服務之間的任何基礎結構都不會快取請求的回應。
同樣地， `Cache-Control: no-store` 新增請求標頭以支援避免快取。

在叫用AEM FPID servlet時，會從JSON回應中擷取FPID，並由 [平台Web SDK](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/tags-configuration/install-web-sdk.html?lang=en) 以傳送至Experience Platform API。

請參閱Experience Platform檔案以取得以下專案的詳細資訊： [在identityMap中使用FPID](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/first-party-device-ids.html#identityMap)

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

### Dispatcher允許篩選器

最後，必須透過AEM Dispatcher允許對自訂FPID servlet的HTTPGET請求 `filter.any` 設定。

如果此Dispatcher設定未正確實作，HTTPGET會要求 `/bin/aep/fpid` 結果為404。

+ `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
/1099 { /type "allow" /method "GET" /url "/bin/aep/fpid" }
```

## Experience Platform資源

檢閱下列有關第一方裝置ID (FPID)和使用Platform Web SDK管理身分資料的Experience Platform檔案。

+ [產生第一方裝置識別碼](https://experienceleague.adobe.com/docs/platform-learn/data-collection/edge-network/generate-first-party-device-ids.html)
+ [Platform Web SDK中的第一方裝置ID](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/first-party-device-ids.html)
+ [Platform Web SDK中的身分資料](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html)
