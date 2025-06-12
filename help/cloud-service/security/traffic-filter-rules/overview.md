---
title: 使用流量过滤规则（包括 WAF 规则）保护网站
description: 了解流量过滤规则，包括其子类别 —— Web Application Firewall (WAF) 规则。如何创建、部署和测试这些规则。另外，分析结果，以保护您的 AEM 网站。
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: e6d67204-2f76-441c-a178-a34798fe266d
duration: 165
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '270'
ht-degree: 100%

---

# 使用流量过滤规则（包括 WAF 规则）保护网站

了解 AEM as a Cloud Service (AEMCS) 中的&#x200B;**流量过滤规则**，包括其子类别——**Web Application Firewall (WAF) 规则**。阅读如何创建、部署和测试这些规则。另外，分析结果，以保护您的 AEM 网站。

>[!VIDEO](https://video.tv.adobe.com/v/3425401?quality=12&learn=on)

## 概述

降低安全漏洞风险是每个组织的重中之重。AEMCS 提供了流量过滤规则功能，其中包括 WAF 规则，以保护网站和应用程序的安全。

流量过滤规则会部署到[内置的内容传递网络](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=zh-Hans)，并在请求到达 AEM 基础设施之前进行评估。借助此功能，您可以大幅提升网站的安全性，确保只有合法请求能够访问 AEM 基础设施。

本教程将指导您完成流量过滤规则（包括 WAF 规则）的创建、部署、测试及结果分析全过程。

您可以在[本文](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=zh-Hans)中阅读更多关于流量过滤规则的内容。

>[!IMPORTANT]
>
> 流量过滤规则的一个子类别称为“WAF 规则”，使用该规则需要具备 WAF-DDoS 防护或增强安全许可。

我们邀请您通过发送电子邮件至 **aemcs-waf-adopter@adobe.com** 提供反馈或询问有关流量过滤规则的问题。

## 下一步

了解[如何设置](./how-to-setup.md)该功能，以便创建、部署和测试流量过滤规则。阅读关于配置 **Elasticsearch、Logstash 和 Kibana (ELK)** 技术栈仪表板工具的内容，用于分析 AEMCS CDN 日志的结果。


