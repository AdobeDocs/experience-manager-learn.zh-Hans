---
title: 概述 — 保护AEM网站
description: 了解如何使用流量过滤器规则(包括其AEM as a Cloud Service中的Web应用程序防火墙(WAF)规则的子类别)保护您的AEM网站免受DoS、DDoS和恶意流量的攻击。
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-13148
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '1185'
ht-degree: 1%

---


# 概述 — 保护AEM网站

了解如何使用&#x200B;**流量过滤器规则**(包括其子类别： AEM as a Cloud Service中的&#x200B;**Web应用程序防火墙(WAF)**&#x200B;规则)保护您的AEM网站免受拒绝服务(DoS)、分布式拒绝服务(DDoS)、恶意流量和复杂的攻击。

此外，您还可以了解标准流量过滤器与WAF流量过滤器规则之间的差异、何时使用标准流量过滤器规则以及如何开始使用Adobe推荐的规则。

>[!IMPORTANT]
>
> WAF流量过滤器规则需要额外的&#x200B;**WAF-DDoS保护**&#x200B;或&#x200B;**增强的安全性**&#x200B;许可证。 默认情况下，Sites和Forms客户可以使用标准流量过滤器规则。

## AEM as a Cloud Service中的流量安全简介

AEM as a Cloud Service利用集成的CDN层来保护和优化网站的投放。 CDN层的最关键组件之一是定义和执行流量规则的能力。 这些规则作为保护盾牌，有助于保护您的站点免受滥用、误用和攻击，同时不会牺牲性能。

流量安全对于保持正常运行时间、保护敏感数据以及确保合法用户的无缝体验至关重要。 AEM提供两类安全规则：

- **标准流量过滤器规则**
- **Web应用程序防火墙(WAF)流量过滤器规则**

规则集可帮助客户防止常见的、复杂的Web威胁，减少恶意客户端或行为不端的客户端带来的噪音，并通过请求记录、阻止和模式检测来提高可观察性。

## 标准流量过滤器规则与WAF流量过滤器规则之间的差异

| 功能 | 标准流量过滤器规则 | WAF流量过滤规则 |
|--------------------------|--------------------------------------------------|---------------------------------------------------------|
| 用途 | 防止滥用，如DoS、DDoS、刮擦或机器人活动 | 检测复杂攻击模式并对其做出反应(例如OWASP Top 10)，这还可以防御机器人 |
| 示例 | 速率限制、地理阻塞、用户代理过滤 | SQL注入、 XSS 、已知攻击IP |
| 灵活性 | 可通过YAML进行高度配置 | 通过YAML进行高度配置，带有预定义的WAF标记 |
| 推荐模式 | 从`log`模式开始，然后移至`block`模式 | 开始为`block` WAF标志使用`ATTACK-FROM-BAD-IP`模式，为`log` WAF标志使用`ATTACK`模式，然后为两者移动到`block`模式 |
| 部署 | 在YAML中定义并通过Cloud Manager配置管道部署 | 在具有`wafFlags`的YAML中定义并通过Cloud Manager配置管道部署 |
| 许可 | 包含在站点和Forms许可证中 | **需要WAF-DDoS保护或增强安全许可证** |

