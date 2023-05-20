---
title: 專用輸出IP位址和VPN的HTTP/HTTPS連線
description: 瞭解如何讓專用輸出IP位址和VPN執行的外部Web服務從AEMas a Cloud Service發出HTTP/HTTPS請求
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9354
thumbnail: KT-9354.jpeg
exl-id: a565bc3a-675f-4d5e-b83b-c14ad70a800b
source-git-commit: bdce84fdcc949c8f8d0690ee7110238d8e8d3e42
workflow-type: tm+mt
source-wordcount: '233'
ht-degree: 0%

---

# 專用輸出IP位址和VPN的HTTP/HTTPS連線

HTTP/HTTPS連線會自動使用專用輸出IP位址或VPN從AEMas a Cloud Service代理，並且不需要任何特殊的 `portForwards` 規則。

## 進階網路支援

下列進階網路選項支援下列程式碼範例。

確保 [專用輸出IP位址或VPN](../advanced-networking.md#advanced-networking) 在執行本教學課程之前，已設定進階網路設定。

| 無進階網路 | [彈性的連線埠輸出](../flexible-port-egress.md) | [專用輸出IP位址](../dedicated-egress-ip-address.md) | [虛擬私人網路](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✘ | ✔ | ✔ |

>[!CAUTION]
>
> 此程式碼範例僅適用於 [專用輸出IP位址](../dedicated-egress-ip-address.md) 和 [VPN](../vpn.md). 相似但不同的程式碼範例適用於 [非標準連線埠上的HTTP/HTTPS連線，用於彈性連線埠輸出](./http-on-non-standard-ports-flexible-port-egress.md).

## 程式碼範例

此Java™程式碼範例屬於可在AEMas a Cloud Service中執行的OSGi服務，該服務會建立8080上外部Web伺服器的HTTP連線。 HTTPS （或HTTP）連線會自動從AEMas a Cloud Service代理出來，不需要特別開發。

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
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.time.Duration;

@Component
public class HttpExternalServiceImpl implements ExternalService {
    private static final Logger log = LoggerFactory.getLogger(HttpExternalServiceImpl.class);

    // client with connection pool reused for all requests
    private HttpClient client = HttpClient.newBuilder().build();

    @Override
    public boolean isAccessible() {

        // Prepare the full URI to request, note this will have the
        // - Scheme (http/https)
        // - External host name
        // - External port
        // The external service URI, including the scheme/host/port, is defined in code, rather than in Cloud Manager portForwards rules.
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
