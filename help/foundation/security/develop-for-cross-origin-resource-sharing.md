---
title: 使用AEM開發跨原始資源共用(CORS)
description: 運用CORS透過使用者端JavaScript從外部Web應用程式存取AEM內容的簡短範例。
version: 6.4, 6.5
topic: Security, Development
role: Developer
level: Beginner
feature: Security
exl-id: 867cf74e-44e7-431b-ac8f-41b63c370635
source-git-commit: 4c91ab68f6e31f0eb549689c7ecfd0ee009801d9
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 0%

---

# 為跨原始資源共用(CORS)開發

運用方式的簡短範例 [!DNL CORS] 透過使用者端JavaScript從外部Web應用程式存取AEM內容。

>[!VIDEO](https://video.tv.adobe.com/v/18837?quality=12&learn=on)

在本影片中：

* **www.example.com** 透過以下方式對應至localhost `/etc/hosts`
* **aem-publish.local** 透過以下方式對應至localhost `/etc/hosts`
* SimpleHTTPServer (的包裝函式 [[!DNL Python]的SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html))透過連線埠8000提供HTML頁面。
   * _Mac App Store不再提供此功能。 使用類似的，例如 [吉夫](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)._
* [!DNL AEM Dispatcher] 執行時間 [!DNL Apache HTTP Web Server] 2.4與反向代理要求至 `aem-publish.local` 至 `localhost:4503`.

如需詳細資訊，請檢閱 [瞭解AEM中的跨原始資源共用(CORS)](./understand-cross-origin-resource-sharing.md).

## www.example.comHTML和JavaScript

此網頁的邏輯如下

1. 按一下按鈕時
1. 建立 [!DNL AJAX GET] 要求至 `http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json`
1. 擷取 `jcr:title` 形成JSON回應
1. 插入 `jcr:title` 進入DOM

```xml
<html>
<head>
<script
  src="https://code.jquery.com/jquery-3.2.1.min.js"
  integrity="sha256-hwg4gsxgFZhOsEEamdOYGBf13FyQuiTwlAQgxVSNgt4="
  crossorigin="anonymous"></script>   
</head>
<body style="width: 960px; margin: 2rem auto; font-size: 2rem;">
    <button style="font-size: 2rem;"
            data="fn-getTitle">Get Title as JSON from AEM</button>
    <pre id="title">The page title AJAX'd in from AEM will injected here</pre>
    
    <script>
    $(function() { 
        
        /** Get Title as JSON **/
        $('body').on('click', '[data="fn-getTitle"]', function(e) { 
            $.get('http://aem-publish.local/content/we-retail/us/en/experience/_jcr_content.1.json', function(data) {
                $('#title').text(data['jcr:title']);
            },'json');
            
            e.preventDefault();
            return false;
        });
    });
    </script>
</body>
</html>
```

## OSGi工廠設定

的OSGi設定處理站 [!DNL Cross-Origin Resource Sharing] 可透過以下方式取得：

* `http://<host>:<port>/system/console/configMgr > [!UICONTROL Adobe Granite Cross-Origin Resource Sharing Policy]`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
    jcr:primaryType="sling:OsgiConfig"
    alloworigin="[https://www.example.com:8000]"
    alloworiginregexp="[]"
    allowedpaths="[/content/we-retail/.*]"
    exposedheaders="[]"
    maxage="{Long}1800"
    supportedheaders="[Origin,Accept,X-Requested-With,Content-Type,
Access-Control-Request-Method,Access-Control-Request-Headers]"
    supportedmethods="[GET]"
    supportscredentials="{Boolean}false"
/>
```

## Dispatcher 配置 {#dispatcher-configuration}

若要允許在快取內容上快取及提供CORS標頭，請新增以下內容 [/clientheaders設定](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#specifying-the-http-headers-to-pass-through-clientheaders) 至所有支援的AEM Publish `dispatcher.any` 檔案。

```
/myfarm { 
  ...
  /clientheaders {
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

**重新啟動網頁伺服器應用程式** 變更後 `dispatcher.any` 檔案。

可能需要完全清除快取，以確保標題在之後的下一個請求中被適當地快取 `/clientheaders` 設定更新。

## 支援材料 {#supporting-materials}

* [macOS的Jeeves](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)
* [Python SimpleHTTPServer](https://docs.python.o:qrg/2/library/simplehttpserver.html) (與Windows/macOS/Linux相容)

* [瞭解AEM中的跨原始資源共用(CORS)](./understand-cross-origin-resource-sharing.md)
* [跨原始資源共用(W3C)](https://www.w3.org/TR/cors/)
* [HTTP存取控制(Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
