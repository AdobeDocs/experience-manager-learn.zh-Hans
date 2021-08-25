---
title: 开发与AEM的跨域资源共享(CORS)
description: 有关利用CORS通过客户端JavaScript从外部Web应用程序访问AEM内容的简短示例。
version: 6.3, 6,4, 6.5
topic: Security, Development
role: Developer
level: Beginner
feature: Security
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 0%

---


# 开发跨域资源共享(CORS)

利用[!DNL CORS]通过客户端JavaScript从外部Web应用程序访问AEM内容的简短示例。

>[!VIDEO](https://video.tv.adobe.com/v/18837/?quality=12&learn=on)

在此视频中：

* **www.example.** commaps通过  `/etc/hosts`
* **aem-publish.** localmaps通过本地主机  `/etc/hosts`
* SimpleHTTPServer（[[!DNL Python]的SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html)的包装器）通过端口8000为HTML页提供服务。
   * _Mac应用商店中不再提供。使用类似的方法，如[Jeeves](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)。_
* [!DNL AEM Dispatcher] 运行于2. [!DNL Apache HTTP Web Server] 4上，且正在向反向代理请 `aem-publish.local` 求 `localhost:4503`。

有关更多详细信息，请参阅AEM](./understand-cross-origin-resource-sharing.md)中的[了解跨域资源共享(CORS)。

## www.example.com HTML和JavaScript

此网页的逻辑是

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

[!DNL Cross-Origin Resource Sharing]的OSGi配置工厂可通过以下方式获得：

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

## 调度程序配置 {#dispatcher-configuration}

要允许缓存内容上缓存和提供CORS标头，请将[/clientheaders配置](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#specifying-the-http-headers-to-pass-through-clientheaders)后面的添加到所有支持AEM发布`dispatcher.any`文件。

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

**在对文件进行更改** 后，重新启动Web服务器应用 `dispatcher.any` 程序。

可能需要完全清除缓存，以确保在`/clientheaders`配置更新后，在下一个请求中正确缓存标头。

## 辅助材料 {#supporting-materials}

* [AEM OSGi跨源资源共享策略配置工厂](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [适用于macOS的吉夫斯](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)
* [Python SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html) （与Windows/macOS/Linux兼容）

* [了解AEM中的跨域资源共享(CORS)](./understand-cross-origin-resource-sharing.md)
* [跨源资源共享(W3C)](https://www.w3.org/TR/cors/)
* [HTTP访问控制(Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)

