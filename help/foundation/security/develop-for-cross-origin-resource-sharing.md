---
title: 开发与AEM的跨域资源共享(CORS)
description: 有关利用CORS通过客户端JavaScript从外部Web应用程序访问AEM内容的简短示例。
version: 6.4, 6.5
topic: Security, Development
role: Developer
level: Beginner
feature: Security
exl-id: 867cf74e-44e7-431b-ac8f-41b63c370635
source-git-commit: 41be8c934bba16857d503398b5c7e327acd8d20b
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 0%

---

# 开发跨域资源共享(CORS)

利用率的简短示例 [!DNL CORS] 通过客户端JavaScript从外部Web应用程序访问AEM内容。

>[!VIDEO](https://video.tv.adobe.com/v/18837/?quality=12&learn=on)

在此视频中：

* **www.example.com** 通过映射到本地主机 `/etc/hosts`
* **aem-publish.local** 通过映射到本地主机 `/etc/hosts`
* SimpleHTTPServer(包装器 [[!DNL Python]&#39;s SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html))通过端口8000提供HTML页面。
   * _在Mac App Store中不再可用。 使用类似的 [吉夫斯](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)._
* [!DNL AEM Dispatcher] 正在运行 [!DNL Apache HTTP Web Server] 2.4和反向代理请求 `aem-publish.local` to `localhost:4503`.

有关更多详细信息，请查看 [了解AEM中的跨域资源共享(CORS)](./understand-cross-origin-resource-sharing.md).

## www.example.comHTML和JavaScript

此网页的逻辑是

1. 单击按钮
1. 制作 [!DNL AJAX GET] 请求 `http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json`
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

的OSGi配置工厂 [!DNL Cross-Origin Resource Sharing] 可通过以下方式获取：

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

要允许缓存内容上缓存和提供CORS标头，请添加以下内容 [/clientheaders配置](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#specifying-the-http-headers-to-pass-through-clientheaders) 发布到所有支持的AEM `dispatcher.any` 文件。

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

**重新启动Web服务器应用程序** 更改后 `dispatcher.any` 文件。

可能需要完全清除缓存，以确保在 `/clientheaders` 配置更新。

## 辅助材料 {#supporting-materials}

* [AEM OSGi跨源资源共享策略配置工厂](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [吉夫斯为macOS](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)
* [Python SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html) (与Windows/macOS/Linux兼容)

* [了解AEM中的跨域资源共享(CORS)](./understand-cross-origin-resource-sharing.md)
* [跨源资源共享(W3C)](https://www.w3.org/TR/cors/)
* [HTTP访问控制(Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
