---
title: AEM GraphQL的CORS配置
description: 了解如何配置跨源资源共享(CORS)以用于AEM GraphQL。
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
jira: KT-10830
thumbnail: KT-10830.jpg
exl-id: 394792e4-59c8-43c1-914e-a92cdfde2f8a
last-substantial-update: 2024-03-22T00:00:00Z
duration: 185
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 2%

---

# 跨源资源共享(CORS)

Adobe Experience Manager as a Cloud Service的跨源资源共享(CORS)有助于非AEM Web资产对AEM的GraphQL API和其他AEM Headless资源进行基于浏览器的客户端调用。

>[!TIP]
>
> 以下配置是示例。 确保根据项目要求对其进行调整。

## CORS要求

如果连接到AEM GraphQL AEM API的客户端并非来自与AEM相同的源（也称为主机或域），则基于浏览器的连接需要CORS。

| 客户端类型 | [单页应用程序(SPA)](../spa.md) | [Web组件/JS](../web-component.md) | [移动设备](../mobile.md) | [服务器到服务器](../server-to-server.md) |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| 需要CORS配置 | ✔ | ✔ | ✘ | ✘ |

## AEM 作者

在AEM创作服务上启用CORS与AEM Publish和AEM Preview服务不同。 AEM Author服务要求将OSGi配置添加到AEM Author服务的运行模式文件夹，并且不使用Dispatcher配置。

### OSGi配置

AEM CORS OSGi配置工厂定义了接受CORS HTTP请求的允许标准。

| 客户端连接到 | AEM 作者 | AEM Publish | AEM预览 |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| 需要CORS OSGi配置 | ✔ | ✘ | ✘ |


以下示例为AEM Author (`../config.author/..`)定义了OSGi配置，因此该配置仅在AEM Author服务上处于活动状态。

关键配置属性包括：

+ `alloworigin`和/或`alloworiginregexp`指定了连接到AEM Web的客户端所运行的源。
+ `allowedpaths`指定允许来自指定源的URL路径模式。
   + 要支持AEM GraphQL持久查询，请添加以下模式：`/graphql/execute.json.*`
   + 要支持体验片段，请添加以下模式： `/content/experience-fragments/.*`
+ `supportedmethods`为CORS请求指定允许的HTTP方法。 要支持AEM GraphQL持久查询（和体验片段），请添加`GET` 。
+ `supportedheaders`包含`"Authorization"`，因为应授权对AEM作者的请求。
+ `supportscredentials`设置为`true`，因为应授权对AEM作者的请求。

[了解有关CORS OSGi配置的更多信息。](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html?lang=zh-Hans)

