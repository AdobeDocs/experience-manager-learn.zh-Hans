---
title: 流量过滤规则（包括 WAF 规则）最佳实践
description: 了解在 AEM as a Cloud Service 中配置流量过滤规则（包括 WAF 规则）的推荐最佳实践，以提升安全性并降低风险。
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18310
thumbnail: null
exl-id: 4a7acdd2-f442-44ee-8560-f9cb64436acf
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '638'
ht-degree: 100%

---

# 流量过滤规则（包括 WAF 规则）最佳实践

了解在 AEM as a Cloud Service 中配置流量过滤规则（包括 WAF 规则）的推荐最佳实践，以提升安全性并降低风险。

>[!IMPORTANT]
>
>本文所述的最佳实践并不详尽，不应替代您自己的安全策略与程序。

## 通用最佳实践

- 从 Adobe [推荐的规则集](./overview.md#adobe-recommended-rules)出发，包括标准流量过滤规则和 WAF 规则，并根据您应用的具体需求与威胁环境进行调整。
- 与您的安全团队协作，确定哪些规则符合组织的安全策略和合规要求。
- 在将新规则或更新后的规则推广至暂存和生产环境之前，务必先在开发环境中进行充分测试。
- 在声明和验证规则时，建议首先将 `action` 类型设为 `log`，以便在不拦截合法流量的前提下观察行为模式。
- 在分析了足够的流量数据且确认不会影响合法请求后，再将规则从 `log` 切换为 `block` 模式。
- 建议逐步引入规则，并让质量保证（QA）、性能测试和安全团队共同参与测试，以识别可能产生的意外影响。
- 定期使用[仪表板分析工具](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling)审查和分析规则的有效性。审查频率（每日、每周或每月）应根据您网站的流量规模与风险状况进行调整。
- 应持续根据最新的威胁情报、流量行为及审查结果优化规则。

## 流量过滤规则最佳实践

- 使用 Adobe [推荐的标准流量过滤规则](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-starter-rules)作为基准，其中包括针对边缘和源端的防护以及基于 OFAC 的访问限制规则。
- 定期查看警报与日志，识别滥用行为或错误配置的模式。
- 根据您应用程序的流量特征和用户行为，调整速率限制的阈值。

  有关如何选择阈值的指导，请参见下表：

  | 变体 | 值 |
  | :--------- | :------- |
  | 源站 | 取&#x200B;**正常**&#x200B;流量条件下每个 IP/POP 的最大源站请求数的最大值（即，不是 DDoS 攻击时的速率），并将其增加几倍 |
  | 边缘 | 取&#x200B;**正常**&#x200B;流量条件下（即非 DDoS 攻击期间），取每个 IP/接入点（POP）下最大边缘请求数的最高值，并按一定倍数进行放大 |

  更多详细信息，请参阅[选择阈值](../blocking-dos-attack-using-traffic-filter-rules.md#choosing-threshold-values)部分。

- 只有在确认 `log` 操作不会影响合法流量后，才能将规则切换为 `block` 操作。

## WAF 规则最佳实践

- 建议从 Adobe [推荐的 WAF 规则](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-nonwaf-starter-rules)出发，其中包括阻止已知恶意 IP、检测 DDoS 攻击以及减少机器人滥用的各种规则。
- `ATTACK` WAF 标志能够警告您有潜在的威胁。务必先确保不存在误报情况，然后才切换为 `block` 模式。
- 如果推荐的 WAF 规则未涵盖某些特定威胁，您可以根据应用的独特需求创建自定义规则。请在文档中查看完整的 [WAF 标志](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#waf-flags-list)列表。

## 实施规则

了解如何在 AEM as a Cloud Service 中实施流量过滤规则和 WAF 规则：

<!-- CARDS
{target = _self}

* ./use-cases/using-traffic-filter-rules.md
  {title = Protecting AEM websites using standard traffic filter rules}
  {description = Learn how to protect AEM websites from DoS, DDoS and bot abuse using Adobe-recommended standard traffic filter rules in AEM as a Cloud Service.}
  {image = ./assets/use-cases/using-traffic-filter-rules.png}
  {cta = Apply Rules}

* ./use-cases/using-waf-rules.md
  {title = Protecting AEM websites using WAF traffic filter rules}
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
                    <a href="./use-cases/using-traffic-filter-rules.md" title="使用标准流量过滤规则保护 AEM 网站" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-traffic-filter-rules.png" alt="使用标准流量过滤规则保护 AEM 网站"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" title="使用标准流量过滤规则保护 AEM 网站">使用标准流量过滤规则保护 AEM 网站</a>
                    </p>
                    <p class="is-size-6">了解如何在 AEM as a Cloud Service 中使用 Adobe 推荐的标准流量过滤规则防护 AEM 网站抵御 DoS、DDoS 攻击以及机器人滥用。</p>
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
                    <a href="./use-cases/using-waf-rules.md" title="使用 WAF 规则保护 AEM 网站" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-waf-rules.png" alt="使用 WAF 规则保护 AEM 网站"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" title="使用 WAF 规则保护 AEM 网站">使用 WAF 规则保护 AEM 网站</a>
                    </p>
                    <p class="is-size-6">了解如何在 AEM as a Cloud Service 中使用 Adobe 推荐的 Web 应用程序防火墙（WAF）规则防御包括 DoS、DDoS 和机器人滥用在内的复杂安全威胁。</p>
                </div>
                <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">启用 WAF</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## 其他资源

- [流量过滤规则（包括 WAF 规则）](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)
- [了解 AEM 中的 DoS/DDoS 预防](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/foundation/security/understanding-dos-and-prevention-approaches)
- [使用流量过滤规则阻止 DoS 和 DDoS 攻击](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/cloud-service/security/blocking-dos-attack-using-traffic-filter-rules)