标准流量过滤器规则可用于实施特定于业务的策略（如速率限制或阻止特定区域），以及根据请求属性和标头（如IP地址、路径或用户代理）阻止流量。
另一方面，WAF流量过滤器规则为已知的Web利用漏洞和攻击向量提供全面的主动保护，并拥有高级智能来限制误报（即阻止合法流量）。
若要定义这两种类型的规则，请使用YAML语法，有关详细信息，请参阅[流量过滤器规则语法](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#rules-syntax)。

## 何时及为何使用它们

**在以下情况下使用标准流量过滤器规则**：

- 您希望应用特定于组织的限制，如IP速率限制。
- 您知道需要过滤的特定模式（例如，恶意IP地址、区域、标头）。

**在以下情况下使用WAF流量过滤器规则**：

- 您需要从专家数据源收集全面的&#x200B;**主动保护**，以防止广泛存在的已知攻击模式（例如，注入、协议滥用）以及已知的恶意IP。
- 要拒绝恶意请求，同时限制阻止合法通信的机会。
- 您希望通过应用简单的配置规则来限制防御常见复杂威胁的工作量。

这些规则共同提供了一个深入防护的策略，允许AEM as a Cloud Service客户采取主动和被动措施来保护其数字资产。

## Adobe推荐的规则

Adobe为标准流量过滤器和WAF流量过滤器规则提供了推荐的规则，以帮助您快速保护AEM站点的安全。

- **标准流量筛选规则**（默认可用）：解决常见的不当使用场景，例如&#x200B;**CDN edge**、**origin**&#x200B;或来自受制裁国家/地区的流量遭受的DoS、DDoS和机器人攻击。\
  示例包括：
   - 在CDN边缘&#x200B;_处每秒发出超过500个请求_&#x200B;的速率限制IP
   - 在源位置&#x200B;_发出超过100个请求/秒_&#x200B;的速率限制IP
   - 阻止来自外国Assets控制局(OFAC)所列国家的流量

- **WAF流量过滤器规则**（需要附加许可证）：针对复杂威胁(包括[OWASP十大威胁](https://owasp.org/www-project-top-ten/)，如SQL注入、跨站点脚本(XSS)和其他Web应用程序攻击)提供额外保护。
示例包括：
   - 阻止来自已知错误IP地址的请求
   - 记录或阻止标记为攻击的可疑请求

>[!TIP]
>
> 首先应用&#x200B;**Adobe推荐的规则**，以从Adobe的安全专业知识和持续更新中获益。 如果贵企业存在特定风险或边缘案例，或者发现任何误报（阻止合法流量），您可以定义&#x200B;**自定义规则**&#x200B;或扩展默认设置以满足您的需求。

## 开始使用

了解如何按照以下设置指南和用例在AEM as a Cloud Service中定义、部署、测试和分析流量过滤器规则，包括WAF规则。 这样可为您提供背景知识，以便您稍后能够放心地应用Adobe推荐的规则。

<!-- CARDS
{target = _self}

* ./setup.md
  {title = How to set up traffic filter rules including WAF rules}
  {description = Learn how to set up to create, deploy, test, and analyze the results of traffic filter rules including WAF rules.}
  {image = ./assets/setup/rules-setup.png}
  {cta = Start Now}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="How to set up traffic filter rules including WAF rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup.md" title="如何设置流量过滤器规则，包括WAF规则" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/setup/rules-setup.png" alt="如何设置流量过滤器规则，包括WAF规则"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup.md" target="_self" rel="referrer" title="如何设置流量过滤器规则，包括WAF规则">如何设置流量过滤器规则，包括WAF规则</a>
                    </p>
                    <p class="is-size-6">了解如何设置以创建、部署、测试和分析流量过滤器规则(包括WAF规则)的结果。</p>
                </div>
                <a href="./setup.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">立即开始</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Adobe推荐的规则设置指南

本指南提供了在AEM as a Cloud Service环境中设置和部署Adobe推荐的标准流量过滤器和WAF流量过滤器规则的分步说明。

<!-- CARDS
{target = _self}

* ./use-cases/using-traffic-filter-rules.md
  {title = Protecting AEM websites using standard traffic filter rules}
  {description = Learn how to protect AEM websites from DoS, DDoS and bot abuse using Adobe-recommended standard traffic filter rules in AEM as a Cloud Service.}
  {image = ./assets/use-cases/using-traffic-filter-rules.png}
  {cta = Apply Rules}

* ./use-cases/using-waf-rules.md
  {title = Protecting AEM websites using WAF rules}
  {description = Learn how to protect AEM websites from sophisticated threats including DoS, DDoS, and bot abuse using Adobe-recommended Web Application Firewall (WAF) traffic filter rules in AEM as a Cloud Service.}
  {image = ./assets/use-cases/using-waf-rules.png}
  {cta = Activate WAF}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using standard traffic filter rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/using-traffic-filter-rules.md" title="使用标准流量过滤器规则保护AEM网站" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-traffic-filter-rules.png" alt="使用标准流量过滤器规则保护AEM网站"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" title="使用标准流量过滤器规则保护AEM网站">使用标准流量过滤器规则保护AEM网站</a>
                    </p>
                    <p class="is-size-6">了解如何使用AEM推荐的标准流量过滤器规则在AEM as a Cloud Service中保护Adobe网站免受DoS、DDoS和机器人滥用。</p>
                </div>
                <a href="./use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">应用规则</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using WAF rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/using-waf-rules.md" title="使用WAF规则保护AEM网站" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-waf-rules.png" alt="使用WAF规则保护AEM网站"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" title="使用WAF规则保护AEM网站">使用WAF规则保护AEM网站</a>
                    </p>
                    <p class="is-size-6">了解如何使用AEM推荐的Adobe Web应用程序防火墙(WAF)流量过滤器规则在AEM as a Cloud Service中保护网站免受复杂威胁，包括DoS、DDoS和机器人滥用。</p>
                </div>
                <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">激活WAF</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## 高级用例

对于更高级的场景，您可以探索以下用例，演示如何根据特定业务需求实施自定义流量过滤器规则：

<!-- CARDS
{target = _self}

* ./how-to/request-logging.md

* ./how-to/request-blocking.md

* ./how-to/request-transformation.md
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Monitoring sensitive requests">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/request-logging.md" title="监控敏感请求" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/how-to/wknd-login.png" alt="监控敏感请求"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/request-logging.md" target="_self" rel="referrer" title="监控敏感请求">正在监视敏感请求</a>
                    </p>
                    <p class="is-size-6">了解如何使用AEM as a Cloud Service中的流量过滤器规则记录敏感请求，从而监控这些请求。</p>
                </div>
                <a href="./how-to/request-logging.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解详情</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Restricting access">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/request-blocking.md" title="限制访问" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/how-to/elk-tool-dashboard-blocked.png" alt="限制访问"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/request-blocking.md" target="_self" rel="referrer" title="限制访问">限制访问</a>
                    </p>
                    <p class="is-size-6">了解如何使用AEM as a Cloud Service中的流量过滤规则阻止特定请求来限制访问。</p>
                </div>
                <a href="./how-to/request-blocking.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解详情</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Normalizing requests">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/request-transformation.md" title="标准化请求" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/how-to/aemrequest-log-transformation.png" alt="标准化请求"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/request-transformation.md" target="_self" rel="referrer" title="标准化请求">标准化请求</a>
                    </p>
                    <p class="is-size-6">了解如何使用AEM as a Cloud Service中的流量过滤器规则转换请求，从而标准化请求。</p>
                </div>
                <a href="./how-to/request-transformation.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解详情</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## 其他资源

- [流量过滤器规则，包括WAF规则](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)
