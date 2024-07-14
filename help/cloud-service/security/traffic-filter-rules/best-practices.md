---
title: 流量过滤器规则（包括WAF规则）的最佳实践
description: 了解流量过滤器规则（包括WAF规则）的建议最佳实践。
version: Cloud Service
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
source-git-commit: c7c78ca56c1d72f13d2dc80229a10704ab0f14ab
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 0%

---

# 流量过滤器规则（包括WAF规则）的最佳实践

了解流量过滤器规则（包括WAF规则）的建议最佳实践。 请务必注意，本文中描述的最佳实践并非详尽无遗，也不会替代您自己的安全策略和程序。

>[!VIDEO](https://video.tv.adobe.com/v/3425408?quality=12&learn=on)

## 一般最佳实践

- 要确定哪些规则适用于您的组织，请与您的安全团队协作。
- 始终先在开发环境中测试规则，然后再将其部署到暂存和生产环境。
- 声明和验证规则时，始终以`action`类型`log`开头，以确保该规则不会阻止合法流量。
- 对于某些规则，从`log`到`block`的过渡应完全基于对足够网站流量的分析。
- 以增量方式引入规则，并考虑让测试团队（QA、性能、渗透测试）参与该过程。
- 使用[仪表板工具](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling)定期分析规则的影响。 根据您站点的流量，可以每天、每周或每月执行分析。
- 要阻止分析后可能察觉到的恶意流量，请添加任何其他规则。 例如，某些IP一直在攻击您的站点。
- 规则的创建、部署和分析应该是一个持续的迭代过程。 这不是一次性活动。

## 流量过滤器规则的最佳实践

为您的AEM项目启用下面的流量过滤器规则。 但是，`rateLimit`和`clientCountry`属性的所需值必须与您的安全团队协作确定。

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
>对于您的生产环境，请与Web安全团队协作以确定`rateLimit`的适当值

## WAF规则的最佳实践

一旦为程序授予了WAF许可并启用了，便会在图表和请求日志中显示与WAF标志匹配的流量，即使您未在规则中声明这些流量也是如此。 因此，您始终了解潜在的新恶意流量，并且可以根据需要创建规则。 查看未反映在声明的规则中的WAF标记，并考虑声明它们。

请考虑下面适用于您的AEM项目的WAF规则。 但是，`action`和`wafFlags`属性的所需值必须与您的安全团队协作确定。

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

总之，本教程已为您提供了在Adobe Experience Manager as a Cloud Service (AEMCS)中增强Web应用程序安全性所需的知识和工具。 通过实用的规则示例和对结果分析的洞察，您可以有效地保护您的网站和应用程序。



