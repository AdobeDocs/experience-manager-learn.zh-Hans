---
title: 使用 AEM 进行跨源资源共享 (CORS) 开发
description: 一个简短的示例，展示了如何利用 CORS 通过客户端 JavaScript 从外部 Web 应用程序访问 AEM 内容。
version: Experience Manager 6.4, Experience Manager 6.5
topic: Security, Development
role: Developer
level: Beginner
feature: Security
doc-type: Technical Video
exl-id: 867cf74e-44e7-431b-ac8f-41b63c370635
duration: 333
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '318'
ht-degree: 100%

---

# 针对跨源资源共享 (CORS) 进行开发

一个简短的示例，展示了如何利用 [!DNL CORS] 通过客户端 JavaScript 从外部 Web 应用程序访问 AEM 内容。本示例使用 CORS OSGi 配置在 AEM 上启用 CORS 访问。OSGi 配置方法在以下情况下是可行的：

* 单个来源正在访问 AEM Publish 内容
* AEM Author 需要 CORS 访问权限

如果需要对 AEM Publish 进行多源访问，请参阅[此文档](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/configurations/cors.html?lang=zh-Hans#dispatcher-configuration)。

>[!VIDEO](https://video.tv.adobe.com/v/33362?quality=12&learn=on&captions=chi_hans)

在本视频中：

* **www.example.com** 通过 `/etc/hosts` 映射到本地主机
* **aem-publish.local** 通过 `/etc/hosts` 映射到本地主机 
* SimpleHTTPServer（[[!DNL Python] 的 SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html) 的封装器）正通过端口 8000 提供 HTML 页面。
   * _Mac App Store 中不再提供。使用类似的如 [Jeeves](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)。_
* [!DNL AEM Dispatcher] 正在 [!DNL Apache HTTP Web Server] 2.4 上运行，并将请求反向代理到 `aem-publish.local`，再转发到 `localhost:4503`。

如需了解更多详情，请查阅[了解 AEM 中的跨源资源共享 (CORS)](./understand-cross-origin-resource-sharing.md)。

## www.example.com HTML 和 JavaScript

此 Web 页面包含逻辑

1. 点击按钮后
1. 向 `http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json` 发起 [!DNL AJAX GET] 请求
1. 从 JSON 响应中检索 `jcr:title` 表单
1. 将 `jcr:title` 注入到 DOM 中

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

## OSGi 工厂配置

[!DNL Cross-Origin Resource Sharing] 的 OSGi 配置工厂可通过以下方式获取：

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

### 允许 CORS 请求标头

为了使所需的 [HTTP 请求标头能够通过并进入 AEM 进行处理](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=zh-Hans#specifying-the-http-headers-to-pass-through-clientheaders)，必须在 Disaptcher 的 `/clientheaders` 配置中允许这些请求标头。

```
/clientheaders {
   ...
   "Origin"
   "Access-Control-Request-Method"
   "Access-Control-Request-Headers"
}
```

### 缓存 CORS 响应标头

为了允许在缓存内容上缓存和提供 CORS 标头，请将以下 [/cache /headers configuration](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=zh-Hans#caching-http-response-headers) 添加到 AEM Publish `dispatcher.any`文件中。

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

在对 `dispatcher.any` 文件进行更改后，**请重新启动 Web 服务器应用程序**。

为确保在 `/cache /headers` 配置更新后的下一个请求中，标头得到适当缓存，可能需要完全清除缓存。

## 支持材料 {#supporting-materials}

* [适用于 macOS 的 Jeeves](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)
* [Python SimpleHTTPServer](https://docs.python.o:qrg/2/library/simplehttpserver.html)（与 Windows/macOS/Linux 兼容）

* [理解 AEM 中的跨源资源共享 (CORS)](./understand-cross-origin-resource-sharing.md)
* [跨源资源共享 (W3C)](https://www.w3.org/TR/cors/)
* [HTTP 访问控制 (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
