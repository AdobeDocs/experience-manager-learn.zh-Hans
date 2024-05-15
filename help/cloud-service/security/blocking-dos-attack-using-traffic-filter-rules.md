---
title: 使用流量过滤规则阻止 DoS 和 DDoS 攻击
description: 了解如何在AEMas a Cloud Service提供的CDN上使用流量过滤器规则阻止DoS和DDoS攻击。
version: Cloud Service
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
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1918'
ht-degree: 1%

---

# 使用流量过滤规则阻止 DoS 和 DDoS 攻击

了解如何使用阻止拒绝服务(DoS)和分布式拒绝服务(DDoS)攻击 **速率限制流量过滤器** AEMas a Cloud Service(AEMCS)托管的CDN中的规则和其他策略。 这些攻击会导致CDN和潜在的AEM Publish服务（也称为来源）出现流量尖峰，并可能影响站点响应性和可用性。

本教程将指导您完成以下操作 _如何分析流量模式并配置速率限制 [流量过滤器规则](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)_ 来减轻这些攻击。 本教程还介绍了如何 [配置警报](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#traffic-filter-rules-alerts) 以便在怀疑有攻击时通知您。

## 了解保护

让我们了解一下AEM网站的默认DDoS保护：

- **缓存：** 在良好的缓存策略下，DDoS攻击的影响会更加有限，因为CDN会阻止大多数请求访问源并造成性能下降。
- **自动缩放：** AEM创作和发布服务会自动扩展以处理流量尖峰，但它们仍可能会受到流量突然大量增加的影响。
- **阻止：** 如果AdobeCDN超过特定IP地址中每个CDN PoP(Point of Presence)的Adobe定义速率，则该CDN会阻止发往源的流量。
- **警报：** 当流量超过特定速率时，操作中心会在源位置发送流量尖峰警报通知。 当流向任何给定CDN Po的流量超过 _Adobe定义的_ 每个IP地址的请求速率。 请参阅 [流量过滤器规则警报](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#traffic-filter-rules-alerts) 以了解更多详细信息。

这些内置的保护应被视为组织将DDoS攻击的性能影响降至最低的能力的基准。 由于每个网站都具有不同的性能特征，并且在达到Adobe定义的速率限制之前可能会出现性能下降，因此建议通过以下方式扩展默认保护 _客户配置_.

我们来看看客户可以采取哪些其他建议措施来保护其网站免受DDoS攻击：

- 声明 **速率限制流量过滤器规则** 阻止来自单个IP地址的流量超过特定速率（每个PoP）。 这些阈值通常比Adobe定义的速率限制低。
- 配置 **警报** 速率限制流量过滤器规则是通过“警报操作”执行的，因此，当触发规则时，将发送操作中心通知。
- 通过声明增加缓存覆盖范围 **请求转换** 以忽略查询参数。

>[!NOTE]
>
>此 [流量过滤器规则警报](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#traffic-filter-rules-alerts) 功能尚未发布。 要通过率先采用者计划获得访问权限，请发送电子邮件至 **<aemcs-waf-adopter@adobe.com>**.

### 速率限制流量规则变化 {#rate-limit-variations}

速率限制流量规则有两种变体：

1. Edge — 基于给定IP每个Po的所有流量（包括可从CDN缓存中提供的流量）的速率来阻止请求。
1. 来源 — 根据指定IP的每个PoP发往来源的数据流的速率来阻止请求。

## 客户历程

以下步骤反映了客户保护其网站的可能过程。

1. 识别是否需要速率限制流量过滤器规则。 这可能是由于收到Adobe的开箱即用流量尖峰原始警报，也可能是主动决定采取预防措施以降低DDo成功的风险。
1. 使用功能板（如果您的网站已上线）分析流量模式，以确定速率限制流量过滤器规则的最佳阈值。 如果您的网站尚未上线，请根据流量预期选择值。
1. 使用上一步的值，配置速率限制流量过滤器规则。 确保启用相应的警报，以便在达到阈值时通知您。
1. 每当发生流量尖峰时，接收流量过滤器规则警报，这为您提供了关于您的组织是否可能被恶意行为者定位的宝贵见解。
1. 根据需要对警报执行操作。 分析流量以确定尖峰是否反映合法请求而非攻击。 如果流量是合法的，则增加阈值；否则降低阈值。

本教程的其余部分将指导您完成此过程。

## 认识到需要配置规则 {#recognize-the-need}

如前所述，默认情况下，Adobe会阻止CDN上超过特定速率的流量，但是，某些网站可能会遇到低于该阈值的性能下降问题。 因此，应配置速率限制流量过滤器规则。

理想情况下，您最好在投入生产之前配置规则。 在实践中，许多组织只会在收到流量尖峰警报（表示可能发生攻击）后反应性地声明规则。

Adobe在源位置发送流量尖峰警报，作为 [操作中心通知](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/operations/actions-center) 超过给定PoP中来自单个IP地址的流量默认阈值时。 如果收到此类警报，建议配置速率限制流量过滤器规则。 此默认警报不同于客户在定义流量过滤器规则时必须明确启用的警报，您将在以后的部分中了解这些警报。


## 分析流量模式 {#analyze-traffic}

如果您的站点已上线，则可以使用CDN日志和以下方法之一分析流量模式：

### ELK — 配置仪表板工具

此 **Elasticsearch、Logstash和Kibana (ELK)** Adobe提供的功能板工具可用于分析CDN日志。 此工具包括可可视化流量模式的功能板，使得更容易确定速率限制流量过滤器规则的最佳阈值。

- 克隆 [AEMCS-CDN-Log-Analysis-ELK-Tool](https://github.com/adobe/AEMCS-CDN-Log-Analysis-ELK-Tool) github存储库。
- 按照以下步骤设置刀具 [如何设置ELK Docker容器](https://github.com/adobe/AEMCS-CDN-Log-Analysis-ELK-Tool?tab=readme-ov-file#how-to-set-up-the-elk-docker-container) 步骤。
- 在设置过程中，导入 `traffic-filter-rules-analysis-dashboard.ndjson` 文件以可视化数据。 此 _CDN流量_ 功能板中包含一些可视化图表，这些可视化图表显示CDN Edge和Origin中每个IP/POP的最大请求数。
- 从 [Cloud Manager](https://my.cloudmanager.adobe.com/)的 _环境_ 卡片，下载AEMCS发布服务的CDN日志。

  ![Cloud Manager CDN日志下载](./assets/cloud-manager-cdn-log-downloads.png)

  >[!TIP]
  >
  > 新请求可能最多需要5分钟才能显示在CDN日志中。

### Splunk — 配置功能板工具

客户具有 [已启用Splunk日志转发](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/logging#splunk-logs) 可以创建一个新仪表板来分析流量模式。 以下XML文件可帮助您在Splunk中创建仪表板：

- [CDN — 流量仪表板](./assets/traffic-dashboard.xml)：此仪表板提供有关CDN Edge和Origin的流量模式的分析。 该可视化图表显示了CDN Edge和Origin中每个IP/POP的最大请求数。

### 查看数据

ELK和Splunk功能板中提供了以下可视化图表：

- **每个客户端IP和POP的边缘RPS**：此可视化图表显示每个IP/POP的最大请求数 **在CDN Edge**. 可视化图表中的峰值表示最大请求数。

  **麋鹿仪表板**：
  ![ELK功能板 — 每个IP/POP的最大请求数](./assets/elk-edge-max-per-ip-pop.png)

  **Splunk功能板**：\
  ![Splunk功能板 — 每个IP/POP的最大请求数](./assets/splunk-edge-max-per-ip-pop.png)

- **每个客户端IP和POP的源RPS**：此可视化图表显示每个IP/POP的最大请求数 **在源位置**. 可视化图表中的峰值表示最大请求数。

  **麋鹿仪表板**：
  ![ELK功能板 — 每个IP/POP的最大源请求数](./assets/elk-origin-max-per-ip-pop.png)

  **Splunk功能板**：
  ![Splunk功能板 — 每个IP/POP的最大源请求数](./assets/splunk-origin-max-per-ip-pop.png)

## 选择阈值

速率限制流量过滤器规则的阈值应基于以上分析，并确保合法流量不被阻止。 有关如何选择阈值的指导，请参阅下表：

| 变体 | 价值 |
| :--------- | :------- |
| 来源 | 获取下的每个IP/POP的最大源请求数的最大值 **普通** 流量条件（即，不是DDoS时的速率）并将其增加倍数 |
| Edge | 获取下的每个IP/POP的最大边缘请求数最大值 **普通** 流量条件（即，不是DDoS时的速率）并将其增加倍数 |

要使用的倍数取决于您对因自然流量、营销活动和其他活动而出现正常流量尖峰的期望。 5到10的倍数可能是合理的。

如果您的网站尚未上线，则没有要分析的数据，您应该根据情况推定要为速率限制流量过滤器规则设置的相应值。 例如：

| 变体 | 价值 |
|------------------------------ |:-----------:|
| Edge | 500 |
| 来源 | 100 |

## 配置规则 {#configure-rules}

配置 **速率限制流量过滤器** AEM项目中的规则 `/config/cdn.yaml` 文件，其值基于上面的讨论。 如果需要，请咨询Web安全团队，确保速率限制值正确且不会阻止合法流量。

请参阅 [在AEM项目中创建规则](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#create-rules-in-your-aem-project) 以了解更多详细信息。

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
          experimental_alert: true
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
          experimental_alert: true   
          
```

请注意，源规则和边缘规则都已声明，并且警报属性设置为 `true` 因此，只要达到阈值，您就可以收到警报，这很可能表示发生了攻击。

>[!NOTE]
>
>此 _试验性_ 释放警报功能后，experimental_alert前的prefix_将被删除。 要加入率先采用者计划，请发送电子邮件至 **<aemcs-waf-adopter@adobe.com>**.

建议将操作类型最初设置为记录，以便您可以在几小时或几天内监控流量，确保合法流量不超过这些速率。 几天后，更改为阻止模式。

按照以下步骤将更改部署到AEMCS环境：

- 提交上述更改并将这些更改推送到Cloud Manager Git存储库。
- 使用Cloud Manager的配置管道将更改部署到AEMCS环境。 参考 [通过Cloud Manager部署规则](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager) 以了解更多详细信息。
- 验证 **速率限制流量过滤器规则** 可以模拟攻击，如中所述 [攻击模拟](#attack-simulation) 部分。 将请求数限制为高于规则中设置的速率限制值的值。

### 配置请求转换规则 {#configure-request-transform-rules}

除了速率限制流量过滤器规则之外，建议使用 [请求转换](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#request-transformations) 取消设置应用程序不需要的查询参数，以最大限度地减少通过缓存失效技术绕过缓存的方式。 例如，如果您只想允许 `search` 和 `campaignId` 查询参数时，可以声明以下规则：

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: 
    - dev
    - stage
    - prod  
data:  
  experimental_requestTransformations:
    rules:            
      - name: unset-all-query-params-except-those-needed
        when:
          reqProperty: tier
          in: ["publish"]
        actions:
          - type: unset
            queryParamMatch: ^(?!search$|campaignId$).*$
```

## 接收流量过滤器规则警报 {#receiving-alerts}

如上所述，如果流量过滤器规则包括 *experimental_alert： true*，则会在规则匹配时收到警报。

## 对警报执行操作 {#acting-on-alerts}

有时，警报是信息性的，让您能够了解攻击的频率。 值得使用上述仪表板分析您的CDN数据，以验证流量尖峰是否由攻击引起，而不仅仅是合法流量增加所致。 如果是后者，请考虑提高阈值。

## 攻击模拟{#attack-simulation}

本节介绍模拟DoS攻击的方法，这些方法可用于为本教程中使用的仪表板生成数据，并验证任何配置的规则是否成功阻止攻击。

>[!CAUTION]
>
> 请勿在生产环境中执行这些步骤。 以下步骤仅用于模拟。
> 
>如果您收到指示流量尖峰的警报，请转到 [分析流量模式](#analyzing-traffic-patterns) 部分。

要模拟攻击，可使用诸如 [Apache基准](https://httpd.apache.org/docs/2.4/programs/ab.html)， [Apache JMeter](https://jmeter.apache.org/)， [韦盖塔](https://github.com/tsenart/vegeta)、和其他内容可以使用。

### Edge请求

使用以下项 [韦盖塔](https://github.com/tsenart/vegeta) 命令您可以向网站发出许多请求：

```shell
$ echo "GET https://<YOUR-WEBSITE-DOMAIN>" | vegeta attack -rate=120 -duration=5s | vegeta report
```

上述命令在5秒内发出120个请求并输出报告。 假设网站访问速率不受限制，这可能会导致流量激增。

### 源请求

要绕过CDN缓存并向源(AEM Publish服务)发出请求，您可以向URL添加唯一的查询参数。 请参阅示例中的Apache JMeter脚本 [使用JMeter脚本模拟DoS攻击](https://experienceleague.adobe.com/en/docs/experience-manager-learn/foundation/security/modsecurity-crs-dos-attack-protection#simulate-dos-attack-using-jmeter-script)

