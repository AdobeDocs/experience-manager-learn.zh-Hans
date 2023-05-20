---
title: AEM GraphQL的CORS設定
description: 瞭解如何設定跨原始資源共用(CORS)以搭配AEM GraphQL使用。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10830
thumbnail: KT-10830.jpg
exl-id: 394792e4-59c8-43c1-914e-a92cdfde2f8a
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '619'
ht-degree: 1%

---

# 跨原始資源共用(CORS)

Adobe Experience Manager as a Cloud Service的跨原始資源共用(CORS)有助於非AEM Web屬性對AEM GraphQL API進行瀏覽器型使用者端呼叫。

以下文章說明如何設定 _單一來源_ 透過CORS存取一組特定的AEM Headless端點。 單一來源表示只有單一非AEM網域會存取AEM，例如https://app.example.com連線到https://www.example.com。 由於快取疑慮，使用此方法時可能無法使用多來源存取。

>[!TIP]
>
> 以下設定為範例。 請確定您調整這些值，以符合專案的要求。

## CORS需求

當連線到AEM GraphQL API的使用者端並非從與AEM相同的來源（也稱為主機或網域）提供服務時，瀏覽器型連線至AEM API需要CORS。

| 使用者端型別 | [單頁應用程式(SPA)](../spa.md) | [Web元件/JS](../web-component.md) | [移动设备](../mobile.md) | [伺服器對伺服器](../server-to-server.md) |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| 需要CORS設定 | ✔ | ✔ | ✘ | ✘ |

## OSGi設定

AEM CORS OSGi Configuration Factory會定義接受CORS HTTP要求的允許條件。

| 使用者端連線至 | AEM Author | AEM 发布 | AEM預覽 |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| 需要CORS OSGi設定 | ✔ | ✔ | ✔ |


以下範例定義AEM Publish的OSGi設定(`../config.publish/..`)，但可新增至 [任何支援的執行模式資料夾](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#runmode-resolution).

主要組態屬性為：

+ `alloworigin` 和/或 `alloworiginregexp` 指定連線至AEM Web執行之使用者端的來源。
+ `allowedpaths` 指定允許來自指定來源的URL路徑模式。
   + 若要支援AEM GraphQL持續查詢，請新增以下模式： `/graphql/execute.json.*`
   + 若要支援體驗片段，請新增以下模式： `/content/experience-fragments/.*`
+ `supportedmethods` 指定CORS要求允許的HTTP方法。 新增 `GET`，以支援AEM GraphQL持續查詢（和體驗片段）。

[進一步瞭解CORS OSGi設定。](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html)

此設定範例支援使用AEM GraphQL持續查詢。 若要使用使用者端定義的GraphQL查詢，請在中新增GraphQL端點URL `allowedpaths` 和 `POST` 至 `supportedmethods`.

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

```json
{
  "alloworigin":[
    "https://spa.external.com/"
  ],
  "alloworiginregexp":[
  ],
  "allowedpaths": [
    "/graphql/execute.json.*",
    "/content/experience-fragments/.*"
  ],
  "supportedheaders": [
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers"
  ],
  "supportedmethods":[
    "GET",
    "HEAD",
    "OPTIONS"
  ],
  "maxage:Integer": 1800,
  "supportscredentials": false,
  "exposedheaders":[ "" ]
}
```

### 授權的AEM GraphQL API請求

存取需要授權的AEM GraphQL API時（通常是AEM Author或AEM Publish上的受保護內容），請確保CORS OSGi設定有其他值：

+ `supportedheaders` 另清單 `"Authorization"`
+ `supportscredentials` 設為 `true`

AEM GraphQL API的授權請求需要CORS設定並不常見，因為它們通常出現在以下情境中 [伺服器對伺服器應用程式](../server-to-server.md) 因此，不需要CORS設定。 需要CORS設定的瀏覽器型應用程式，例如 [單頁應用程式](../spa.md) 或 [Web元件](../web-component.md)通常使用授權，因為很難保護認證。

例如，這兩個設定的設定如下 `CORSPolicyImpl` OSGi工廠設定：

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

```json
{ 
  ...
  "supportedheaders": [
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers",
    "Authorization"
  ],
  ...
  "supportscredentials": true,
  ...
}
```

#### 範例OSGi設定

+ [您可以在WKND專案中找到OSGi設定的範例。](https://github.com/adobe/aem-guides-wknd/blob/main/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json)

## Dispatcher 配置

AEM Publish （和Preview）服務的Dispatcher必須設定為支援CORS。

| 使用者端連線至 | AEM Author | AEM 发布 | AEM預覽 |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| 需要Dispatcher CORS設定 | ✘ | ✔ | ✔ |

### 允許HTTP請求上的CORS標頭

更新 `clientheaders.any` 檔案以允許HTTP請求標頭 `Origin`，  `Access-Control-Request-Method`、和 `Access-Control-Request-Headers` 傳遞至AEM，讓HTTP請求可由 [AEM CORS設定](#osgi-configuration).

`dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any`

```
# Allowing CORS headers to be passed through to the publish tier to support headless and SPA Editor use cases.
# https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_request_headers

"Origin"
"Access-Control-Request-Method"
"Access-Control-Request-Headers"

$include "./default_clientheaders.any"
```

#### Dispatcher設定範例

+ [Dispatcher的範例 _使用者端標頭_ 可在WKND專案中找到設定。](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any#L10-L12)


### 傳遞CORS HTTP回應標頭

設定Dispatcher陣列以快取 **CORS HTTP回應標題** 為了確保在Dispatcher快取中提供AEM GraphQL持續查詢時包含在內，請新增 `Access-Control-...` 快取標頭清單的標頭。

+ `dispatcher/src/conf.dispatcher.d/available_farms/wknd.farm`

```
/publishfarm {
    ...
    /cache {
        ...
        # CORS HTTP response headers
        # https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_response_headers
        /headers {
            ...
            "Access-Control-Allow-Origin"
            "Access-Control-Expose-Headers"
            "Access-Control-Max-Age"
            "Access-Control-Allow-Credentials"
            "Access-Control-Allow-Methods"
            "Access-Control-Allow-Headers"
        }
    ...
    }
...
}
```

#### Dispatcher設定範例

+ [Dispatcher的範例 _CORS HTTP回應標題_ 可在WKND專案中找到設定。](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/available_farms/wknd.farm#L109-L114)
