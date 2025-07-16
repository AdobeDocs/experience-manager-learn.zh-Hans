---
title: 使用WAF规则保护AEM网站
description: 了解如何使用AEM as a Cloud Service中推荐的Web应用程序防火墙(WAF)规则保护AEMAdobe网站免受复杂威胁，包括DoS、DDoS和机器人滥用。
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
badgeLicense: label="需要许可证" type="positive" before-title="true"
jira: KT-18308
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '1376'
ht-degree: 7%

---

# 使用WAF规则保护AEM网站

了解如何在AEM as a Cloud Service中使用&#x200B;_Adobe推荐的_ **Web应用程序防火墙(WAF)规则**&#x200B;来保护AEM网站免受复杂威胁，包括DoS、DDoS和机器人滥用。

复杂攻击的特点是请求率高、模式复杂，并使用高级技术绕过传统的安全措施。

>[!IMPORTANT]
>
> WAF流量过滤器规则需要额外的&#x200B;**WAF-DDoS保护**&#x200B;或&#x200B;**增强的安全性**&#x200B;许可证。 默认情况下，Sites和Forms客户可以使用标准流量过滤器规则。

## 学习目标

- 查看Adobe推荐的WAF规则。
- 定义、部署、测试和分析规则的结果。
- 了解何时以及如何根据结果优化规则。
- 了解如何使用AEM操作中心查看由规则生成的警报。

### 实施概述

实施步骤包括：

