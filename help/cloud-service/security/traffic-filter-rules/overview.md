---
title: 使用流量过滤规则（包括 WAF 规则）保护网站
description: 了解流量过滤器规则，包括其Web应用程序防火墙(WAF)规则的子类别。 如何创建、部署和测试规则。 此外，分析结果可保护您的AEM站点。
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
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 13%

---

# 使用流量过滤规则（包括 WAF 规则）保护网站

了解&#x200B;**流量过滤器规则**，包括其在AEM as a Cloud Service (AEMCS)中的&#x200B;**Web应用程序防火墙(WAF)规则**&#x200B;的子类别。 阅读有关如何创建、部署和测试规则的信息。 此外，分析结果可保护您的AEM站点。

>[!VIDEO](https://video.tv.adobe.com/v/3425401?quality=12&learn=on)

## 概述

减少安全违规风险是任何组织的首要任务。 AEMCS提供了流量过滤器规则功能(包括WAF规则)，以保护网站和应用程序。

流量过滤器规则将部署到[内置CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=zh-Hans)，并在请求到达AEM基础架构之前进行评估。 利用此功能，您可以显着增强网站的安全性，确保只允许合法请求访问AEM基础架构。

本教程将指导您完成创建、部署、测试和分析流量过滤器规则(包括WAF规则)结果的过程。

您可以在[本文](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=zh-Hans)中阅读有关流量过滤器规则的更多信息。

>[!IMPORTANT]
>
> 名为“WAF规则”的流量过滤器规则子类别需要WAF-DOS保护或增强安全性许可证。

我们邀请您通过发送电子邮件至 **aemcs-waf-adopter@adobe.com** 提供反馈或询问有关流量过滤规则的问题。

## 下一步

了解[如何设置](./how-to-setup.md)该功能，以便您可以创建、部署和测试流量过滤器规则。 了解如何设置&#x200B;**Elasticsearch、Logstash和Kibana (ELK)**&#x200B;栈栈仪表板工具来分析AEMCS CDN日志的结果。


