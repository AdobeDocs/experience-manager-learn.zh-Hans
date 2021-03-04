---
title: 利用AEM开发跨来源资源共享(CORS)
description: 利用CORS通过客户端JavaScript从外部Web应用程序访问AEM内容的一个简短示例。
version: 6.3, 6,4, 6.5
sub-product: 基础，内容服务，站点
topics: security, development, content-delivery
activity: develop
audience: developer
doc-type: tutorial
topic: 安全
role: 开发人员
level: 初学者
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 0%

---


# 为跨来源资源共享(CORS)进行开发

利用[!DNL CORS]通过客户端JavaScript从外部Web应用程序访问AEM内容的简短示例。

>[!VIDEO](https://video.tv.adobe.com/v/18837/?quality=12&learn=on)

在此视频中：

* **www.example.** commaps to localhost  `/etc/hosts`
* **aem-publish.** localmaps到localhost  `/etc/hosts`
* [SimpleHTTPServer](https://itunes.apple.com/us/app/simple-http-server/id441002840?mt=12) (SimpleHTTPServer的 [[!DNL Python]包装器](https://docs.python.org/2/library/simplehttpserver.html))通过端口8000为HTML页提供服务。
* [!DNL AEM Dispatcher] 正在2. [!DNL Apache HTTP Web Server] 4上运行，并将请求反向代 `aem-publish.local` 理到 `localhost:4503`。

有关详细信息，请参阅AEM](./understand-cross-origin-resource-sharing.md)中的[了解跨来源资源共享(CORS)。

## www.example.com HTML和JavaScript

此网页具有

1. 单击按钮
1. 向`http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json`发出[!DNL AJAX GET]请求
1. 从JSON响应中检索`jcr:title`
1. 将`jcr:title`注入DOM

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

## OSGi工厂配置

[!DNL Cross-Origin Resource Sharing]的OSGi配置工厂可通过以下方式提供：

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

## 调度程序配置{#dispatcher-configuration}

要允许缓存内容上的CORS头缓存和服务，请将以下[/clientheaders configuration](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#specifying-the-http-headers-to-pass-through-clientheaders)添加到所有支持AEM Publish `dispatcher.any`文件。

```
/cache { 
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

**在对文件进行更改** 后重新启动Web服务器应 `dispatcher.any` 用程序。

可能需要完全清除缓存，以确保在`/clientheaders`配置更新后在下一个请求上正确缓存标头。

## 支撑材料{#supporting-materials}

* [AEM OSGi Configuration Factory for Cross-来源 Resource Sharing Policies](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [适用于macOS的SimpleHTTPServer](https://itunes.apple.com/us/app/simple-http-server/id441002840?mt=12)
* [Python SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html) （兼容Windows/macOS/Linux）

* [了解AEM中的跨来源资源共享(CORS)](./understand-cross-origin-resource-sharing.md)
* [跨来源资源共享(W3C)](https://www.w3.org/TR/cors/)
* [HTTP访问控制(Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)

