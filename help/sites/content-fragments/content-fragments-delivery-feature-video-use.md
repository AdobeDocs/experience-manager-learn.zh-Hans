---
title: 在AEM中传送内容片段
seo-title: 在Adobe Experience Manager中传送内容片段
description: 内容片段独立于布局，可直接在包含核心组件的AEM Sites中使用，或以无外设方式交付到下游渠道。
seo-description: 内容片段独立于布局，可直接在包含核心组件的AEM Sites中使用，或以无外设方式交付到下游渠道。
sub-product: 内容服务
feature: 内容片段
topics: authoring, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
uuid: 045473d2-5abe-4414-b91c-d369f3069ead
discoiquuid: 912e0c41-83cf-49f7-b515-09519b6718c1
topic: 内容管理
role: 业务从业者
level: 初学者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '599'
ht-degree: 7%

---


# 传送内容片段{#delivering-content-fragments}

Adobe Experience Manager(AEM)内容片段是基于文本的编辑内容，可能包括一些与设计或布局信息相关联但被视为纯粹内容的结构化数据元素。 内容片段通常创建为与渠道无关的内容，旨在在渠道之间使用和重复使用，而这些内容又会将内容打包为特定于上下文的体验。

内容片段独立于布局，可直接在包含核心组件的AEM Sites中使用，或以无外设方式交付到下游渠道。

此视频系列介绍了使用内容片段的投放选项。 有关定义和[创作内容片段的详细信息，可在此处](content-fragments-feature-video-use.md)找到。

1. 在网页上使用内容片段
2. 使用AEM Content Services将内容片段公开为JSON
3. 使用资产HTTP API

## 在网页{#using-content-fragments-in-web-pages}中使用内容片段

>[!VIDEO](https://video.tv.adobe.com/v/22449/?quality=12&learn=on)

内容片段可在AEM Sites页面上使用，或使用AEM WCM核心组件[内容片段组件](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/components/content-fragment-component.html)以类似方式使用体验片段。

可以使用AEM样式系统设置内容片段组件的样式，以根据需要显示内容。

## 将内容片段公开为JSON {#exposing-content-fragments-as-json}

>[!VIDEO](https://video.tv.adobe.com/v/22448/?quality=12&learn=on)

AEM Content Services可方便创建基于AEM Page的HTTP端点，将内容再现为标准JSON格式。

上述视频使用[内容片段组件](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html)来公开单个内容片段。 [内容片段列表组件](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-list.html)是一个新组件，它允许作者定义一个查询，该列表将使用内容片段动态填充页面。 当需要公开多个内容片段时，首选使用内容片段列表组件。

*Content Services端点JSON负载示例：*\
**[atelers.json](assets/athletes.json)**

## 使用资产HTTP API

>[!VIDEO](https://video.tv.adobe.com/v/26390/?quality=12&learn=on)

AEM 6.5中首先引入了资产HTTP API对内容片段的增强支持。 这为开发人员针对内容片段执行创建、读取、更新和删除(CRUD)操作提供了一种简单的方法。

*示例POSTMAN请*
**[求：CRUD-CFM-API-We.Retail.postman_collection.json](assets/CRUD-CFM-API-We.Retail.postman_collection.json)**

## 使用哪种投放方法

### Web 渠道

通过Web渠道传送内容片段的方法很简单，只需将内容片段组件与AEM Sites结合使用即可。

### 无头

在无头用例中，有两种选项可将内容片段公开为JSON以支持第三方渠道:

1. 当主要用例是交付内容片段供第三方渠道使用（只读）时，请使用AEM Content Services和Proxy API页(视频#2)。 内容服务框架提供了更多关于哪些数据会泄露的灵活性和选项。 开发者还可以扩展内容服务框架以扩大和/或丰富数据。

2. 当第三方渠道需要修改和/或更新内容片段时，请使用资产HTTP API(视频#3)。 典型用例是在AEM作者环境上收录第三方内容。

## 其他资源 {#additional-resources}

* [创作内容片段](content-fragments-feature-video-use.md)
* [AEM WCM核心组件](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/introduction.html)
* [AEM WCM核心内容片段组件](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html)

要从视频系列下载并安装AEM 6.4+实例中用于最终状态的以下包：\
**[aem_demo_fluid-experiencescontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
