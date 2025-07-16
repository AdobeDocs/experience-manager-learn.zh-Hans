---
title: 使用流量过滤规则阻止DoS、DDoS和复杂的攻击
description: 了解如何使用AEM as a Cloud Service中的流量过滤规则阻止DoS、DDoS和复杂的攻击。
version: Experience Manager as a Cloud Service
feature: Security, Operations
topic: Security, Administration, Performance
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
duration: 436
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15184
thumbnail: KT-15184.jpeg
exl-id: 60c2306f-3cb6-4a6e-9588-5fa71472acf7
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '609'
ht-degree: 32%

---

# 使用流量过滤规则阻止DoS、DDoS和复杂的攻击

了解如何在AEM as a Cloud Service (AEMCS)托管的CDN中使用&#x200B;**流量过滤器规则**&#x200B;来阻止拒绝服务(DoS)、分布式拒绝服务(DDoS)和复杂的攻击。

这些攻击会导致 CDN 流量激增，并可能影响 AEM Publish 服务（即源站）的流量，进而影响网站的响应能力和可用性。

本文概述了AEM网站的默认保护，以及如何通过客户配置扩展这些保护。 它还介绍了如何分析流量模式并配置标准流量过滤器规则以阻止这些攻击。

## AEM as a Cloud Service中的默认保护

让我们了解一下您的 AEM 网站的默认 DDoS 防护：

- **缓存：**&#x200B;借助良好的缓存策略，DDoS 攻击的影响将更为有限，因为内容分发网络能够阻止大部分请求流向源站，从而避免性能下降。
- **自动扩展：**&#x200B;尽管 AEM 的 Author 和 Publish 服务仍可能受到流量突然大幅增加的影响，但它们能够自动扩展，以应对流量激增的情况。
- **阻止：**&#x200B;如果来自特定 IP 地址的流量超过 Adobe 根据每个 CDN 接入点 (PoP) 定义的速率，则 Adobe CDN 将会阻止该流量到达源站。
- **警报：**&#x200B;当流量超过特定速率时，操作中心会发送有关源站流量激增的警报通知。当任何给定 CDN PoP 的流量超过 _Adobe 定义的_&#x200B;每个 IP 地址的请求速率时，会触发此警报。请参阅[流量过滤规则警报](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#traffic-filter-rules-alerts)，以了解更多详情。

这些内置保护措施应被视为组织将 DDoS 攻击对性能的影响降至最低的能力的基准。由于每个网站都有不同的性能特征，并且在达到Adobe定义的速率限制之前可能会看到性能下降，因此建议通过&#x200B;_客户配置_&#x200B;来扩展默认保护。

## 使用流量过滤规则扩展保护

让我们来看看客户可以采取的另外一些推荐措施，以保护其网站免受 DDoS 攻击：

- 实施Adobe推荐的[标准流量过滤器规则](./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md)，通过记录可疑行为并发出警报来识别潜在的恶意流量模式。
- 使用&#x200B;**WAF-DDoS保护**&#x200B;或&#x200B;**增强安全性**&#x200B;附加组件并实施Adobe推荐的[WAF流量过滤器规则](./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md)来防御复杂的攻击，包括那些使用高级协议或基于有效负载的技术的攻击。
- 通过将[请求转换](./traffic-filter-and-waf-rules/how-to/request-transformation.md)配置为忽略不必要的查询参数来提高缓存覆盖率。

## 开始使用

浏览以下教程，配置Adobe推荐的规则以阻止攻击。

<!-- CARDS
{target = _self}

* ./traffic-filter-and-waf-rules/setup.md
  {title = How to set up traffic filter rules including WAF rules}
  {description = Learn how to set up to create, deploy, test, and analyze the results of traffic filter rules including WAF rules.}
  {image = ./traffic-filter-and-waf-rules/assets/setup/rules-setup.png}
  {cta = Start Now}

* ./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md
  {title = Protecting AEM websites using standard traffic filter rules}
  {description = Learn how to protect AEM websites from DoS, DDoS and bot abuse using Adobe-recommended standard traffic filter rules in AEM as a Cloud Service.}
  {image = ./traffic-filter-and-waf-rules/assets/use-cases/using-traffic-filter-rules.png}
  {cta = Apply Rules}

* ./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md
  {title = Protecting AEM websites using WAF traffic filter rules}
  {description = Learn how to protect AEM websites from sophisticated threats including DoS, DDoS, and bot abuse using Adobe-recommended Web Application Firewall (WAF) traffic filter rules in AEM as a Cloud Service.}
  {image = ./traffic-filter-and-waf-rules/assets/use-cases/using-waf-rules.png}
  {cta = Activate WAF}

-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="How to set up traffic filter rules including WAF rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./traffic-filter-and-waf-rules/setup.md" title="如何设置流量过滤器规则，包括WAF规则" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./traffic-filter-and-waf-rules/assets/setup/rules-setup.png" alt="如何设置流量过滤器规则，包括WAF规则"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./traffic-filter-and-waf-rules/setup.md" target="_self" rel="referrer" title="如何设置流量过滤器规则，包括WAF规则">如何设置流量过滤器规则，包括WAF规则</a>
                    </p>
                    <p class="is-size-6">了解如何设置以创建、部署、测试和分析流量过滤器规则(包括WAF规则)的结果。</p>
                </div>
                <a href="./traffic-filter-and-waf-rules/setup.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">立即开始</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using standard traffic filter rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md" title="使用标准流量过滤器规则保护AEM网站" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./traffic-filter-and-waf-rules/assets/use-cases/using-traffic-filter-rules.png" alt="使用标准流量过滤器规则保护AEM网站"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" title="使用标准流量过滤器规则保护AEM网站">使用标准流量过滤器规则保护AEM网站</a>
                    </p>
                    <p class="is-size-6">了解如何使用AEM推荐的标准流量过滤器规则在AEM as a Cloud Service中保护Adobe网站免受DoS、DDoS和机器人滥用。</p>
                </div>
                <a href="./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">应用规则</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using WAF traffic filter rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md" title="使用WAF流量过滤器规则保护AEM网站" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./traffic-filter-and-waf-rules/assets/use-cases/using-waf-rules.png" alt="使用WAF流量过滤器规则保护AEM网站"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md" target="_self" rel="referrer" title="使用WAF流量过滤器规则保护AEM网站">使用AEM流量过滤器规则保护WAF网站</a>
                    </p>
                    <p class="is-size-6">了解如何使用AEM推荐的Adobe Web应用程序防火墙(WAF)流量过滤器规则在AEM as a Cloud Service中保护网站免受复杂威胁，包括DoS、DDoS和机器人滥用。</p>
                </div>
                <a href="./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">激活WAF</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
