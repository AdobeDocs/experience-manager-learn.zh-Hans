---
title: AdobeCDN — 缓存之外的高级功能
description: 了解Adobe CDN除缓存之外的高级功能，例如在CDN上配置流量、设置令牌和凭据、CDN错误页等。
version: Cloud Service
feature: Website Performance, CDN Cache
topic: Architecture, Performance, Content Management
role: Developer, Architect, User, Leader
level: Beginner
doc-type: Article
duration: 0
last-substantial-update: 2024-08-21T00:00:00Z
jira: KT-15123
thumbnail: KT-15123.jpeg
exl-id: 8948a900-01e9-49ed-9ce5-3a057f5077e4
source-git-commit: 8795024a7b5e6d10cb2ff2f770dd3d080af85e68
workflow-type: tm+mt
source-wordcount: '546'
ht-degree: 0%

---

# AdobeCDN — 缓存之外的高级功能

了解Adobe内容分发网络(CDN)除缓存之外的高级功能，例如在CDN上配置流量、设置令牌和凭据、CDN错误页面等。

除了缓存内容之外，AdobeCDN还提供多种高级功能，有助于优化网站性能。 这些功能包括：

- 在CDN上配置流量
- 配置CDN凭据和身份验证
- CDN错误页面

这些功能是&#x200B;**自助服务**&#x200B;功能。 已在AEM项目的`cdn.yaml`文件中配置并使用Cloud Manager配置管道部署。

>[!VIDEO](https://video.tv.adobe.com/v/3433104?quality=12&learn=on)

## 在CDN上配置流量

让我们了解与&#x200B;_在CDN上配置流量_&#x200B;相关的关键功能：

- **DoS攻击防护：** AdobeCDN在网络层吸收DoS攻击，阻止它们访问您的源服务器。
- **速率限制：**&#x200B;为了保护您的源服务器免受过多请求的影响，您可以在CDN上配置速率限制。
- **Web应用程序防火墙(WAF)：** WAF保护您的网站免受常见Web应用程序漏洞的攻击，如SQL注入、跨站点脚本编写等。 使用此功能需要增强安全许可证或WAF-DOS保护许可证。
- **请求转换：**&#x200B;修改传入请求，例如设置或取消设置标头、修改查询参数、Cookie等。
- **响应转换：**&#x200B;修改传出响应，如设置或取消设置标头。
- **源选择：**&#x200B;根据请求URL将流量路由到其他源服务器(Adobe和非Adobe)。
- **URL重定向：**&#x200B;将请求(HTTP 301/302)重定向到不同的绝对或相对URL。

## 配置CDN凭据和身份验证

让我们了解与&#x200B;_配置CDN凭据和身份验证_&#x200B;相关的密钥功能：

- **清除API令牌**：允许您创建自己的清除密钥，以便从缓存中清除单个资源、组资源或所有资源。
- **基本身份验证**：一种轻量级身份验证机制，当您想要限制访问您的网站或网站的一部分时。 在上线之前，通常需要作为各种审核流程的一部分。
- **HTTP标头验证**：在客户管理的CDN将流量路由到AdobeCDN时使用。 AdobeCDN根据`X-AEM-Edge-Key`标头值验证传入的请求。 允许您为`X-AEM-Edge-Key`标头创建自己的值。

## CDN错误页面

让我们了解与&#x200B;_CDN错误页面_&#x200B;相关的关键功能：

- **品牌错误页**：当AdobeCDN无法访问您的源服务器时，在&#x200B;_不可能的情况_&#x200B;中向您的用户显示品牌错误页。

## 实施方式

这些高级功能的实施包括两个步骤：

1. **更新CDN配置文件**：使用所需的配置更新AEM项目中的`cdn.yaml`文件。 这些配置作为规则添加，并且遵循规则语法。 规则三个主要组件： `name`、`when`和`action`。

2. **部署CDN配置文件**：使用Cloud Manager配置管道部署更新的`cdn.yaml`文件。 有关详细信息，请参阅[通过Cloud Manager部署规则](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager)。

### 示例

在以下示例中，示例WKND站点配置为将`/top3` URL重定向到`/us/en/top3.html`。

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  experimental_redirects:
    rules:
      - name: redirect-top3-adventures
        when: { reqProperty: path, equals: "/top3" }
        action:
          type: redirect
          status: 302
          location: /us/en/top3.html
```

## 相关Tutorials

[使用流量过滤器规则保护网站](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/overview)

[配置和部署HTTP标头验证CDN规则](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/content-delivery/custom-domain-names-with-customer-managed-cdn#configure-and-deploy-http-header-validation-cdn-rule)

[如何清除CDN缓存](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/caching/how-to/purge-cache)

[配置CDN错误页](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/content-delivery/custom-error-pages#cdn-error-pages)

[在CDN上配置流量](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#client-side-redirectors)

[正在配置CDN凭据和身份验证](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-credentials-authentication)

