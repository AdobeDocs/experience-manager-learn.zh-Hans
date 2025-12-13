---
title: 理解 DoS/DDoS 防护
description: 了解如何预防和减轻针对 AEM 的 DoS 和 DDoS 攻击。
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Security
topic: Security, Development
role: Admin, Developer
level: Beginner
doc-type: Article
duration: 75
last-substantial-update: 2024-03-30T00:00:00Z
jira: KT-15219
exl-id: 1d7dd829-e235-4884-a13f-b6ea8f6b4b0b
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 100%

---

# 了解 AEM 中的 DoS/DDoS 防护

了解可用于防止和减轻针对 AEM 环境的 DoS 和 DDoS 攻击的选项。在深入防护机制之前，先简要了解 [DoS](https://developer.mozilla.org/en-US/docs/Glossary/DOS_attack) 与 [DDoS](https://developer.mozilla.org/en-US/docs/Glossary/Distributed_Denial_of_Service) 的基本概念。

- DoS（拒绝服务）攻击和 DDoS（分布式拒绝服务）攻击都是恶意行为，旨在破坏目标服务器、服务或网络的正常运行，使其对合法用户无法访问。
- DoS 攻击通常源自单一攻击源，而 DDoS 攻击则来自多个攻击源。
- 由于多个攻击设备资源的联合，DDoS 攻击的规模通常比 DoS 攻击更大。
- 这些攻击是通过向目标发送大量流量，并利用网络协议中的漏洞来实施的。

下表描述了如何预防和缓解 DoS 和 DDoS 攻击：

<table>
    <tbody>
        <tr>
            <td><strong>预防机制</strong></td>
            <td><strong>描述</strong></td>
            <td><strong>AEM as a Cloud Service</strong></td>
            <td><strong>AEM 6.5 (AMS)</strong></td>
            <td><strong>AEM 6.5（本地）</strong></td>
        </tr>
        <tr>
            <td>Web 应用程序防火墙 (WAF)</td>
            <td>一种旨在保护网络应用免受多种类型攻击的安全解决方案。</td>
            <td>
            <a href="https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/examples-and-analysis#waf-rules" target="_blank">WAF-DDoS 保护许可证</a></td>
            <td>通过 AMS 合同使用 <a href="https://docs.aws.amazon.com/waf/" target="_blank">AWS</a> 或 <a href="https://azure.microsoft.com/en-us/products/web-application-firewall" target="_blank">Azure</a> 的 WAF。</td>
            <td>您首选的 WAF</td>
        </tr>
        <tr>
            <td>ModSecurity</td>
            <td>ModSecurity（又名“mod_security”Apache 模块）是一个开源、跨平台的解决方案，能够防护多种针对网络应用的攻击。<br/> 在 AEM as a Cloud Service 中，此功能仅适用于 AEM Publish 服务，因为 AEM Author 服务前端没有 Apache Web 服务器和 AEM Dispatcher。</td>
            <td colspan="3"><a href="https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/foundation/security/modsecurity-crs-dos-attack-protection" target="_blank">启用 ModSecurity </a></td>
        </tr>
        <tr>
            <td>流量过滤规则</td>
            <td>流量过滤规则可用于在 CDN 层阻止或允许请求。</td>
            <td><a href="https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/examples-and-analysis" target="_blank">流量过滤规则示例</a></td>
            <td><a href="https://docs.aws.amazon.com/waf/latest/developerguide/waf-rule-statement-type-rate-based.html" target="_blank">AWS</a> 或 <a href="https://learn.microsoft.com/en-us/azure/web-application-firewall/ag/rate-limiting-overview" target="_blank">Azure</a> 的规则限制功能。</td>
            <td>您的首选解决方案</td>
        </tr>
    </tbody>
</table>

## 事件后分析与持续改进

虽然识别和预防 DoS/DDoS 攻击并没有一个放之四海而皆准的标准流程，但这取决于您的组织的安全流程。**事件后分析和持续改进**&#x200B;是这一流程中的关键步骤。以下是一些值得考虑的最佳实践：

- 通过进行事件后分析，包括审查日志、网络流量和系统配置，确定 DoS/DDoS 攻击的根本原因。
- 根据事件后分析的结果，完善预防机制。

