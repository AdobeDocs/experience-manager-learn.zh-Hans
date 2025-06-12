---
title: 流量过滤规则（包括 WAF 规则）最佳实践
description: 了解流量过滤规则（包括 WAF 规则）的推荐最佳实践。
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: 4a7acdd2-f442-44ee-8560-f9cb64436acf
duration: 170
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '411'
ht-degree: 100%

---

# 流量过滤规则（包括 WAF 规则）最佳实践

了解流量过滤规则（包括 WAF 规则）的推荐最佳实践。需要注意的是，本文中描述的最佳实践并不全面，且不应替代您自身的安全政策和流程。

>[!VIDEO](https://video.tv.adobe.com/v/3425408?quality=12&learn=on)

## 通用最佳实践

- 要确定适合您组织的规则，请与您的安全团队协作。
- 请务必先在开发环境中测试规则，然后再部署到预发布和生产环境。
- 在声明和验证规则时，务必从 `action` 类型的 `log` 开始，以确保规则不会阻挡合法流量。
- 对于某些规则，从 `log` 过渡到 `block` 应完全基于对充足网站流量的分析。
- 逐步引入规则，并考虑在此过程中让测试团队（质量保证、性能测试、渗透测试）参与。
- 定期使用[仪表板工具](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling)分析规则的影响。根据您网站的流量规模，分析可以每日、每周或每月进行。
- 在分析后，如发现已知恶意流量，可添加额外规则进行阻断。例如，某些曾对您的网站发动攻击的 IP 地址。
- 规则的创建、部署和分析应当是一个持续且迭代的过程。这不是一次性的工作。

## 流量过滤规则最佳实践

为您的 AEM 项目启用以下流量过滤规则。然而，`rateLimit` 和 `clientCountry` 属性的期望值必须与您的安全团队共同确定。

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  trafficFilters:
    rules:
    #  Prevent DoS attacks by blocking client for 5 minutes if they make more than 100 requests in 1 second.
      - name: prevent-dos-attacks
        when:
          reqProperty: path
          like: '*'
        rateLimit:
          limit: 100
          window: 1
          penalty: 300
          groupBy:
            - reqProperty: clientIp
        action: block
    # Block requests coming from OFAC countries
      - name: block-ofac-countries
        when:
          allOf:
              - reqProperty: tier
              - matches: publish
              - reqProperty: clientCountry
                in:
                  - SY
                  - BY
                  - MM
                  - KP
                  - IQ
                  - CD
                  - SD
                  - IR
                  - LR
                  - ZW
                  - CU
                  - CI
```

>[!WARNING]
>
>对于生产环境，请与您的网络安全团队合作，确定 `rateLimit` 的合适取值

## WAF 规则最佳实践

一旦为您的项目授权并启用 WAF，符合 WAF 标志的流量将出现在图表和请求日志中，即使您未在规则中声明这些标志也是如此。这样，您可以随时了解潜在的新恶意流量，并根据需要创建相应规则。请关注未在已声明规则中体现的 WAF 标记，并考虑将其纳入规则声明。

请考虑为您的 AEM 项目采用以下 WAF 规则。然而，`action` 和 `wafFlags` 属性的期望值必须与您的安全团队共同确定。

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  trafficFilters:
    rules:

    # Traffic Filter rules shown in above section
    ...

    # Enable WAF protections (only works if WAF is enabled for your environment)
      - name: block-waf-flags
        when:
          reqProperty: tier
          matches: "author|publish"
        action:
          type: block
          wafFlags:
            - SANS
            - TORNODE
            - NOUA
            - SCANNER
            - USERAGENT
            - PRIVATEFILE
            - ABNORMALPATH
            - TRAVERSAL
            - NULLBYTE
            - BACKDOOR
            - LOG4J-JNDI
            - SQLI
            - XSS
            - CODEINJECTION
            - CMDEXE
            - NO-CONTENT-TYPE
            - UTF8
    # Disable protection against CMDEXE on /bin
      - name: allow-cdmexe-on-root-bin
        when:
          allOf:
            - reqProperty: tier
              matches: "author|publish"
            - reqProperty: path
              matches: "^/bin/.*"
        action:
          type: allow
          wafFlags:
            - CMDEXE
```

## 摘要

总结来说，本教程为您提供了加强 Adobe Experience Manager as a Cloud Service (AEMCS) 中 Web 应用程序安全性所需的知识和工具。通过实用的规则示例和有关结果分析的深入见解，您可以有效地保护您的网站和应用程序。



