---
title: 来自AEM的双向传输层安全性(mTLS)身份验证
description: 了解如何从AEM对需要相互传输层安全性(mTLS)身份验证的Web API进行HTTPS调用。
feature: Security
version: Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Security, Development
role: Admin, Architect, Developer
level: Experienced
jira: KT-13881
thumbnail: KT-13881.png
doc-type: Article
last-substantial-update: 2023-10-10T00:00:00Z
exl-id: 7238f091-4101-40b5-81d9-87b4d57ccdb2
duration: 495
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '731'
ht-degree: 0%

---

# 来自AEM的双向传输层安全性(mTLS)身份验证

了解如何从AEM对需要相互传输层安全性(mTLS)身份验证的Web API进行HTTPS调用。

>[!VIDEO](https://video.tv.adobe.com/v/3447869?quality=12&learn=on&captions=chi_hans)

mTLS或双向TLS身份验证要求&#x200B;**客户端和服务器都相互进行身份验证**，从而增强了TLS协议的安全性。 此身份验证通过使用数字证书来完成。 它通常用于高度安全性和身份验证至关重要的场景。

默认情况下，在尝试与需要mTLS身份验证的Web API建立HTTPS连接时，连接会失败，并出现以下错误：

```
javax.net.ssl.SSLHandshakeException: Received fatal alert: certificate_required
```

当客户端不提供证书来验证自身时，会出现此问题。

让我们了解如何使用[Apache HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html)和&#x200B;**AEM的KeyStore和TrustStore**&#x200B;成功调用需要mTLS身份验证的API。


## HttpClient和加载AEM KeyStore资料

从较高层面来看，需要执行以下步骤才能从AEM中调用受mTLS保护的API。

### AEM证书生成

与贵组织的安全团队合作，请求AEM证书。 安全团队提供或询问与证书相关的详细信息，例如密钥、证书签名请求(CSR)，并使用CSR颁发证书。

出于演示目的，请生成与证书相关的详细信息，例如密钥、证书签名请求(CSR)。 在以下示例中，自签名CA用于颁发证书。

- 首先，生成内部证书颁发机构(CA)证书。

  ```shell
  # Create an internal Certification Authority (CA) certificate
  openssl req -new -x509 -days 9999 -keyout internal-ca-key.pem -out internal-ca-cert.pem
  ```

- 生成AEM证书。

  ```shell
  # Generate Key
  openssl genrsa -out client-key.pem
  
  # Generate CSR
  openssl req -new -key client-key.pem -out client-csr.pem
  
  # Generate certificate and sign with internal Certification Authority (CA)
  openssl x509 -req -days 9999 -in client-csr.pem -CA internal-ca-cert.pem -CAkey internal-ca-key.pem -CAcreateserial -out client-cert.pem
  
  # Verify certificate
  openssl verify -CAfile internal-ca-cert.pem client-cert.pem
  ```

- 将AEM私钥转换为DER格式，AEM的KeyStore需要采用DER格式的私钥。

  ```shell
  openssl pkcs8 -topk8 -inform PEM -outform DER -in client-key.pem -out client-key.der -nocrypt
  ```

>[!TIP]
>
>自签名CA证书仅用于开发目的。 对于生产，请使用受信任的证书颁发机构(CA)来颁发证书。


### 证书交换

如果对AEM证书使用自签名CA（如上所述），请将证书或内部证书颁发机构(CA)证书发送到API提供程序。

此外，如果API提供商使用自签名CA证书，则从API提供商接收证书或内部证书颁发机构(CA)证书。

### 证书导入

要导入AEM的证书，请执行以下步骤：

1. 以&#x200B;**管理员**&#x200B;身份登录到&#x200B;**AEM作者**。

1. 导航到&#x200B;**AEM作者>工具>安全>用户>创建或选择现有用户**。

   ![创建或选择现有用户](assets/mutual-tls-authentication/create-or-select-user.png)

   为演示目的，创建了名为`mtl-demo-user`的新用户。

1. 要打开&#x200B;**用户属性**，请单击用户名。

1. 单击&#x200B;**密钥库**&#x200B;选项卡，然后单击&#x200B;**创建密钥库**&#x200B;按钮。 然后在&#x200B;**设置KeyStore访问密码**&#x200B;对话框中，为此用户的密钥库设置密码，然后单击“保存”。

   ![创建密钥库](assets/mutual-tls-authentication/create-keystore.png)

1. 在新屏幕的&#x200B;**从DER文件添加私钥**&#x200B;部分下，执行以下步骤：

   1. 输入别名

   1. 以DER格式导入以上生成的AEM私钥。

   1. 导入上面生成的证书链文件。

   1. 单击提交

      ![导入AEM私钥](assets/mutual-tls-authentication/import-aem-private-key.png)

1. 验证是否已成功导入证书。

   ![AEM私钥和证书已导入](assets/mutual-tls-authentication/aem-privatekey-cert-imported.png)

如果API提供程序正在使用自签名CA证书，请将收到的证书导入AEM的TrustStore，请按照[此处](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/call-internal-apis-having-private-certificate.html?lang=zh-Hans#httpclient-and-load-aem-truststore-material)中的步骤操作。

同样，如果AEM使用自签名CA证书，请请求API提供商导入它。

### 使用HttpClient的典型mTLS API调用代码

更新Java™代码，如下所示。 要使用`@Reference`注释来获取AEM的`KeyStoreService`服务，调用代码必须是OSGi组件/服务或Sling模型（并在其中使用`@OsgiService`）。


```java
...

// Get AEM's KeyStoreService reference
@Reference
private com.adobe.granite.keystore.KeyStoreService keyStoreService;

...

// Get AEM KeyStore using KeyStoreService
KeyStore aemKeyStore = getAEMKeyStore(keyStoreService, resourceResolver);

if (aemKeyStore != null) {

    // Create SSL Context
    SSLContextBuilder sslbuilder = new SSLContextBuilder();

    // Load AEM KeyStore material into above SSL Context with keystore password
    // Ideally password should be encrypted and stored in OSGi config
    sslbuilder.loadKeyMaterial(aemKeyStore, "admin".toCharArray());

    // If API provider cert is self-signed, load AEM TrustStore material into above SSL Context
    // Get AEM TrustStore
    KeyStore aemTrustStore = getAEMTrustStore(keyStoreService, resourceResolver);
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
    closeableHttpResponse = httpClient.execute(new HttpGet(MTLS_API_ENDPOINT));

    // Code that reads response code and body from the 'closeableHttpResponse' object
    ...
} 

/**
 * Returns the AEM KeyStore of a user. In this example we are using the
 * 'mtl-demo-user' user.
 * 
 * @param keyStoreService
 * @param resourceResolver
 * @return AEM KeyStore
 */
private KeyStore getAEMKeyStore(KeyStoreService keyStoreService, ResourceResolver resourceResolver) {

    // get AEM KeyStore of 'mtl-demo-user' user, you can create a user or use an existing one. 
    // Then create keystore and upload key, certificate files.
    KeyStore aemKeyStore = keyStoreService.getKeyStore(resourceResolver, "mtl-demo-user");

    return aemKeyStore;
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

- 将OOTB `com.adobe.granite.keystore.KeyStoreService` OSGi服务注入您的OSGi组件。
- 使用`KeyStoreService`和`ResourceResolver`获取用户的AEM KeyStore，`getAEMKeyStore(...)`方法可做到这一点。
- 如果API提供程序使用自签名CA证书，则获取全局AEM TrustStore，`getAEMTrustStore(...)`方法会执行此操作。
- 创建`SSLContextBuilder`的对象，请参阅Java™ [API详细信息](https://javadoc.io/static/org.apache.httpcomponents/httpcore/4.4.8/index.html?org/apache/http/ssl/SSLContextBuilder.html)。
- 使用`loadKeyMaterial(final KeyStore keystore,final char[] keyPassword)`方法将用户的AEM KeyStore加载到`SSLContextBuilder`中。
- 密钥库密码是在创建密钥库时设置的密码，应存储在OSGi配置中，请参阅[密码配置值](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html?lang=zh-Hans#secret-configuration-values)。

## 避免JVM密钥库更改

使用私有证书有效调用mTLS API的传统方法涉及修改JVM密钥库。 这是通过使用Java™ [keytool](https://docs.oracle.com/en/java/javase/11/tools/keytool.html#GUID-5990A2E4-78E3-47B7-AE75-6D1826259549)命令导入私有证书来实现的。

但是，此方法不符合安全最佳实践，AEM通过使用&#x200B;**用户特定的KeyStore和全局TrustStore**&#x200B;和[KeyStoreService](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/com/adobe/granite/keystore/KeyStoreService.html)提供了卓越的选项。

## 解决方案包

可从[此处](assets/internal-api-call/REST-APIs.zip)下载视频中降级的Node.js示例项目。

AEM servlet代码在WKND站点项目的`tutorial/web-api-invocation`分支[中可用，请参见](https://github.com/adobe/aem-guides-wknd/tree/tutorial/web-api-invocation/core/src/main/java/com/adobe/aem/guides/wknd/core/servlets)。
