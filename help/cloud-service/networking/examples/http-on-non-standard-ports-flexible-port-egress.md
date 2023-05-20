---
title: 非標準連線埠上的HTTP/HTTPS連線，用於彈性連線埠輸出
description: 瞭解如何將AEM的HTTP/HTTPS請求變成as a Cloud Service的，以在彈性連線埠輸出的非標準連線埠上執行的外部Web服務。
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9354
thumbnail: KT-9354.jpeg
exl-id: c8cc0385-9e94-4120-9fb1-aeccbfcc8aa4
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '239'
ht-degree: 0%

---

# 非標準連線埠上的HTTP/HTTPS連線，用於彈性連線埠輸出

非標準連線埠（非80/443）上的HTTP/HTTPS連線必須以AEMas a Cloud Service代理，但是它們不需要任何特殊的 `portForwards` 規則，並可使用AEM進階網路的 `AEM_PROXY_HOST` 和保留的Proxy連線埠 `AEM_HTTP_PROXY_PORT` 或 `AEM_HTTPS_PROXY_PORT` 視目的地為HTTP/HTTPS而定。

## 進階網路支援

下列進階網路選項支援下列程式碼範例。

確保 [適當的](../advanced-networking.md#advanced-networking) 在執行本教學課程之前，已設定進階網路設定。

| 無進階網路 | [彈性的連線埠輸出](../flexible-port-egress.md) | [專用輸出IP位址](../dedicated-egress-ip-address.md) | [虛擬私人網路](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✘ | ✘ |

>[!CAUTION]
>
> 此程式碼範例僅適用於 [彈性連線埠輸出](../flexible-port-egress.md). 相似但不同的程式碼範例適用於 [專用輸出IP位址和VPN的非標準連線埠上的HTTP/HTTPS連線](./http-dedicated-egress-ip-vpn.md).

## 程式碼範例

此Java™程式碼範例屬於可在AEMas a Cloud Service中執行的OSGi服務，該服務會建立8080上外部Web伺服器的HTTP連線。 與HTTPS Web伺服器的連線會使用環境變數 `AEM_PROXY_HOST` 和 `AEM_HTTPS_PROXY_PORT` (預設為 `proxy.tunnel:3128` (在AEM版本&lt; 6094中)。

>[!NOTE]
> 建議使用 [Java™ 11 HTTP API](https://docs.oracle.com/en/java/javase/11/docs/api/java.net.http/java/net/http/package-summary.html) 用於從AEM進行HTTP/HTTPS呼叫。

+ `core/src/com/adobe/aem/wknd/examples/connections/impl/HttpExternalServiceImpl.java`

```java
package com.adobe.aem.wknd.examples.core.connections.impl;

import com.adobe.aem.wknd.examples.core.connections.ExternalService;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.io.IOException;
import java.net.InetSocketAddress;
import java.net.ProxySelector;
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.time.Duration;

@Component
public class HttpExternalServiceImpl implements ExternalService {
    private static final Logger log = LoggerFactory.getLogger(HttpExternalServiceImpl.class);

    @Override
    public boolean isAccessible() {
        HttpClient client;

        // Use System.getenv("AEM_PROXY_HOST") and proxy port System.getenv("AEM_HTTP_PROXY_PORT") 
        // or System.getenv("AEM_HTTPS_PROXY_PORT"), depending on if the destination requires HTTP/HTTPS

        if (System.getenv("AEM_PROXY_HOST") != null) {
            // Create a ProxySelector that uses to AEM's provided AEM_PROXY_HOST, with a fallback of proxy.tunnel, and proxy port using the AEM_HTTP_PROXY_PORT variable. 
            // If the destination requires HTTPS, then use the variable AEM_HTTPS_PROXY_PORT instead of AEM_HTTP_PROXY_PORT.
 
            ProxySelector proxySelector = ProxySelector.of(new InetSocketAddress(
                System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel"), 
                Integer.parseInt(System.getenv().get("AEM_HTTP_PROXY_PORT"))));

            client = HttpClient.newBuilder().proxy(proxySelector).build();
            log.debug("Using HTTPClient with AEM_PROXY_HOST");
        } else {
            client = HttpClient.newBuilder().build();
            // If no proxy is set up (such as local dev)
            log.debug("Using HTTPClient without AEM_PROXY_HOST");
        }

        // Prepare the full URI to request, note this will have the
        // - Scheme (http/https)
        // - External host name
        // - External port
        // The external service URI, including the scheme/host/port, is defined in code, and NOT in Cloud Manager portForwards rules.
        URI uri = URI.create("http://api.example.com:8080/test.json");

        // Prepare the HttpRequest
        HttpRequest request = HttpRequest.newBuilder().uri(uri).timeout(Duration.ofSeconds(2)).build();

        // Send the HttpRequest using the configured HttpClient
        HttpResponse<String> response = null;
        try {
            // Request the URL
            response = client.send(request, HttpResponse.BodyHandlers.ofString());

            log.debug("HTTP response body: {} ", response.body());

            // Our simple example returns true is response is successful! (200 status code)
            return response.statusCode() == 200;
        } catch (IOException e) {
            return false;
        } catch (InterruptedException e) {
            return false;
        }
    }
}
```