- 将WAF规则添加到AEM WKND项目的`/config/cdn.yaml`文件。
- 提交更改并将其推送到Cloud Manager Git存储库。
- 使用Cloud Manager配置管道将更改部署到AEM环境。
- 通过使用[Nikto](https://github.com/sullo/nikto/wiki)模拟DDoS攻击来测试规则。
- 使用AEMCS CDN日志和ELK仪表板工具对结果进行分析。

## 先决条件

在继续之前，请确保您已按照[如何设置流量过滤器和WAF规则](../setup.md)教程中的说明完成所需的设置。 此外，您已克隆[AEM WKND Sites项目](https://github.com/adobe/aem-guides-wknd)并将其部署到您的AEM环境。

## 查看和定义规则

Adobe推荐的Web应用程序防火墙(WAF)规则对于保护AEM网站免受复杂威胁（包括DoS、DDoS和机器人滥用）至关重要。 复杂的攻击通常具有高请求率、复杂模式以及使用高级技术（基于协议或基于有效负载的攻击）绕过传统安全措施的特点。

让我们查看三个建议的WAF规则，这些规则应添加到AEM WKND项目的`cdn.yaml`文件中：

### 1.阻止来自已知恶意IP的攻击

此规则&#x200B;**阻止看起来可疑的**&#x200B;和&#x200B;*的*&#x200B;请求，这些请求均来自标记为恶意的IP地址。 由于同时满足这两个标准，我们可以确信误报（阻止合法流量）的风险非常低。 已知不良IP根据威胁情报信息源和其他来源进行识别。

`ATTACK-FROM-BAD-IP` WAF标志用于标识这些请求。 它汇总了此处[列出的若干WAF标志](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#waf-flags-list)。

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  trafficFilters:
    rules:
    - name: attacks-from-bad-ips-globally
      when:
        reqProperty: tier
        in: ["author", "publish"]
      action:
        type: block
        wafFlags:
          - ATTACK-FROM-BAD-IP
```

### 2.全局记录（及以后阻止）来自任何IP的攻击

此规则&#x200B;**记录**&#x200B;已识别为潜在攻击的请求，即使未在威胁智能信息源中找到IP地址也是如此。

`ATTACK` WAF标志用于标识这些请求。 与`ATTACK-FROM-BAD-IP`类似   聚合多个WAF标志。

这些请求可能是恶意的，但由于IP地址未在威胁智能信息源中识别，因此谨慎的做法是以`log`模式而不是块模式启动。 分析日志中的误报，一旦验证，**请确保将规则切换到`block`模式**。

```yaml
...
    - name: attacks-from-any-ips-globally
      when:
        reqProperty: tier
        in: ["author", "publish"]
      action:
        type: log
        alert: true
        wafFlags:
          - ATTACK
```

或者，如果您的业务要求不允许恶意流量，您可以选择立即使用`block`模式。

这些推荐的WAF规则针对已知和新出现的威胁提供了额外的安全层。

![WKND WAF规则](../assets/use-cases/wknd-cdn-yaml-waf-rules.png)

## 迁移至Adobe推荐的最新WAF规则

在引入`ATTACK-FROM-BAD-IP`和`ATTACK` WAF标志（2025年7月）之前，建议的WAF规则如下。 它们包含特定WAF标志的列表，以阻止符合特定条件（如`SANS`、`TORNODE`、`NOUA`等）的请求。

```yaml
...
data:
  trafficFilters:
    rules:
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
...
```

上述规则仍然有效，但建议您迁移到使用`ATTACK-FROM-BAD-IP`和`ATTACK` WAF标志&#x200B;_的新规则，前提是尚未自定义`wafFlags`以满足您的业务要求_。

您可以按照以下步骤迁移到新规则以符合最佳实践：

- 查看`cdn.yaml`文件中的现有WAF规则，这些规则可能与上面的示例类似。 确认没有针对您的业务需求自定义`wafFlags`。

- 将您现有的WAF规则替换为新的Adobe推荐的WAF规则，这些规则使用`ATTACK-FROM-BAD-IP`和`ATTACK`标志。 确保所有规则都处于块模式。

如果您之前已自定义`wafFlags`，则仍可迁移到这些新规则，但请仔细操作，以确保任何自定义项都提前到修订后的规则中。

迁移应有助于简化WAF规则，同时仍提供针对复杂威胁的强大保护。 新规则旨在更有效、更易于管理。


## 部署规则

要部署上述规则，请执行以下步骤：

- 将更改提交并推送到 Cloud Manager Git 存储库。

- 使用之前创建的AEM配置管道[将更改部署到Cloud Manager环境](../setup.md#deploy-rules-using-adobe-cloud-manager)。

  ![Cloud Manager 配置管道](../assets/use-cases/cloud-manager-config-pipeline.png)

## 测试规则

为了验证WAF规则的有效性，请使用[Nikto](https://github.com/sullo/nikto)模拟攻击，该Web服务器扫描程序可检测漏洞和错误配置。 以下命令会触发针对AEM WKND网站的SQL注入攻击，该网站受WAF规则保护。

```shell
$./nikto.pl -useragent "AttackSimulationAgent (Demo/1.0)" -D V -Tuning 9 -ssl -h https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html
```

![Nikto 攻击模拟](../assets/use-cases/nikto-attack.png)

要了解攻击模拟，请查阅 [Nikto - 扫描调整](https://github.com/sullo/nikto/wiki/Scan-Tuning)文档，其中介绍了如何指定要包含或排除的测试攻击类型。

## 查看警报

触发流量过滤器规则时会生成警报。 您可以在[AEM操作中心](https://experience.adobe.com/aem/actions-center)中查看这些警报。

![WKND AEM操作中心](../assets/use-cases/wknd-aem-action-center.png)

## 分析结果

要分析流量过滤规则的结果，您可以使用AEMCS CDN日志和ELK仪表板工具。 按照[CDN日志摄取](../setup.md#ingest-cdn-logs)设置部分中的说明将CDN日志摄取到ELK栈栈中。

在以下屏幕截图中，您可以看到AEM开发环境的CDN日志被摄取到ELK栈栈中。

![WKND CDN日志ELK](../assets/use-cases/wknd-cdn-logs-elk-waf.png)

在ELK应用程序内，**WAF仪表板**&#x200B;应显示
客户端IP (cli_ip)、主机、URL、操作(waf_action)和规则名称(waf_match)列中标记的请求和相应值。

![WKND WAF仪表板ELK](../assets/use-cases/elk-tool-dashboard-waf-flagged.png)

此外，**WAF 标志分布**&#x200B;和&#x200B;**热门攻击**&#x200B;面板还展示了更多详细信息。

![WKND WAF仪表板ELK](../assets/use-cases/elk-tool-dashboard-waf-flagged-top-attacks-1.png)

![WKND WAF仪表板ELK](../assets/use-cases/elk-tool-dashboard-waf-flagged-top-attacks-2.png)

![WKND WAF仪表板ELK](../assets/use-cases/elk-tool-dashboard-waf-flagged-top-attacks-3.png)

### Splunk集成

[已启用 Splunk 日志转发功能](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/implementing/developing/logging#splunk-logs)的客户可以创建新的仪表板来分析流量模式。

要在 Splunk 中创建仪表板，请按照[将 Splunk 仪表板用于 AEMCS CDN 日志分析](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/Splunk/README.md#splunk-dashboards-for-aemcs-cdn-log-analysis)的步骤进行操作。

## 何时以及如何优化规则

您的目标是避免阻止合法流量，同时仍可保护AEM网站免受复杂威胁的侵扰。 推荐的WAF规则旨在作为您安全策略的起点。

要优化规则，请考虑以下步骤：

- **监测流量模式**：使用CDN日志和ELK仪表板监测流量模式，并识别任何异常或流量激增。 请注意ELK仪表板中的&#x200B;_WAF标记分布_&#x200B;和&#x200B;_热门攻击_&#x200B;面板，以了解检测到的攻击类型。
- **调整wafFlags**：如果太频繁地触发`ATTACK`标志，或者
您需要优化攻击向量，可以创建具有特定WAF标志的自定义规则。 请参阅文档中的[WAF标志](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#waf-flags-list)的完整列表。 考虑首先在`log`模式下尝试新的自定义规则。
- **移至阻止规则**：验证流量模式并调整WAF标记后，可以考虑移至阻止规则。

## 摘要

在本教程中，您已了解如何使用Adobe推荐的Web应用程序防火墙(WAF)规则保护AEM网站免受复杂威胁，包括DoS、DDoS和机器人滥用。

## 用例 — 超出标准规则

对于更高级的场景，您可以探索以下用例，演示如何根据特定业务需求实施自定义流量过滤器规则：

<!-- CARDS
{target = _self}

* ../how-to/request-logging.md

* ../how-to/request-blocking.md

* ../how-to/request-transformation.md
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Monitoring sensitive requests">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="../how-to/request-logging.md" title="监控敏感请求" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/how-to/wknd-login.png" alt="监控敏感请求"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="../how-to/request-logging.md" target="_self" rel="referrer" title="监控敏感请求">正在监视敏感请求</a>
                    </p>
                    <p class="is-size-6">了解如何使用AEM as a Cloud Service中的流量过滤器规则记录敏感请求，从而监控这些请求。</p>
                </div>
                <a href="../how-to/request-logging.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解详情</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Restricting access">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="../how-to/request-blocking.md" title="限制访问" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/how-to/elk-tool-dashboard-blocked.png" alt="限制访问"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="../how-to/request-blocking.md" target="_self" rel="referrer" title="限制访问">限制访问</a>
                    </p>
                    <p class="is-size-6">了解如何使用AEM as a Cloud Service中的流量过滤规则阻止特定请求来限制访问。</p>
                </div>
                <a href="../how-to/request-blocking.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解详情</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Normalizing requests">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="../how-to/request-transformation.md" title="标准化请求" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/how-to/aemrequest-log-transformation.png" alt="标准化请求"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="../how-to/request-transformation.md" target="_self" rel="referrer" title="标准化请求">标准化请求</a>
                    </p>
                    <p class="is-size-6">了解如何使用AEM as a Cloud Service中的流量过滤器规则转换请求，从而标准化请求。</p>
                </div>
                <a href="../how-to/request-transformation.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解详情</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## 其他资源

- [推荐的入门规则](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-nonwaf-starter-rules)
- [WAF标记列表](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#waf-flags-list)
