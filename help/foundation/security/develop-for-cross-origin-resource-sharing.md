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
duration: 378
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 0%

---

# 为跨源资源共享(CORS)而开发

利用的简短示例 [!DNL CORS] 通过客户端JavaScript从外部Web应用程序访问AEM内容。 此示例使用CORS OSGi配置在AEM上启用CORS访问。 在以下情况下，OSGi配置方法是可行的：

* 访问AEM Publish内容的来源只有一个
* AEM创作需要CORS访问权限

如果需要对AEM Publish的多源访问，请参阅 [本文档](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/configurations/cors.html?lang=en#dispatcher-configuration).

>[!VIDEO](https://video.tv.adobe.com/v/18837?quality=12&learn=on)

在此视频中：

* **www.example.com** 通过以下方式映射到本地主机 `/etc/hosts`
* **aem-publish.local** 通过以下方式映射到本地主机 `/etc/hosts`
* SimpleHTTPServer（的包装器） [[!DNL Python]的SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html))通过端口8000提供HTML页面。
   * _Mac App Store中不再提供。 使用类似于 [吉夫](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)._
* [!DNL AEM Dispatcher] 运行于 [!DNL Apache HTTP Web Server] 2.4和反向代理请求 `aem-publish.local` 到 `localhost:4503`.

有关更多详细信息，请查看 [了解AEM中的跨源资源共享(CORS)](./understand-cross-origin-resource-sharing.md).

## www.example.comHTML和JavaScript

此网页的逻辑是

1. 单击按钮时
1. 发出 [!DNL AJAX GET] 请求给 `http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json`
1. 检索 `jcr:title` 形成JSON响应
1. 插入 `jcr:title` 进入DOM

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

的OSGi配置工厂 [!DNL Cross-Origin Resource Sharing] 通过以下方式提供：

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

允许必需的 [要传递到AEM以进行处理的HTTP请求标头](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#specifying-the-http-headers-to-pass-through-clientheaders)，它们必须在Dispatcher的 `/clientheaders` 配置。

```
/clientheaders {
   ...
   "Origin"
   "Access-Control-Request-Method"
   "Access-Control-Request-Headers"
}
```

### 缓存CORS响应标头

要允许在缓存的内容上缓存和提供CORS标头，请添加以下内容 [/cache /headers配置](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=zh-Hans#caching-http-response-headers) 到AEM发布 `dispatcher.any` 文件。

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

**重新启动Web服务器应用程序** 在对进行更改之后 `dispatcher.any` 文件。

可能需要完全清除缓存，以确保在之后的下一个请求中正确缓存标头 `/cache /headers` 配置更新。

## 支持材料 {#supporting-materials}

* [适用于macOS的吉夫](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)
* [Python SimpleHTTPServer](https://docs.python.o:qrg/2/library/simplehttpserver.html) (与Windows/macOS/Linux兼容)

* [了解AEM中的跨源资源共享(CORS)](./understand-cross-origin-resource-sharing.md)
* [跨源资源共享(W3C)](https://www.w3.org/TR/cors/)
* [HTTP访问控制(Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
