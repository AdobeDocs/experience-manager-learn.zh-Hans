---
title: 使用AEM as a Cloud Service缓存页面变体
description: 了解如何设置和使用AEM as a Cloud Service来支持缓存页面变体。
role: Architect, Developer
topic: Development
feature: CDN Cache, Dispatcher
exl-id: fdf62074-1a16-437b-b5dc-5fb4e11f1355
duration: 149
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '551'
ht-degree: 1%

---

# 缓存页面变量

了解如何设置和使用AEM as a Cloud Service来支持缓存页面变体。

## 示例用例

+ 任何根据用户的地理位置和包含动态内容的页面缓存提供一组不同的服务选项和相应定价选项的服务提供商都应在CDN和Dispatcher中进行管理。

+ 零售客户在全国各地都有商店，每个商店根据其所在位置提供不同的选件，并且应在CDN和Dispatcher中管理包含动态内容的页面缓存。

## 解决方案概述

+ 确定变体键及其可能具有的值的数量。 在我们的示例中，我们因美国州而异，因此最大数量为50。 这个值足够小，不会导致CDN的变量限制出现问题。 [查看变体限制部分](#variant-limitations)。

+ AEM代码必须将Cookie __&quot;x-aem-variant&quot;__&#x200B;设置为访客的首选状态(例如 `Set-Cookie: x-aem-variant=NY`)。

+ 访客的后续请求会发送该Cookie(例如， `"Cookie: x-aem-variant=NY"`)，并且Cookie将在CDN级别转换为预定义的标头（即`x-aem-variant:NY`），该标头将传递给Dispatcher。

+ Apache重写规则修改请求路径，以在页面URL中包含标头值作为Apache Sling选择器(例如 `/page.variant=NY.html`)。 这允许AEM Publish根据选择器提供不同的内容，并且调度程序为每个变体缓存一个页面。

+ AEM Dispatcher发送的响应必须包含HTTP响应标头`Vary: x-aem-variant`。 这会指示CDN存储不同标头值的不同缓存副本。

>[!TIP]
>
>无论何时设置Cookie(如 Set-Cookie： x-aem-variant=NY)不应缓存响应（应具有Cache-Control： private或Cache-Control： no-cache）

## HTTP请求流程

![变量缓存请求流](./assets/variant-cache-request-flow.png)

>[!NOTE]
>
>在请求任何使用变体的内容之前，必须执行上述初始HTTP请求流程。

## 用途

1. 为了演示该功能，我们将使用[WKND](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=zh-Hans)的实施作为示例。

1. 在AEM中实施[SlingServletFilter](https://sling.apache.org/documentation/the-sling-engine/filters.html)以使用变量值在HTTP响应上设置`x-aem-variant` Cookie。

1. AEM CDN会自动将`x-aem-variant` Cookie转换为同名的HTTP标头。

1. 将Apache Web Server mod_rewrite规则添加到您的`dispatcher`项目，该规则可修改请求路径以包含变体选择器。

1. 使用Cloud Manager部署过滤器并重写规则。

1. 测试整个请求流程。

## 代码示例

+ 在AEM中设置带值的`x-aem-variant` Cookie的SlingServletFilter示例。

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

+ __dispatcher/src/conf.d/rewrite.rules__&#x200B;文件中的示例重写规则，该文件在Git中作为源代码管理，并使用Cloud Manager部署。

  ```
  ...
  
  RewriteCond %{REQUEST_URI} ^/us/.*  
  RewriteCond %{HTTP:x-aem-variant} ^.*$  
  RewriteRule ^([^?]+)\.(html.*)$ /content/wknd$1.variant=%{HTTP:x-aem-variant}.$2 [PT,L] 
  
  ...
  ```

## 变量限制

+ AEM CDN可管理多达200个变量。 这意味着`x-aem-variant`标头最多可以有200个唯一值。 有关详细信息，请查看[CDN配置限制](https://docs.fastly.com/en/guides/resource-limits)。

+ 必须注意的是，您选择的变体键决不会超过此数字。  例如，用户ID不是合适的键，因为对于大多数网站，它很容易超过200个值，而如果某个国家/地区只有不到200个州，则该国家/地区的状态会更适合。

>[!NOTE]
>
>当变量超过200个时，CDN将做出响应“变量太多”而非页面内容。
