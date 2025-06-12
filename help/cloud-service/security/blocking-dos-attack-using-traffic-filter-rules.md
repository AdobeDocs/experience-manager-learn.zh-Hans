---
title: 使用流量过滤规则阻止 DoS 和 DDoS 攻击
description: 了解如何使用 AEM as a Cloud Servic 提供的 CDN 上的流量过滤规则阻止 DoS 和 DDoS 攻击。
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
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1924'
ht-degree: 100%

---

# 使用流量过滤规则阻止 DoS 和 DDoS 攻击

了解如何在 AEM as a Cloud Service (AEMCS) 托管的 CDN 上使用&#x200B;**速率限制流量过滤**&#x200B;规则和其他策略来阻止拒绝服务 (DoS) 和分布式拒绝服务 (DDoS) 攻击。这些攻击会导致 CDN 流量激增，并可能影响 AEM Publish 服务（即源站）的流量，进而影响网站的响应能力和可用性。

本教程旨在指导您&#x200B;_如何分析流量模式并配置速率限制[流量过滤规则](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)_，以抵御这些攻击。此外，本教程还介绍了如何[配置警报](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#traffic-filter-rules-alerts)，以便在疑似发生攻击时及时通知您。

## 了解保护

让我们了解一下您的 AEM 网站的默认 DDoS 防护：

- **缓存：**&#x200B;借助良好的缓存策略，DDoS 攻击的影响将更为有限，因为内容分发网络能够阻止大部分请求流向源站，从而避免性能下降。
- **自动扩展：**&#x200B;尽管 AEM 的 Author 和 Publish 服务仍可能受到流量突然大幅增加的影响，但它们能够自动扩展，以应对流量激增的情况。
- **阻止：**&#x200B;如果来自特定 IP 地址的流量超过 Adobe 根据每个 CDN 接入点 (PoP) 定义的速率，则 Adobe CDN 将会阻止该流量到达源站。
- **警报：**&#x200B;当流量超过特定速率时，操作中心会发送有关源站流量激增的警报通知。当任何给定 CDN PoP 的流量超过 _Adobe 定义的_&#x200B;每个 IP 地址的请求速率时，会触发此警报。请参阅[流量过滤规则警报](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#traffic-filter-rules-alerts)，以了解更多详情。

这些内置保护措施应被视为组织将 DDoS 攻击对性能的影响降至最低的能力的基准。由于每个网站的性能特征各不相同，且在达到 Adobe 定义的速率限制之前就可能出现性能下降，因此建议通过&#x200B;_客户配置_&#x200B;来扩展默认保护措施。

让我们来看看客户可以采取的另外一些推荐措施，以保护其网站免受 DDoS 攻击：

- 声明&#x200B;**速率限制流量过滤规则**，以阻止每个 PoP 来自单个 IP 地址的流量超过特定速率。这些规则通常比 Adobe 定义的速率限制阈值要低。
- 通过“警报操作”配置速率限制流量过滤规则的&#x200B;**警报**，以便在规则触发时发送操作中心通知。
- 通过声明&#x200B;**请求转换**&#x200B;来忽略查询参数，以提高缓存覆盖率。

### 速率限制流量规则变化 {#rate-limit-variations}

速率限制流量规则有两种变体：

1. Edge - 根据每个 PoP 给定 IP 的所有流量（包括可从内容分发网络缓存中提供的流量）的速率，对请求进行阻拦。
1. 源站 - 根据每个 PoP 指定给源站的流量速率，对给定 IP 的请求进行阻拦。

## 客户历程

以下步骤反映了客户在保护其网站时可能遵循的流程。

1. 认识到需要设置速率限制流量过滤规则。这可能是由于接收到 Adobe 的源站流量激增警报而产生的结果，也可能是为了降低 DDoS 成功攻击的风险而主动采取的预防措施。
1. 如果您的网站已经上线，请使用仪表板分析流量模式，以确定您的速率限制流量过滤规则的最佳阈值。如果您的网站尚未上线，请根据您的流量预期选择值。
1. 根据上一步获取的值，配置速率限制流量过滤规则。确保启用相应的警报，以便在达到阈值时及时通知您。
1. 每当流量激增时，您都会收到流量过滤规则的警报，这为您提供了宝贵的洞察，帮助您了解您的组织是否可能成为恶意行为者的攻击目标。
1. 必要时保持警惕。分析流量，以确定流量激增是否反映了合法请求，而非攻击。如果流量是合法的，则提高阈值；否则，降低阈值。

本教程的其余部分将引导您完成此过程。

## 认识到配置规则的必要性 {#recognize-the-need}

如前所述，Adobe 默认会阻止 CDN 上超过特定速率的流量，但某些网站在低于该阈值时可能会出现性能下降的情况。因此，应配置速率限制流量过滤规则。

理想情况下，您应在投入生产环境之前配置好规则。但在实际操作中，许多组织仅在收到表明可能存在攻击的流量激增警报时，才会被动地声明规则。

