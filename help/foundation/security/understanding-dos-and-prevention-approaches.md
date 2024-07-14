---
title: 了解DoS/DDoS预防
description: 了解如何防止和缓解针对AEM的DoS和DDoS攻击。
version: 6.5, Cloud Service
feature: Security
topic: Security, Development
role: Admin, Architect, Developer
level: Beginner
doc-type: Article
duration: 75
last-substantial-update: 2024-03-30T00:00:00Z
jira: KT-15219
exl-id: 1d7dd829-e235-4884-a13f-b6ea8f6b4b0b
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 1%

---

# 了解AEM中的DoS/DDoS预防

了解可用于防止和减轻AEM环境上的DoS和DDoS攻击的选项。 在深入研究预防机制之前，请简要概述[DoS](https://developer.mozilla.org/en-US/docs/Glossary/DOS_attack)和[DDoS](https://developer.mozilla.org/en-US/docs/Glossary/Distributed_Denial_of_Service)。

- DoS（拒绝服务）和DDoS（分布式拒绝服务）攻击都是恶意尝试破坏目标服务器、服务或网络的正常功能，使其目标用户无法访问这些服务器、服务或网络。
- DoS攻击通常来自单一来源，而DDoS攻击来自多个来源。
- DDoS攻击通常比DoS攻击规模更大，因为多个攻击设备的资源组合在一起。
- 这些攻击是通过向目标泛滥过多的通信量，并利用网络协议中的漏洞来实现的。

下表介绍了如何防止和减少DoS和DDoS攻击：

<table>
    <tbody>
        <tr>
            <td><strong>预防机制</strong></td>
            <td><strong>描述</strong></td>
            <td><strong>AEM as a Cloud Service</strong></td>
            <td><strong>AEM 6.5 (AMS)</strong></td>
            <td><strong>AEM 6.5（内部部署）</strong></td>
        </tr>
        <tr>
            <td>Web应用程序防火墙(WAF)</td>
            <td>旨在保护Web应用程序免受各种攻击的安全解决方案。</td>
            <td>
            <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/examples-and-analysis#waf-rules" target="_blank">WAF-DDoS保护许可证</a></td>
            <td>通过AMS合同<a href="https://docs.aws.amazon.com/waf/" target="_blank">AWS</a>或<a href="https://azure.microsoft.com/en-us/products/web-application-firewall" target="_blank">Azure</a> WAF。</td>
            <td>您的首选WAF</td>
        </tr>
        <tr>
            <td>修改安全性</td>
            <td>ModSecurity（又称“mod_security”Apache模块）是一种开源、跨平台的解决方案，可抵御针对Web应用程序的一系列攻击。<br/>在AEM as a Cloud Service中，这仅适用于AEM Publish服务，因为在AEM Author服务之前没有AEM Web Server和Apache Dispatcher。</td>
            <td colspan="3"><a href="https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/foundation/security/modsecurity-crs-dos-attack-protection" target="_blank">启用Modsecurity </a></td>
        </tr>
        <tr>
            <td>流量过滤器规则</td>
            <td>流量过滤器规则可用于在CDN层阻止或允许请求。</td>
            <td><a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/examples-and-analysis" target="_blank">流量过滤器规则示例</a></td>
            <td><a href="https://docs.aws.amazon.com/waf/latest/developerguide/waf-rule-statement-type-rate-based.html" target="_blank">AWS</a>或<a href="https://learn.microsoft.com/en-us/azure/web-application-firewall/ag/rate-limiting-overview" target="_blank">Azure</a>规则限制功能。</td>
            <td>您的首选解决方案</td>
        </tr>
    </tbody>
</table>

## Post事件分析和持续改进

虽然在识别和防御DoS/DDoS攻击方面没有一个万能的标准流程，但这取决于贵组织的安全流程。 **事后分析和持续改进**&#x200B;是此过程中的关键步骤。 以下是需要考虑的一些最佳实践：

- 通过事件后分析（包括审查日志、网络通信和系统配置）来确定DoS/DDoS攻击的根本原因。
- 根据事件后分析的结果，改进预防机制。

