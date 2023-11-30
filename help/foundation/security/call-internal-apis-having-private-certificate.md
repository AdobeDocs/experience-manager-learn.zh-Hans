---
title: 调用具有私有证书的内部API
description: 了解如何调用具有私有证书或自签名证书的内部API。
feature: Security
version: 6.5, Cloud Service
topic: Security, Development
role: Admin, Architect, Developer
level: Experienced
jira: KT-11548
thumbnail: KT-11548.png
doc-type: Article
last-substantial-update: 2023-08-25T00:00:00Z
exl-id: c88aa724-9680-450a-9fe8-96e14c0c6643
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '530'
ht-degree: 0%

---

# 调用具有私有证书的内部API

了解如何使用专用证书或自签名证书从AEM向Web API进行HTTPS调用。

>[!VIDEO](https://video.tv.adobe.com/v/3424853?quality=12&learn=on)

默认情况下，在尝试与使用自签名证书的Web API建立HTTPS连接时，连接会失败，并出现以下错误：

```
PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target
```

此问题通常发生在 **API的SSL证书不是由可识别的证书颁发机构(CA)颁发的** 和Java™应用程序无法验证SSL/TLS证书。

让我们了解如何使用成功调用具有私有证书或自签名证书的API [Apache HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html) 和 **AEM全局TrustStore**.


## 使用HttpClient的典型API调用代码

以下代码创建到Web API的HTTPS连接：

```java
...
String API_ENDPOINT = "https://example.com";

// Create HttpClientBuilder
HttpClientBuilder httpClientBuilder = HttpClientBuilder.create();

// Create HttpClient
CloseableHttpClient httpClient = httpClientBuilder.build();

// Invoke API
CloseableHttpResponse closeableHttpResponse = httpClient.execute(new HttpGet(API_ENDPOINT));

// Code that reads response code and body from the 'closeableHttpResponse' object
...
```

代码使用 [Apache Http组件](https://hc.apache.org/)的 [HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html) 库类及其方法。


## HttpClient和加载AEM TrustStore资料

调用API端点，该API端点具有 _私有证书或自签名证书_， [HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html)的 `SSLContextBuilder` 必须使用AEM TrustStore加载，并且用于促进连接。

请按照以下步骤操作：

1. 登录 **AEM创作** 作为 **管理员**.
1. 导航到 **AEM Author >工具>安全>信任存储区**，然后打开 **全局信任存储区**. 如果首次访问，请为全局信任存储区设置密码。

   ![全局信任存储区](assets/internal-api-call/global-trust-store.png)

1. 要导入专用证书，请单击 **选择证书文件** 按钮并选择所需的证书文件 `.cer` 扩展。 通过单击导入它 **提交** 按钮。

1. 更新Java™代码，如下所示。 请注意，要使用 `@Reference` 获取AEM `KeyStoreService` 调用代码必须是OSGi组件/服务或Sling模型(以及 `@OsgiService` 在该处使用)。

   ```java
   ...
   
   // Get AEM's KeyStoreService reference
   @Reference
   private com.adobe.granite.keystore.KeyStoreService keyStoreService;
   
   ...
   
   // Get AEM TrustStore using KeyStoreService
   KeyStore aemTrustStore = getAEMTrustStore(keyStoreService, resourceResolver);
   
   if (aemTrustStore != null) {
   
       // Create SSL Context
       SSLContextBuilder sslbuilder = new SSLContextBuilder();
   
       // Load AEM TrustStore material into above SSL Context
       sslbuilder.loadTrustMaterial(aemTrustStore, null);
   
       // Create SSL Connection Socket using above SSL Context
       SSLConnectionSocketFactory sslsf = new SSLConnectionSocketFactory(
               sslbuilder.build(), NoopHostnameVerifier.INSTANCE);
   
       // Create HttpClientBuilder
       HttpClientBuilder httpClientBuilder = HttpClientBuilder.create();
       httpClientBuilder.setSSLSocketFactory(sslsf);
   
       // Create HttpClient
       CloseableHttpClient httpClient = httpClientBuilder.build();
   
       // Invoke API
       closeableHttpResponse = httpClient.execute(new HttpGet(API_ENDPOINT));
   
       // Code that reads response code and body from the 'closeableHttpResponse' object
       ...
   } 
   
   /**
    * 
    * Returns the global AEM TrustStore
    * 
    * @param keyStoreService OOTB OSGi service that makes AEM based KeyStore
    *                         operations easy.
    * @param resourceResolver
    * @return
    */
   private KeyStore getAEMTrustStore(KeyStoreService keyStoreService, ResourceResolver resourceResolver) {
   
       // get AEM TrustStore from the KeyStoreService and ResourceResolver
       KeyStore aemTrustStore = keyStoreService.getTrustStore(resourceResolver);
   
       return aemTrustStore;
   }
   
   ...
   ```

   * 插入OOTB `com.adobe.granite.keystore.KeyStoreService` 将OSGi服务插入您的OSGi组件中。
   * 使用获取全局AEM TrustStore `KeyStoreService` 和 `ResourceResolver`， `getAEMTrustStore(...)` 方法可做到这一点。
   * 创建对象 `SSLContextBuilder`，请参阅Java™ [API详细信息](https://javadoc.io/static/org.apache.httpcomponents/httpcore/4.4.8/index.html?org/apache/http/ssl/SSLContextBuilder.html).
   * 将全局AEM TrustStore加载到 `SSLContextBuilder` 使用 `loadTrustMaterial(KeyStore truststore,TrustStrategy trustStrategy)` 方法。
   * 通过 `null` 对象 `TrustStrategy` 在上述方法中，它确保在API执行期间只有AEM可信证书成功。


>[!CAUTION]
>
>使用上述方法执行时，具有有效CA颁发证书的API调用失败。 在遵循此方法时，只允许使用AEM受信任证书的API调用成功。
>
>使用 [标准方法](#prototypical-api-invocation-code-using-httpclient) 用于执行有效CA颁发的证书的API调用，这意味着应仅使用上述方法执行与私有证书关联的API。

## 避免JVM密钥库更改

使用私有证书有效调用内部API的传统方法涉及修改JVM密钥库。 通过使用Java导入私有证书来实现这一点™ [keytool](https://docs.oracle.com/en/java/javase/11/tools/keytool.html#GUID-5990A2E4-78E3-47B7-AE75-6D1826259549) 命令。

但是，此方法不符合安全最佳实践，AEM通过利用 **全局信任存储区** 和 [KeyStoreService](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/com/adobe/granite/keystore/KeyStoreService.html).


## 解决方案包

可以从下载视频中降级的Node.js示例项目 [此处](assets/internal-api-call/REST-APIs.zip).

AEM servlet代码在WKND站点项目的 `tutorial/web-api-invocation` 分支， [请参阅](https://github.com/adobe/aem-guides-wknd/tree/tutorial/web-api-invocation/core/src/main/java/com/adobe/aem/guides/wknd/core/servlets).
