---
title: 专用出口IP地址和VPN的HTTP/HTTPS连接
description: 了解如何使专用出口IP地址和VPN运行的AEM对外部Web服务发出HTTP/HTTPS请求as a Cloud Service
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

# 专用出口IP地址和VPN的HTTP/HTTPS连接

HTTP/HTTPS连接会自动用专用出口IP地址或VPN代理出AEMas a Cloud Service，并且它们不需要任何特殊的 `portForwards` 规则。

## 高级联网支持

以下高级联网选项支持以下代码示例。

确保 [专用出口IP地址或VPN](../advanced-networking.md#advanced-networking) 在执行本教程之前，已设置高级联网配置。

| 无高级联网 | [灵活端口出口](../flexible-port-egress.md) | [专用出口IP地址](../dedicated-egress-ip-address.md) | [虚拟专用网络](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✘ | ✔ | ✔ |

>[!CAUTION]
>
> 此代码示例仅用于 [专用出口IP地址](../dedicated-egress-ip-address.md) 和 [VPN](../vpn.md). 有一个相似但不同的代码示例可用于 [非标准端口上的HTTP/HTTPS连接，用于灵活端口出口](./http-on-non-standard-ports-flexible-port-egress.md).

## 代码示例

此Java™代码示例是一个可在AEMas a Cloud Service中运行的OSGi服务，该服务通过HTTP连接到8080上的外部Web服务器。 HTTPS（或HTTP）连接会自动通过AEMas a Cloud Service代理，而无需特殊开发。

>[!NOTE]
> 建议使用 [Java™ 11 HTTP API](https://docs.oracle.com/en/java/javase/11/docs/api/java.net.http/java/net/http/package-summary.html) 用于从AEM进行HTTP/HTTPS调用。

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
