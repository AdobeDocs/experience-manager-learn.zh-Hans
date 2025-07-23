---
title: 概述 — 保护 AEM 网站
description: 了解如何在 AEM as a Cloud Service 中使用流量过滤规则（包括其子类别 Web 应用防火墙（WAF）规则）来防护您的 AEM 网站，抵御 DoS、DDoS 攻击以及恶意流量。
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-13148
thumbnail: null
exl-id: e6d67204-2f76-441c-a178-a34798fe266d
source-git-commit: 22a35b008de380bf2f2ef5dfde6743261346df89
workflow-type: ht
source-wordcount: '1185'
ht-degree: 100%

---

# 概述 — 保护 AEM 网站

了解如何在 AEM as a Cloud Service 中使用&#x200B;**流量过滤规则**（包含其子类别 **Web 应用程序防火墙（WAF） 规则）**&#x200B;来保护您的 AEM 网站，防御拒绝服务攻击（DoS）、分布式拒绝服务攻击（DDoS）、恶意流量以及其他更复杂的攻击。

您还将了解标准流量过滤规则与 WAF 流量过滤规则之间的差异、各自的适用场景，以及如何开始使用 Adobe 推荐的规则。

>[!IMPORTANT]
>
> 使用 WAF 流量过滤规则需要额外的 **WAF-DDoS 防护**&#x200B;或&#x200B;**增强安全性（Enhanced Security）**&#x200B;许可证。Sites 和 Forms 客户可默认使用标准流量过滤规则。