当来自单个 IP 地址的流量超过给定 PoP 的默认阈值时，Adobe 会以[操作中心通知](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/operations/actions-center)的形式发送源站流量激增警报。如果您收到此类警报，建议配置流量限制过滤器规则。此默认警报与您在定义流量过滤规则时必须由客户明确启用的警报不同，您将在后续部分中了解这些警报。

## 分析流量模式 {#analyze-traffic}

如果您的网站已经上线，您可以使用 CDN 日志和 Adobe 提供的仪表板来分析流量模式。

- **CDN 流量仪表板**：通过 CDN 和源站请求率、4xx 和 5xx 错误率以及非缓存请求，提供流量洞察。此外，还提供每个客户端 IP 地址每秒的最大 CDN 和源站请求数，以及更多洞察信息，以优化 CDN 配置。

- **CDN 缓存命中率**：提供总缓存命中率以及按 HIT、PASS 和 MISS 状态统计的请求总数。还可提供命中 HIT、PASS 和 MISS 次数最多的 URL。

使用&#x200B;_以下选项之一_&#x200B;配置仪表板工具：

### ELK - 配置仪表板工具

Adobe 提供的 **Elasticsearch、Logstash 和 Kibana (ELK)** 仪表板工具可用于分析 CDN 日志。该工具包含一个可视化流量模式的仪表板，使您能更轻松地确定速率限制流量过滤规则的最佳阈值。

