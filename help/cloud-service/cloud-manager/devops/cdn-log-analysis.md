---
title: CDN日志分析工具
description: 了解Adobe提供的AEM Cloud Service CDN日志分析工具，以及它如何帮助您深入了解您的CDN性能和AEM实施。
version: Experience Manager as a Cloud Service
feature: Developer Tools
topic: Development
role: Developer, Architect, Admin
level: Beginner
doc-type: Tutorial
duration: 219
last-substantial-update: 2024-05-17T00:00:00Z
jira: KT-15505
thumbnail: KT-15505.jpeg
exl-id: 830c2486-099b-454f-bc07-6bf36e81ac8d
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 0%

---

# CDN日志分析工具

了解Adobe提供的&#x200B;_AEM Cloud Service CDN日志分析工具_，以及它如何帮助您深入了解您的CDN性能和AEM实施。
 
>[!VIDEO](https://video.tv.adobe.com/v/3429177?quality=12&learn=on)

## 概述

[AEM as a Cloud Service CDN日志分析工具](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling)提供可与[Splunk](https://www.splunk.com/en_us/products/observability-cloud.html)或[ELK栈栈](https://www.elastic.co/elastic-stack)集成的预建仪表板，用于实时监视和分析CDN日志。

通过使用该工具，您可以实现实时监控和主动问题检测。 因此，可以确保优化内容投放以及针对拒绝服务(DoS)和分布式拒绝服务(DDoS)攻击的适当安全措施。

## 主要功能

- 简化的日志分析
- 实时监控
- 无缝集成
- 仪表板
   - 确定潜在的安全威胁
   - 更快的最终用户体验

## 功能板概述

为了快速启动日志分析，Adobe为Splunk和ELK栈栈提供了预建的仪表板。

- **CDN缓存命中率**：按HIT、PASS和MISS状态提供对缓存命中率和请求总数的见解。 它还提供顶级HIT、PASS和MISS URL。

  ![CDN缓存命中率](assets/CHR-dashboard.png)

- **CDN流量仪表板**：通过CDN和原始请求率、4xx和5xx错误率以及非缓存请求提供流量分析。 它还为每个客户端IP地址提供每秒最大CND和源请求数，以及优化CDN配置的更多见解。

  ![CDN流量仪表板](assets/Traffic-dashboard.png)

- **WAF功能板**：通过已分析、已标记和已阻止的请求提供见解。 它还提供了WAF Flag ID排名最前的攻击、客户端IP、国家/地区和用户代理排名前100的攻击以及优化WAF配置的更多见解。

  ![WAF信息板](assets/WAF-Dashboard.png)

## Splunk集成

对于利用[Splunk](https://www.splunk.com/en_us/products/observability-cloud.html)且已启用AEMCS日志转发到其Splunk实例的组织，可以快速导入预建仪表板。 这种设置有助于加速日志分析，提供可操作的见解以优化AEM实施并减轻DOS攻击等安全威胁。

您可以开始使用[适用于AEMCS CDN日志分析的Splunk功能板](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/Splunk/README.md#splunk-dashboards-for-aemcs-cdn-log-analysis)指南。


## ELK集成

包含Elasticsearch、Logstash和Kibana的[ELK栈栈](https://www.elastic.co/elastic-stack)是另一个用于日志分析的强大选项。 对于无权访问Splunk设置或日志转发功能的组织，此插件非常有用。 在本地设置ELK栈栈非常简单，工具提供了Docker组合文件以快速入门。 然后，您可以导入预建功能板并摄取使用Adobe Cloud Manager下载的CDN日志。

可开始使用AEMCS CDN日志分析的[ELK Docker容器](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/ELK/README.md#elk-docker-container-for-aemcs-cdn-log-analysis)指南。
