---
title: 使用流量过滤器规则（包括WAF规则）保护网站
description: 了解流量过滤器规则，包括其Web应用程序防火墙(WAF)规则的子类别。 如何创建、部署和测试规则。 此外，分析结果可保护您的AEM站点。
version: Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-20T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
source-git-commit: fa28ae232a5353eb34788fd2abe8402b42a62f66
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 0%

---


# 使用流量过滤器规则（包括WAF规则）保护网站

了解 **流量过滤器规则**，包括其子类别 **Web应用程序防火墙(WAF)规则** 在AEMas a Cloud Service(AEMCS)中。 阅读有关如何创建、部署和测试规则的信息。 此外，分析结果可保护您的AEM站点。

## 概述

减少安全违规风险是任何组织的首要任务。 AEMCS提供了流量过滤器规则功能（包括WAF规则），以保护网站和应用程序。

流量过滤器规则部署到 [内置CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html) 在请求到达AEM基础架构之前对和进行评估。 利用此功能，您可以显着增强网站的安全性，确保只允许合法请求访问AEM基础架构。

本教程将指导您完成创建、部署、测试和分析流量过滤器规则（包括WAF规则）结果的过程。

有关流量过滤器规则的更多信息，请参阅 [本文](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=en)

>[!IMPORTANT]
>
> 名为“WAF规则”的流量过滤器规则的子类别需要WAF-DDoS保护许可证


## 下一步

学习 [如何设置](./how-to-setup.md) 该功能允许您创建、部署和测试流量过滤器规则。 阅读有关设置的信息 **Elasticsearch、Logstash和Kibana (ELK)** 栈叠功能板工具以分析AEMCS CDN日志的结果。



