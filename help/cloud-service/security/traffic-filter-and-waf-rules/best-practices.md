---
title: 流量过滤规则（包括 WAF 规则）最佳实践
description: 了解在AEM as a Cloud Service中配置流量过滤器规则(包括WAF规则)的建议最佳实践，以增强安全性和降低风险。
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18310
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '638'
ht-degree: 18%

---

# 流量过滤规则（包括 WAF 规则）最佳实践

了解在AEM as a Cloud Service中配置流量过滤器规则(包括WAF规则)的建议最佳实践，以增强安全性和降低风险。

>[!IMPORTANT]
>
>本文中介绍的最佳实践并非详尽无遗，也不会替代您自己的安全政策和程序。

## 通用最佳实践

- 从Adobe提供的[推荐的标准流量过滤器和WAF规则集](./overview.md#adobe-recommended-rules)开始，并根据应用程序的特定需求和威胁环境对其进行调整。
- 与您的安全团队协作，以确定哪些规则符合您组织的安全状况和法规遵从性要求。
- 在将新规则升级到暂存和生产环境之前，请始终在开发环境中测试新规则或更新规则。
- 声明和验证规则时，从`action`类型`log`开始，在不阻止合法通信的情况下观察行为。
- 只有在分析足够的流量数据并确认没有有效的请求受到影响后，才从`log`移动到`block`。
- 以增量方式引入规则，涉及QA、性能和安全测试团队，以确定意外的副作用。
- 使用[仪表板工具](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling)定期查看和分析规则有效性。 审查的频率（每日、每周、每月）应与您网站的流量和风险配置文件保持一致。
- 不断根据新的威胁情报、流量行为和审核结果优化规则。

## 流量过滤规则最佳实践

- 使用Adobe [推荐的标准流量过滤器规则](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-starter-rules)作为基准，其中包括边缘、来源保护和基于OFAC的限制的规则。
- 定期查看警报和日志，识别滥用或配置错误的模式。
- 根据应用程序的流量模式和用户行为调整速率限制的阈值。

  有关如何选择阈值的指导，请参见下表：

  | 变体 | 值 |
  | :--------- | :------- |
  | 源站 | 取&#x200B;**正常**&#x200B;流量条件下每个 IP/POP 的最大源站请求数的最大值（即，不是 DDoS 攻击时的速率），并将其增加几倍 |
  | Edge | 取&#x200B;**正常**&#x200B;流量条件下每个 IP/POP 的最大 Edge 请求数的最大值（即，不是 DDoS 攻击时的速率），并将其增加几倍 |

  有关更多详细信息，另请参阅[选择阈值](../blocking-dos-attack-using-traffic-filter-rules.md#choosing-threshold-values)部分。

- 仅在确认`block`操作不会影响合法流量后移动到`log`操作。

## WAF 规则最佳实践

- 从Adobe [推荐的WAF规则](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-nonwaf-starter-rules)开始，这些规则包括阻止已知不良IP、检测DDoS攻击以及减少机器人滥用的规则。
- `ATTACK` WAF标记应提醒您注意潜在的威胁。 在迁移到`block`之前，请确保没有误报。
- 如果推荐的WAF规则未涵盖特定威胁，请考虑根据应用程序的独特要求创建自定义规则。 请参阅文档中的[WAF标志](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#waf-flags-list)的完整列表。

## 实施规则

了解如何在AEM as a Cloud Service中实施流量过滤器规则和WAF规则：

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
                    <p class="is-size-6">了解如何使用AEM as a Cloud Service中推荐的Web应用程序防火墙(WAF)规则保护AEMAdobe网站免受复杂威胁，包括DoS、DDoS和机器人滥用。</p>
                </div>
                <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">激活WAF</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## 其他资源

- [流量过滤器规则，包括WAF规则](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)
- [了解AEM中的DoS/DDoS预防](https://experienceleague.adobe.com/en/docs/experience-manager-learn/foundation/security/understanding-dos-and-prevention-approaches)
- [使用流量过滤规则阻止DoS和DDoS攻击](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/cloud-service/security/blocking-dos-attack-using-traffic-filter-rules)