- 克隆 [AEMCS-CDN-Log-Analysis-Tooling](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling) GitHub 存储库。
- 按照[如何设置 ELK Docker 容器](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/ELK/README.md#how-to-set-up-the-elk-docker-containerhow-to-setup-the-elk-docker-container)的步骤来设置工具。
- 作为设置的一部分，请导入 `traffic-filter-rules-analysis-dashboard.ndjson` 文件以实现数据的可视化。_CDN 流量_&#x200B;仪表板包含可视化内容，其中显示 CDN Edge 和源站上每个 IP/POP 的最大请求数。
- 从 [Cloud Manager](https://my.cloudmanager.adobe.com/) 的&#x200B;_环境_&#x200B;卡片中，下载 AEMCS Publish 服务的 CDN 日志。

  ![Cloud Manager CDN 日志下载](./assets/cloud-manager-cdn-log-downloads.png)

  >[!TIP]
  >
  > 新请求可能需要长达 5 分钟才能出现在 CDN 日志中。

### Splunk - 配置仪表板工具

[已启用 Splunk 日志转发功能](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/implementing/developing/logging#splunk-logs)的客户可以创建新的仪表板来分析流量模式。

要在 Splunk 中创建仪表板，请按照[将 Splunk 仪表板用于 AEMCS CDN 日志分析](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/Splunk/README.md#splunk-dashboards-for-aemcs-cdn-log-analysis)的步骤进行操作。

### 查看数据

ELK 和 Splunk 仪表板中提供了以下可视化功能：

- **每个客户端 IP 和 POP 的 Edge RPS**：该可视化图表展示了 **CDN Edge** 每个 IP/POP 的最大请求数。图表中的峰值表示最大请求数。

  **ELK 仪表板**： 
  ![ELK 仪表板 - 每个 IP/POP 的最大请求数](./assets/elk-edge-max-per-ip-pop.png)

  **Splunk 仪表板**：
  ![Splunk 仪表板 - 每个 IP/POP 的最大请求数](./assets/splunk-edge-max-per-ip-pop.png)

- **每个客户端 IP 和 POP 的源站 RPS**：该可视化图表展示了&#x200B;**源站** 每个 IP/POP 的最大请求数。图表中的峰值表示最大请求数。

  **ELK 仪表板**： 
  ![ELK 仪表板 - 每个 IP/POP 的最大源站请求数](./assets/elk-origin-max-per-ip-pop.png)

  **Splunk 仪表板**：
  ![Splunk 仪表板 - 每个 IP/POP 的最大源站请求数](./assets/splunk-origin-max-per-ip-pop.png)

## 选择阈值

速率限制流量过滤规则的阈值应基于上述分析，并确保合法流量不会被拦截。有关如何选择阈值的指导，请参见下表：

| 变体 | 值 |
| :--------- | :------- |
| 源站 | 取&#x200B;**正常**&#x200B;流量条件下每个 IP/POP 的最大源站请求数的最大值（即，不是 DDoS 攻击时的速率），并将其增加几倍 |
| Edge | 取&#x200B;**正常**&#x200B;流量条件下每个 IP/POP 的最大 Edge 请求数的最大值（即，不是 DDoS 攻击时的速率），并将其增加几倍 |

所需倍数取决于您对自然流量、营销活动和其他事件引发的正常流量激增的预期。5-10 之间的倍数可能是合理的。

如果您的网站尚未上线，则没有数据可供分析，您应该基于合理推测来设定速率限制流量过滤规则的适当值。例如：

| 变体 | 值 |
|------------------------------ |:-----------:|
| Edge | 500 |
| 源站 | 100 |

## 配置规则 {#configure-rules}

在 AEM 项目的`/config/cdn.yaml` 文件中配置&#x200B;**速率限制流量过滤**&#x200B;规则，其值应基于上述讨论。如有需要，请咨询您的 Web 安全团队，以确保速率限制值是适当的，且不会阻止合法流量。

有关更多详细信息，请参阅 [AEM 项目中的创建规则](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#create-rules-in-your-aem-project)。

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
    ...
    #  Prevent attack at edge by blocking client for 5 minutes if they make more than 500 requests per second on average
      - name: prevent-dos-attacks-edge
        when:
          reqProperty: tier
          in: ["author","publish"]
        rateLimit:
          limit: 500 # replace with the appropriate value
          window: 10 # compute the average over 10s
          penalty: 300 # block IP for 5 minutes
          count: all # count all requests
          groupBy:
            - reqProperty: clientIp
        action:
          type: log
          alert: true
    #  Prevent attack at origin by blocking client for 5 minutes if they make more than 100 requests per second on average
      - name: prevent-dos-attacks-origin
        when:
          reqProperty: tier
          in: ["author","publish"]
        rateLimit:
          limit: 100 # replace with the appropriate value
          window: 10 # compute the average over 10s
          penalty: 300 # block IP for 5 minutes
          count: fetches # count only fetches
          groupBy:
            - reqProperty: clientIp
        action:
          type: log
          alert: true
```

请注意，这里既声明了源站规则也声明了 Edge 规则，并且将警报属性设置为 `true`，这样每当达到阈值时，你就能收到警报，这很可能表明发生了攻击。

建议最初将操作类型设置为记录，以便您可以在几个小时或几天内监控流量，确保合法流量不会超过这些速率。几天后，改为阻止模式。

按照以下步骤将更改部署到您的 AEMCS 环境中：

- 将上述更改提交并推送到您的 Cloud Manager Git 存储库。
- 使用 Cloud Manager 的配置管道将更改部署到 AEMCS 环境。如需更多详细信息，请参阅 [Cloud Manager 的部署规则](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager)。
- 为了验证&#x200B;**速率限制流量过滤规则**&#x200B;是否按预期工作，您可以按照[攻击模拟](#attack-simulation)部分所述模拟一次攻击。将请求数量限制为高于规则中设置的速率限制值的值。

### 配置请求转换规则 {#configure-request-transform-rules}

除了速率限制流量过滤规则外，建议使用[请求转换](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#request-transformations)来移除应用程序不需要的查询参数，以尽可能减少通过缓存刷新技术绕过缓存的方法。例如，如果您只想允许 `search` 和 `campaignId` 查询参数，则可以声明以下规则：

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  requestTransformations:
    rules:
      - name: unset-all-query-params-except-those-needed
        when:
          reqProperty: tier
          in: ["publish"]
        actions:
          - type: unset
            queryParamMatch: ^(?!search$|campaignId$).*$
```

## 接收流量过滤规则警报 {#receiving-alerts}

如上所述，如果流量过滤规则中包含 *alert: true*，则当规则匹配时，会收到一条警报。

## 根据警报采取行动 {#acting-on-alerts}

有时，警报只是提供信息，以便你了解攻击的频率。使用上述仪表板分析你的 CDN 数据是非常有价值的，这有助于确认流量激增是否由攻击引起，而非合法流量的自然增长。如果是后者，请考虑提高你的阈值。

## 攻击模拟{#attack-simulation}

本节介绍模拟 DoS 攻击的方法，这些方法可用于为本教程中使用的仪表板生成数据，并验证配置的规则是否能成功阻止攻击。

>[!CAUTION]
>
> 请勿在生产环境中执行这些步骤。以下步骤仅用于模拟目的。
>
>如果您收到指示流量激增的警报，请转到[分析流量模式](#analyzing-traffic-patterns)部分。

为了模拟攻击，可以使用 [Apache Benchmark](https://httpd.apache.org/docs/2.4/programs/ab.html)、[Apache JMeter](https://jmeter.apache.org/)、[Vegeta](https://github.com/tsenart/vegeta) 等工具。

### Edge 请求

使用以下 [Vegeta](https://github.com/tsenart/vegeta) 命令，您可以向您的网站发起多个请求：

```shell
$ echo "GET https://<YOUR-WEBSITE-DOMAIN>" | vegeta attack -rate=120 -duration=60s | vegeta report
```

以上命令在 5 秒内发出 120 个请求，并会输出报告。假设网站没有速率限制，这可能会导致流量激增。

### 源站请求

要绕过 CDN 缓存并向源站（AEM Publish 服务）发出请求，您可以在 URL 中添加一个唯一的查询参数。请参考[使用 JMeter 脚本模拟 DoS 攻击](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/foundation/security/modsecurity-crs-dos-attack-protection#simulate-dos-attack-using-jmeter-script)的示例 Apache JMeter 脚本

