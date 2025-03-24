---
title: 自定义域名选项
description: 了解如何管理和实施AEM as a Cloud Service托管网站的自定义域名。
version: Experience Manager as a Cloud Service
feature: Cloud Manager, Custom Domain Names
topic: Architecture, Migration
role: Admin, Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 130
last-substantial-update: 2024-08-09T00:00:00Z
jira: KT-15946
thumbnail: KT-15946.jpeg
exl-id: e11ff38c-e823-4631-a5b0-976c2d11353e
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '599'
ht-degree: 1%

---

# 自定义域名选项

了解如何管理和实施AEM as a Cloud Service托管网站的域名。

>[!VIDEO](https://video.tv.adobe.com/v/3432632?quality=12&learn=on)

## 开始之前

在开始实施自定义域名之前，请确保您了解以下概念：

### 什么是域名

域名是人类友好的名称网站名称，如adobe.com，它指向Internet上的特定位置（如170.2.14.16等IP地址）。

### AEM as a Cloud Service中的默认域名

默认情况下，AEM as a Cloud Service配置有默认域名，以`*.adobeaemcloud.com`结尾。 针对`*.adobeaemcloud.com`颁发的通配符SSL证书将自动应用于所有环境，此通配符证书由Adobe负责。

默认域名采用`https://<SERVICE-TYPE>-p<PROGRAM-ID>-e<ENVIRONMENT-ID>.adobeaemcloud.com`格式。

- `<SERVICE-TYPE>`可以是&#x200B;**作者**、**发布**&#x200B;或&#x200B;**预览**。
- `<PROGRAM-ID>`是项目的唯一标识符。 一个组织可以有多个项目。
- `<ENVIRONMENT-ID>`是环境的唯一标识符，每个项目都包含四个环境：**快速开发(RDE)**、**开发**、**阶段**&#x200B;和&#x200B;**prod**。 除没有预览环境的&#x200B;**RDE**&#x200B;外，每个环境都包含上述三种服务类型。

总之，在配置所有AEM as a Cloud Service环境后，您就拥有了与默认域名组合在一起的&#x200B;**11** （RDE没有预览环境）唯一URL。

### Adobe管理的CDN与客户管理的CDN

为了减少延迟并改进网站性能，AEM as a Cloud Service已与Adobe管理的内容交付网络(CDN)集成。 已为所有环境自动启用Adobe-managed CDN 。 有关详细信息，请参阅[AEM as a Cloud Service缓存](../caching/overview.md)。

但是，客户也可以使用他们自己的CDN，称为&#x200B;**客户管理的CDN**。 它不是必需的，但很少有客户出于公司政策或其他原因使用它。 在这种情况下，客户负责管理CDN配置和设置。

### 自定义域名

出于品牌、真实性和业务开发目的，自定义域名始终优先于默认域名。 但是，它们只能应用于&#x200B;**发布**&#x200B;和&#x200B;**预览**&#x200B;服务类型，而不能应用于&#x200B;**作者**。

添加自定义域名时，必须为给定的自定义域提供有效的SSL证书。 SSL证书必须是由受信任的证书颁发机构(CA)签名的有效证书。

通常，客户将自定义域名用于Prod环境(AEM as a Cloud Service网站)，有时也用于较低级别的环境，如&#x200B;**stage**&#x200B;或&#x200B;**dev**。

| AEM服务类型 | 支持的自定义域？ |
|---------------------|:-----------------------:|
| 创作 | ✘ |
| 预览 | ✔ |
| 发布 | ✔ |

## 实施域名

要使用Adobe管理的CDN或客户管理的CDN实施域名，以下流程图将指导您完成该过程：

![域名管理流程图](./assets/domain-name-management-flowchart.png){width="800" zoomable="yes"}

此外，下表还指导您从何处管理特定配置：

| 自定义的域名 | 将SSL证书添加到 | 将域名添加到 | 在配置DNS记录 | 是否需要HTTP标头验证CDN规则？ |
|---------------------|:-----------------------:|-----------------------:|-----------------------:|-----------------------:|
| Adobe托管的CDN | Adobe Cloud Manager | Adobe Cloud Manager | DNS托管服务 | ✘ |
| 客户管理的CDN | CDN供应商 | CDN供应商 | DNS托管服务 | ✔ |

### 分步教程

现在您已经了解了域名管理流程，可以通过执行以下教程来为AEM as a Cloud Service网站实施自定义域名：

**[使用Adobe管理的CDN的自定义域名](./custom-domain-name-with-adobe-managed-cdn.md)**：在本教程中，您将了解如何使用Adobe管理的CDN **向**AEM as a Cloud Service网站添加自定义域名。
**[具有客户管理的CDN的自定义域名](./custom-domain-names-with-customer-managed-cdn.md)**：在本教程中，您将了解如何使用客户管理的CDN **向** AEM as a Cloud Service网站添加自定义域名。
