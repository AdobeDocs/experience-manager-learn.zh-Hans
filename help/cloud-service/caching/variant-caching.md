---
title: 使用AEMas a Cloud Service缓存页面变体
description: 了解如何设置和使用AEM as a cloud service来支持缓存页面变体。
role: Architect, Developer
topic: Development
feature: CDN Cache, Dispatcher
source-git-commit: 7ca34c2fe14ffb944746e9ae1171bbf2309e3c9d
workflow-type: tm+mt
source-wordcount: '538'
ht-degree: 2%

---

# 缓存页面变体

了解如何设置和使用AEM as a cloud service来支持缓存页面变体。

## 用例示例

+ 根据用户的地理位置和包含动态内容的页面缓存，提供一组不同服务和相应定价选项的任何服务提供商都应在CDN和Dispatcher中进行管理。

+ 零售客户在全国各地拥有商店，每家商店根据其所在位置提供不同的选件，且包含动态内容的页面缓存应在CDN和Dispatcher中进行管理。

## 解决方案概述

+ 识别变体键值及其可能具有的值数。 在我们的示例中，我们因美国州而异，因此最大数为50。 这个量度足够小，不会导致CDN的变体限制问题。 [查看变体限制部分](#variant-limitations).

+ AEM代码必须设置Cookie __&quot;x-aem-variant&quot;__ 访客的首选状态(例如 `Set-Cookie: x-aem-variant=NY`)，以响应初始HTTP请求的相应HTTP响应。

+ 访客的后续请求会发送该Cookie(例如 `“Cookie: x-aem-variant=NY”`)，并且Cookie将在CDN级别转换为预定义的标头(例如， `x-aem-variant:NY`)，以传递到调度程序。

+ Apache重写规则修改了请求路径，以将标头值作为Apache Sling选择器(例如， `/page.variant=NY.html`). 这允许AEM发布根据选择器提供不同的内容，而调度程序则为每个变体缓存一个页面。

+ 由AEM Dispatcher发送的响应必须包含HTTP响应标头 `Vary: x-aem-variant`. 这会指示CDN针对不同的标头值存储不同的缓存副本。

## HTTP请求流

![变型缓存请求流](./assets/variant-cache-request-flow.png)

>[!NOTE]
>
>上述初始HTTP请求流程必须在使用变体的任何内容被请求之前进行。

## 用途

1. 为了演示该功能，我们将使用 [WKND](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=zh-Hans)以的实施为例。

1. 实施 [SlingServletFilter](https://sling.apache.org/documentation/the-sling-engine/filters.html) 在AEM中设置 `x-aem-variant` Cookie（具有变量值）。

1. AEM CDN自动转换 `x-aem-variant` Cookie转换为同名的HTTP标头。

1. 向 `dispatcher` 项目，可修改请求路径以包含变体选择器。

1. 使用Cloud Manager部署过滤器并重写规则。

1. 测试整个请求流程。

## 代码示例

+ 要设置的示例SlingServletFilter `x-aem-variant` cookie中的值。

   ```
   package com.adobe.aem.guides.wknd.core.servlets.filters;
   
   import javax.servlet.*;
   import java.io.IOException;
   
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.api.SlingHttpServletResponse;
   import org.apache.sling.servlets.annotations.SlingServletFilter;
   import org.apache.sling.servlets.annotations.SlingServletFilterScope;
   import org.osgi.service.component.annotations.Component;
   import org.slf4j.Logger;
   import org.slf4j.LoggerFactory;
   
   
   // Invoke filter on  HTTP GET /content/wknd.*.foo|bar.html|json requests.
   // This code and scope is for example purposes only, and will not interfere with other requests.
   @Component
   @SlingServletFilter(scope = {SlingServletFilterScope.REQUEST},
           resourceTypes = {"cq:Page"},
           pattern = "/content/wknd/.*",
           extensions = {"html", "json"},
           methods = {"GET"})
   public class PageVariantFilter implements Filter {
       private static final Logger log = LoggerFactory.getLogger(PageVariantFilter.class);
       private static final String VARIANT_COOKIE_NAME = "x-aem-variant";
   
       @Override
       public void init(FilterConfig filterConfig) throws ServletException { }
   
       @Override
       public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
           SlingHttpServletResponse slingResponse = (SlingHttpServletResponse) servletResponse;
           SlingHttpServletRequest slingRequest = (SlingHttpServletRequest) servletRequest;
   
           // Check is the variant was previously set
           final String existingVariant = slingRequest.getCookie(VARIANT_COOKIE_NAME).getValue();
   
           if (existingVariant == null) {
               // Variant has not been set, so set it now
               String newVariant = "NY"; // Hard coding as an example, but should be a calculated value
               slingResponse.setHeader("Set-Cookie", VARIANT_COOKIE_NAME + "=" + newVariant + "; Path=/; HttpOnly; Secure; SameSite=Strict");
               log.debug("x-aem-variant cookie is set with the value {}", newVariant);
           } else {
               log.debug("x-aem-variant previously set with value {}", existingVariant);
           }
   
           filterChain.doFilter(servletRequest, slingResponse);
       }
   
       @Override
       public void destroy() { }
   }
   ```

+ 中的重写规则示例 __dispatcher/src/conf.d/rewrite.rules__ 文件，该文件在Git中作为源代码进行管理，并使用Cloud Manager进行部署。

   ```
   ...
   
   RewriteCond %{REQUEST_URI} ^/us/.*  
   RewriteCond %{HTTP:x-aem-variant} ^.*$  
   RewriteRule ^([^?]+)\.(html.*)$ /content/wknd$1.variant=%{HTTP:x-aem-variant}.$2 [PT,L] 
   
   ...
   ```

## 变体限制

+ AEM CDN最多可管理200个变体。 这表示 `x-aem-variant` 标头最多可以具有200个唯一值。 有关更多信息，请查看 [CDN配置限制](https://docs.fastly.com/en/guides/resource-limits).

+ 必须小心，确保所选的变体密钥永远不会超过此数字。  例如，用户ID不是一个好键，因为对于大多数网站而言，它会轻松超过200个值，而如果一个国家/地区的州/地区少于200个，则它更适合。

>[!NOTE]
>
>当变体数量超过200时，CDN将使用“变体过多”响应来响应，而不是页面内容。