>[!VIDEO](https://video.tv.adobe.com/v/3469394/?quality=12&learn=on)

## AEM as a Cloud Service 流量安全简介

AEM as a Cloud Service 通过集成的 CDN 层来保护并优化您网站的内容分发。CDN 层最关键的一个组件是定义并强制执行流量规则。这些规则充当防护屏障，有助于防止滥用、误用及各类攻击，同时不影响站点性能。

流量安全对于保障网站可用性、保护敏感数据以及为合法用户提供顺畅体验至关重要。AEM 提供两类安全规则：

- **标准流量过滤规则**
- **Web 应用程序防火墙（WAF）流量过滤规则**

这些规则集可帮助客户防御常见和复杂的 Web 威胁，减少来自恶意或异常客户端的干扰，并通过记录请求、阻止机制和模式识别提升整体可观测性。

## 标准流量过滤规则与 WAF 流量过滤规则之间的区别

| 功能 | 标准流量过滤规则 | WAF 流量过滤规则 |
|--------------------------|--------------------------------------------------|---------------------------------------------------------|
| 用途 | 用于防止滥用行为，例如 DoS、DDoS 攻击、数据抓取或机器人攻击活动 | 检测并响应复杂攻击模式（例如 OWASP Top 10），同时防御机器人攻击 |
| 示例 | 速率限制、地理封锁、用户代理过滤 | SQL 注入、跨站脚本（XSS）、已知攻击 IP |
| 灵活性 | 通过 YAML 高度灵活地配置 | 通过 YAML 和预定义的 WAF 标志可以非常灵活地进行配置 |
| 推荐模式 | 建议先使用 `log` 模式，然后再切换为 `block` 模式 | 建议为 `ATTACK-FROM-BAD-IP` WAF 标志使用 `block` 模式，为 `ATTACK` WAF 标志使用 `log` 模式，然后统一切换为 `block` 模式 |
| 部署 | 在 YAML 中定义，通过 Cloud Manager 配置管道部署 | 通过 `wafFlags` 在 YAML 中定义，通过 Cloud Manager 配置管道部署 |
| 授予许可 | 包含在 Sites 和 Forms 许可证中 | **需要 WAF-DDoS 防护或增强安全许可证** |

标准流量过滤规则适用于执行业务特定的策略，例如设置速率限制、阻止来自特定地区的访问，或根据请求属性和请求头（如 IP 地址、路径或用户代理）阻止特定流量。相比之下，WAF 流量过滤规则提供针对已知 Web 漏洞和攻击向量的全面主动防护，并具备先进的智能机制以减少误报（即误拦合法流量）。
要定义这两类规则，均需使用 YAML 语法，详见[流量过滤规则语法说明](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#rules-syntax)。

## 何时以及为何使用这些规则

在以下情况下&#x200B;**使用标准流量过滤规则**：

- 需要采用组织特定的限制策略，例如 IP 速率限制。
- 已知需要过滤的特定模式（例如恶意 IP 地址、特定地区、请求头信息等）。

在以下情况下&#x200B;**使用 WAF 流量过滤规则**：

- 您希望获得全面且&#x200B;**主动的防护**，防御广泛传播的已知攻击模式（例如注入攻击、协议滥用）以及来源于专业威胁情报数据源的已知恶意 IP。
- 您希望在拦截恶意请求的同时，最大限度地降低误拦合法流量的风险。
- 您希望采用简单的配置规则，在减少防御工作的同时有效应对常见和复杂的安全威胁。

这些规则共同构建起一个纵深防御策略，使 AEM as a Cloud Service 客户能够采取主动与被动相结合的安全措施来保护其数字资产。

## Adobe 推荐的规则

Adobe 推荐标准流量过滤规则和 WAF 流量过滤规则，帮助您快速保护 AEM 站点的安全。

- **标准流量过滤规则** （默认提供）：用于应对常见的滥用场景，例如针对 **CDN 边缘**&#x200B;或&#x200B;**源端**&#x200B;的 DoS、DDoS 攻击，以及来自受限国家/地区的流量。\
  示例包括：
   - 对&#x200B;_在 CDN 边缘_&#x200B;每秒发起超过 500 次请求的 IP 进行速率限制
   - 对&#x200B;_在源端_&#x200B;每秒发起超过 100 次请求的 IP 进行速率限制
   - 阻止来自美国财政部外国资产控制办公室（OFAC）所列国家/地区的流量

- **WAF 流量过滤规则**（需额外许可证）：提供针对复杂威胁的增强防护，包括 [OWASP Top Ten](https://owasp.org/www-project-top-ten/) 所列的安全风险，如 SQL 注入、跨站点脚本（XSS）以及其他 Web 应用程序攻击。示例包括：
   - 阻止来自已知恶意 IP 地址的请求
   - 记录或阻止被标记为攻击行为的可疑请求

>[!TIP]
>
> 建议首先应用 **Adobe 推荐的规则**，以充分利用 Adobe 的安全专业能力及其持续更新的防护策略。如果您的业务存在特定风险或边缘场景，或发现误报（即拦截了合法流量），您可以定义&#x200B;**自定义规则**，或在默认规则集的基础上进行扩展，以满足您的实际需求。

## 开始使用

通过以下设置指南和用例了解如何在 AEM as a Cloud Service 中定义、部署、测试和分析流量过滤规则（包括 WAF 规则）。这些为您提供了背景知识，使您后续能够自信地应用 Adobe 推荐的规则。

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
                    <a href="./setup.md" title="如何设置流量过滤规则（包括 WAF 规则）" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/setup/rules-setup.png" alt="如何设置流量过滤规则（包括 WAF 规则）"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup.md" target="_self" rel="referrer" title="如何设置流量过滤规则（包括 WAF 规则）">如何设置流量过滤规则（包括 WAF 规则）</a>
                    </p>
                    <p class="is-size-6">了解如何进行设置，以创建、部署、测试和分析流量过滤规则（包括 WAF 规则）的结果。</p>
                </div>
                <a href="./setup.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">立即开始</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Adobe 推荐规则的设置指南

本指南提供分步说明，帮助您在 AEM as a Cloud Service 环境中设置并部署 Adobe 推荐的标准流量过滤规则和 WAF 流量过滤规则。

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
                    <p class="is-size-6">了解如何在 AEM as a Cloud Service 中使用 Adobe 推荐的 Web 应用程序防火墙（WAF）流量过滤规则，防御包括 DoS、DDoS 和机器人滥用在内的复杂安全威胁。</p>
                </div>
                <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">启用 WAF</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## 高级用例

对于更复杂的场景，您可以参考以下用例，了解如何根据具体业务需求实施自定义流量过滤规则：

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
                        <a href="./how-to/request-logging.md" target="_self" rel="referrer" title="监控敏感请求">监控敏感请求</a>
                    </p>
                    <p class="is-size-6">了解如何通过在 AEM as a Cloud Service 中使用流量过滤规则来记录敏感请求，以对敏感请求进行监控。</p>
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
                    <p class="is-size-6">了解如何在 AEM as a Cloud Service 中通过流量过滤规则阻止特定请求以限制访问。</p>
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
                    <a href="./how-to/request-transformation.md" title="将请求标准化" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/how-to/aemrequest-log-transformation.png" alt="将请求标准化"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/request-transformation.md" target="_self" rel="referrer" title="将请求标准化">将请求标准化</a>
                    </p>
                    <p class="is-size-6">了解如何在 AEM as a Cloud Service 中通过流量过滤规则将请求转换为标准化请求。</p>
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

- [流量过滤规则（包括 WAF 规则）](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)
