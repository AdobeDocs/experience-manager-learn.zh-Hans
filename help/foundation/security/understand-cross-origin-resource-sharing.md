---
title: 瞭解與AEM的跨原始資源共用(CORS)
description: Adobe Experience Manager的跨原始資源共用(CORS)有助於非AEM Web屬性對AEM發出使用者端呼叫（包括已驗證和未驗證），以擷取內容或直接與AEM互動。
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
topics: security, development, content-delivery
activity: understand
audience: architect, developer
doc-type: article
topic: Security
role: Developer
level: Intermediate
exl-id: 6009d9cf-8aeb-4092-9e8c-e2e6eec46435
source-git-commit: 2bd1b66dc28a6e591afda746e9d276cae7a29948
workflow-type: tm+mt
source-wordcount: '913'
ht-degree: 1%

---

# 瞭解跨原始資源共用([!DNL CORS])

Adobe Experience Manager的跨原始資源共用([!DNL CORS])有助於非AEM網頁屬性對AEM發出使用者端呼叫（包括已驗證和未驗證），以擷取內容或直接與AEM互動。

## AdobeGranite跨原始資源共用原則OSGi設定

CORS設定在AEM中作為OSGi設定處理站進行管理，每個原則都會表示為處理站的一個執行個體。

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![AdobeGranite跨原始資源共用原則OSGi設定](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### 原則選擇

透過比較以下專案來選取原則

* `Allowed Origin` 使用 `Origin` 請求標頭
* 和 `Allowed Paths` 要求路徑。

會使用符合這些值的第一個原則。 如果未找到，則任何 [!DNL CORS] 要求被拒絕。

如果完全沒有設定原則， [!DNL CORS] 由於處理常式已停用，因此只要伺服器的其他模組沒有回應，請求也不會得到回應 [!DNL CORS].

### 原則屬性

#### [!UICONTROL 允許的原始項]

* `"alloworigin" <origin> | *`
* 以下專案清單： `origin` 指定可存取資源的URI的引數。 對於沒有認證的請求，伺服器可以指定 &#42; 作為萬用字元，因此允許任何來源存取資源。 *絕對不建議使用 `Allow-Origin: *` 允許所有外國（即攻擊者）網站提出瀏覽器嚴格禁止不含CORS的請求，因此不會出現在生產環境中。*

#### [!UICONTROL 允許的原始項(Regexp)]

* `"alloworiginregexp" <regexp>`
* 以下專案清單： `regexp` 規則運算式，指定可存取資源的URI。 *如果不小心建置，規則運算式可能會導致意外的相符專案，讓攻擊者使用也會符合原則的自訂網域名稱。* 通常建議針對每個特定來源主機名稱使用不同原則， `alloworigin`，即使這表示會重複設定其他原則屬性。 不同的起源往往有不同的生命週期和要求，因此受益於清晰的區分。

#### [!UICONTROL 允许的路径]

* `"allowedpaths" <regexp>`
* 以下專案清單： `regexp` 規則運算式，指定套用原則的資源路徑。

#### [!UICONTROL 公開的標頭]

* `"exposedheaders" <header>`
* 標頭引數清單，指示允許瀏覽器存取的請求標頭。

#### [!UICONTROL 最大年齡]

* `"maxage" <seconds>`
* A `seconds` 表示可快取預檢要求結果的時間的引數。

#### [!UICONTROL 支援的標頭]

* `"supportedheaders" <header>`
* 以下專案清單： `header` 表示提出實際請求時可以使用哪些HTTP標頭的引數。

#### [!UICONTROL 允許的方法]

* `"supportedmethods"`
* 方法引數清單，指出提出實際請求時可以使用哪些HTTP方法。

#### [!UICONTROL 支援認證]

* `"supportscredentials" <boolean>`
* A `boolean` 指示對請求的回應是否可向瀏覽器公開。 當用作對預檢請求的回應的一部分時，這表示是否可以使用憑證提出實際請求。

### 設定範例

網站1是基本、匿名存取的唯讀案例，透過以下方式使用內容： [!DNL GET] 要求：

```json
{
  "supportscredentials":false,
  "exposedheaders":[
    ""
  ],
  "supportedmethods":[
    "GET",
    "HEAD",
    "OPTIONS"
  ],
  "alloworigin":[
    "http://127.0.0.1:3000",
    "https://site1.com"
    
  ],
  "maxage:Integer": 1800,
  "alloworiginregexp":[
    "http://localhost:.*"
    "https://.*\.site1\.com"
  ],
  "allowedpaths":[
    "/content/_cq_graphql/site1/endpoint.json",
    "/graphql/execute.json.*",
    "/content/site1/.*"
  ],
  "supportedheaders":[
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers",
  ]
}
```

網站2較為複雜，且需要授權和變體(POST、PUT、DELETE)請求：

```json
{
  "supportscredentials":true,
  "exposedheaders":[
    ""
  ],
  "supportedmethods":[
    "GET",
    "HEAD"
    "POST",
    "DELETE",
    "OPTIONS",
    "PUT"
  ],
  "alloworigin":[
    "http://127.0.0.1:3000",
    "https://site2.com"
    
  ],
  "maxage:Integer": 1800,
  "alloworiginregexp":[
    "http://localhost:.*"
    "https://.*\.site2\.com"
  ],
  "allowedpaths":[
    "/content/site2/.*",
    "/libs/granite/csrf/token.json",
  ],
  "supportedheaders":[
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers",
    "Authorization",
    "CSRF-Token"
  ]
}
```

## Dispatcher快取疑慮和設定 {#dispatcher-caching-concerns-and-configuration}

從Dispatcher 4.1.1+回應標頭開始可快取。 如此即可進行快取 [!DNL CORS] 標頭以及 [!DNL CORS]-requested resources ，只要要求為匿名。

通常，在Dispatcher快取內容的相同考量可以套用到Dispatcher的快取CORS回應標頭。 下表定義何時 [!DNL CORS] 標題(以及 [!DNL CORS] 請求)。

| 可快取 | 环境 | 驗證狀態 | 解释 |
|-----------|-------------|-----------------------|-------------|
| 否 | AEM 发布 | 已驗證 | AEM Author上的Dispatcher快取僅限於靜態的非編寫資產。 這使得在AEM Author上快取大部分資源（包括HTTP回應標頭）變得困難和不實際。 |
| 否 | AEM 发布 | 已驗證 | 避免在已驗證的請求上快取CORS標頭。 這符合不快取已驗證請求的常見指南，因為很難確定請求使用者的驗證/授權狀態將如何影響傳送的資源。 |
| 是 | AEM 发布 | 匿名 | Dispatcher中可快取的匿名請求也可以快取其回應標頭，以確保未來的CORS請求可以存取快取的內容。 AEM Publish上的任何CORS設定變更 **必須** 之後會令受影響的快取資源失效。 最佳實務指示程式碼或設定部署會清除Dispatcher快取，因為很難判斷哪些快取內容可能會生效。 |

若要允許快取CORS標頭，請將以下設定新增至所有支援的AEM Publish dispatcher.any檔案。

```
/myfarm { 
  ...
  /headers {
      "Origin"
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

記住 **重新啟動網頁伺服器應用程式** 變更後 `dispatcher.any` 檔案。

可能需要完全清除快取，以確保標題在之後的下一個請求中被適當地快取 `/cache/headers` 設定更新。

## 疑難排解CORS

記錄位於 `com.adobe.granite.cors`：

* 啟用 `DEBUG` 以檢視關於為何使用 [!DNL CORS] 要求被拒絕
* 啟用 `TRACE` 檢視所有透過CORS處理常式提出的請求的詳細資訊

### 提示：

* 使用curl手動重新建立XHR請求，但請務必複製所有標題和詳細資訊，因為每個標題和詳細資訊都可能有所不同；有些瀏覽器主控台允許複製curl命令
* 驗證請求是否被CORS處理常式拒絕，而不是被驗證、CSRF權杖篩選器、Dispatcher篩選器或其他安全性層拒絕
   * 如果CORS處理常式以200回應，但 `Access-Control-Allow-Origin` 回應中沒有標頭，請檢視記錄檔中的拒絕訊息 [!DNL DEBUG] 在 `com.adobe.granite.cors`
* 如果Dispatcher快取 [!DNL CORS] 請求已啟用
   * 確保 `/cache/headers` 設定已套用至 `dispatcher.any` 而且已成功重新啟動網頁伺服器
   * 請確保在任何OSGi或dispatcher.any設定變更後已正確清除快取。
* 如有需要，請檢查請求上是否有驗證認證。

## 支援材料

* [跨原始資源共用原則的AEM OSGi設定處理站](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [跨原始資源共用(W3C)](https://www.w3.org/TR/cors/)
* [HTTP存取控制(Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
