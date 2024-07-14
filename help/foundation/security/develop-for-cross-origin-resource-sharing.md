---
title: 使用AEM开发跨源资源共享(CORS)
description: 利用CORS通过客户端JavaScript从外部Web应用程序访问AEM内容的简短示例。
version: 6.4, 6.5
topic: Security, Development
role: Developer
level: Beginner
feature: Security
doc-type: Technical Video
exl-id: 867cf74e-44e7-431b-ac8f-41b63c370635
duration: 333
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 0%

---

# 为跨源资源共享(CORS)而开发

利用[!DNL CORS]通过客户端JavaScript从外部Web应用程序访问AEM内容的简短示例。 此示例使用CORS OSGi配置在AEM上启用CORS访问。 在以下情况下，OSGi配置方法是可行的：

* 访问AEM Publish内容的来源只有一个
* AEM创作需要CORS访问权限

如果需要对AEM Publish的多源访问，请参阅[此文档](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/configurations/cors.html?lang=en#dispatcher-configuration)。

>[!VIDEO](https://video.tv.adobe.com/v/18837?quality=12&learn=on)

在此视频中：

* **www.example.com**&#x200B;通过`/etc/hosts`映射到本地主机
* **aem-publish.local**&#x200B;通过`/etc/hosts`映射到本地主机
* SimpleHTTPServer （[[!DNL Python]的SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html)的包装器）正在通过端口8000为HTML页提供服务。
   * _在Mac App Store中不再可用。 使用类似的，如[吉维斯](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)._
* [!DNL AEM Dispatcher]正在[!DNL Apache HTTP Web Server] 2.4上运行，并且正在反向代理请求`aem-publish.local`到`localhost:4503`。

有关详细信息，请查阅[了解AEM](./understand-cross-origin-resource-sharing.md)中的跨源资源共享(CORS)。

## www.example.comHTML和JavaScript

此网页的逻辑是

1. 单击按钮时
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

[!DNL Cross-Origin Resource Sharing]的OSGi配置工厂可通过以下方式使用：

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

## Dispatcher配置 {#dispatcher-configuration}

### 允许CORS请求标头

要允许所需的[HTTP请求标头传递到AEM进行处理](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#specifying-the-http-headers-to-pass-through-clientheaders)，必须在Disaptcher的`/clientheaders`配置中允许使用这些标头。

```
/clientheaders {
   ...
   "Origin"
   "Access-Control-Request-Method"
   "Access-Control-Request-Headers"
}
```

### 缓存CORS响应标头

要允许在缓存的内容上缓存并提供CORS标头，请将以下[/cache /headers配置](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=zh-Hans#caching-http-response-headers)添加到AEM Publish `dispatcher.any`文件中。

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

**在对`dispatcher.any`文件进行更改后重新启动Web服务器应用程序**。

可能需要完全清除缓存，以确保在更新`/cache /headers`配置后的下一个请求中正确缓存标头。

## 支持材料 {#supporting-materials}

* macOS的[吉夫](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)
* [Python SimpleHTTPServer](https://docs.python.o:qrg/2/library/simplehttpserver.html)(与Windows/macOS/Linux兼容)

* [了解AEM中的跨源资源共享(CORS)](./understand-cross-origin-resource-sharing.md)
* [跨源资源共享(W3C)](https://www.w3.org/TR/cors/)
* [HTTP访问控制(Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