以下示例支持在AEM Author上使用AEM GraphQL持久查询。 若要使用客户端定义的GraphQL查询，请在`allowedpaths`和`POST`中将GraphQL终结点URL添加到`supportedmethods`。

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

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
    "Access-Control-Request-Headers",
    "Authorization"
  ],
  "supportedmethods":[
    "GET",
    "HEAD",
    "POST"
  ],
  "maxage:Integer": 1800,
  "supportscredentials": true,
  "exposedheaders":[ "" ]
}
```

#### 示例OSGi配置

+ [在WKND项目中可以找到OSGi配置示例。](https://github.com/adobe/aem-guides-wknd/blob/main/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json)

## AEM Publish

在AEM Publish （和Preview）服务上启用CORS与AEM Author服务不同。 AEM Publish服务要求将AEM Dispatcher配置添加到AEM Publish的Dispatcher配置。 AEM发布不使用[OSGi配置](#osgi-configuration)。

在AEM发布上配置CORS时，请确保：

+ 无法通过从AEM Dispatcher项目的`Origin`文件中删除`Origin`标头（如果之前已添加）将`clientheaders.any` HTTP请求标头发送到AEM发布服务。 应从`Access-Control-`文件中删除任何`clientheaders.any`标头，由Dispatcher管理这些标头，而不是AEM发布服务。
+ 如果您在AEM Publish服务中启用了任何[CORS OSGi配置](#osgi-configuration)，则必须将其删除并将其配置迁移到下面列出的[Dispatcher vhost配置](#set-cors-headers-in-vhost)。

### Dispatcher 配置

必须将AEM Publish (and Preview)服务的Dispatcher配置为支持CORS。

| 客户端连接到 | AEM 作者 | AEM Publish | AEM预览 |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| 需要Dispatcher CORS配置 | ✘ | ✔ | ✔ |

#### 在vhost中设置CORS标头

1. 在Dispatcher配置项目中，打开AEM Publish服务的vhost配置文件，通常位于`dispatcher/src/conf.d/available_vhosts/<example>.vhost`
2. 将以下`<IfDefine ENABLE_CORS>...</IfDefine>`块的内容复制到已启用的vhost配置文件中。

   ```{ highlight="17"}
   <VirtualHost *:80>
     ...
     <IfModule mod_headers.c>
         ################## Start of CORS configuration ##################
   
         SetEnvIfExpr "req_novary('Origin') == ''" CORSType=none CORSProcessing=false
         SetEnvIfExpr "req_novary('Origin') != ''" CORSType=cors CORSProcessing=true CORSTrusted=false
   
         SetEnvIfExpr "req_novary('Access-Control-Request-Method') == '' && %{REQUEST_METHOD} == 'OPTIONS' && req_novary('Origin') != ''" CORSType=invalidpreflight CORSProcessing=false
         SetEnvIfExpr "req_novary('Access-Control-Request-Method') != '' && %{REQUEST_METHOD} == 'OPTIONS' && req_novary('Origin') != ''" CORSType=preflight CORSProcessing=true CORSTrusted=false
         SetEnvIfExpr "req_novary('Origin') -strcmatch 'http://%{HTTP_HOST}*'" CORSType=samedomain CORSProcessing=false
         SetEnvIfExpr "req_novary('Origin') -strcmatch 'https://%{HTTP_HOST}*'" CORSType=samedomain CORSProcessing=false
   
         # For requests that require CORS processing, check if the Origin can be trusted
         SetEnvIfExpr "%{HTTP_HOST} =~ /(.*)/ " ParsedHost=$1
   
         ################## Adapt regex to match CORS origin(s) for your environment
         SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(https://.*\.your-domain\.tld(:\d+)?$)#" CORSTrusted=true
   
         # Extract the Origin header
         SetEnvIfNoCase ^Origin$ ^(.*)$ CORSTrustedOrigin=$1
   
         # Flush If already set
         Header unset Access-Control-Allow-Origin
         Header unset Access-Control-Allow-Credentials
   
         # Trusted
         Header always set Access-Control-Allow-Credentials "true" "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Allow-Origin "%{CORSTrustedOrigin}e" "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Allow-Methods "GET" "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Max-Age 1800 "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Allow-Headers "Origin, Accept, X-Requested-With, Content-Type, Access-Control-Request-Method, Access-Control-Request-Headers" "expr=reqenv('CORSTrusted') == 'true'"
   
         # Non-CORS or Not Trusted
         Header unset Access-Control-Allow-Credentials "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
         Header unset Access-Control-Allow-Origin "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
         Header unset Access-Control-Allow-Methods "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
         Header unset Access-Control-Max-Age "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
   
         # Always vary on origin, even if its not there.
         Header merge Vary Origin
   
         # CORS - send 204 for CORS requests which are not trusted
         RewriteCond expr "reqenv('CORSProcessing') == 'true' && reqenv('CORSTrusted') == 'false'"
         RewriteRule "^(.*)" - [R=204,L]
   
         # Remove Origin before sending to AEM Publish if this configuration handles the CORS
         RequestHeader unset Origin "expr=reqenv('CORSTrusted') == 'true'"
   
         ################## End of CORS configuration ##################
     </IfModule>
     ...
   </VirtualHost>
   ```

3. 通过更新以下行中的正则表达式，匹配访问AEM Publish服务的所需源。 如果需要多个原点，请复制此行并更新每个原点/原点阵列。

   ```
   SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(https://.*.your-domain.tld(:\d+)?$)#" CORSTrusted=true
   ```

   + 例如，要启用从源访问CORS，请执行以下操作：

      + `https://example.com`上的任何子域
      + `http://localhost`上的任意端口

     将该行替换为以下两行：

     ```
     SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(https://.*\.example\.com$)#" CORSTrusted=true
     SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(http://localhost(:\d+)?$)#" CORSTrusted=true
     ```

#### Dispatcher配置示例

+ [在WKND项目中可以找到Dispatcher配置示例。](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost)
