---
title: 在AEM中交付内容片段
seo-title: 在Adobe Experience Manager交付内容片段
description: 内容片段独立于布局，可直接在AEM Sites与核心组件一起使用，或以无头方式交付到下游渠道。
seo-description: 内容片段独立于布局，可直接在AEM Sites与核心组件一起使用，或以无头方式交付到下游渠道。
sub-product: 内容服务
feature: content-fragments
topics: authoring, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
uuid: 045473d2-5abe-4414-b91c-d369f3069ead
discoiquuid: 912e0c41-83cf-49f7-b515-09519b6718c1
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '592'
ht-degree: 6%

---


# 交付内容片段 {#delivering-content-fragments}

Adobe Experience Manager(AEM)内容片段是基于文本的编辑内容，可能包含一些与之相关的结构化数据元素，但是这些元素被视为纯内容，而无需设计或布局信息。 内容片段通常创建为与渠道无关的内容，用于在渠道之间使用和重复使用，而这些内容又会将内容打包为特定于上下文的体验。

内容片段独立于布局，可直接在AEM Sites与核心组件一起使用，或以无头方式交付到下游渠道。

此视频系列介绍使用内容片段的投放选项。 有关定义和创 [作内容片段的详细信息，请参阅此处](content-fragments-feature-video-use.md)。

1. 在网页上使用内容片段
2. 使用AEM Content Services将内容片段公开为JSON
3. 使用资产HTTP API

## 在网页中使用内容片段 {#using-content-fragments-in-web-pages}

>[!VIDEO](https://video.tv.adobe.com/v/22449/?quality=12&learn=on)

内容片段可以在AEM Sites网页上使用，也可以使用AEM WCM核心组件的内容片段组件，以类似的方 [式使用体验片段](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/components/content-fragment-component.html)。

内容片段组件可以使用AEM样式系统设置样式以根据需要显示内容。

## 将内容片段公开为JSON {#exposing-content-fragments-as-json}

>[!VIDEO](https://video.tv.adobe.com/v/22448/?quality=12&learn=on)

AEM Content Services便于创建AEM基于页面的HTTP端点，将内容再现为标准化JSON格式。

上述视频使用内容 [片段组件](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/components/content-fragment-component.html) ，以公开单个内容片段。 内 [容片段列表组件](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-list.html) 是一个新组件，它允许作者定义一个查询，该列表将动态填充页面内容片段。 当需要公开多个内容片段时，首选使用内容片段列表组件。

*Content Services端点JSON有效负荷示例：*\
**[运动员。json](assets/athletes.json)**

## 使用资产HTTP API

>[!VIDEO](https://video.tv.adobe.com/v/26390/?quality=12&learn=on)

首先在AEM 6.5中引入的是通过资产HTTP API增强对内容片段的支持。 这为开发人员提供了一种针对内容片段执行创建、读取、更新和删除(CRUD)操作的简单方法。

*POSTMAN请求示例：***[CRUD-CFM-API-We.Retail.postman_collection.json](assets/CRUD-CFM-API-We.Retail.postman_collection.json)**

## 使用哪种投放方法

### Web 渠道

通过Web渠道传送内容片段的方法很简单，方法是将内容片段组件与AEM Sites结合使用。

### 无头

在无头用例中，有两种方法可将内容片段公开为JSON以支持第三方渠道:

1. 当主要用例为第三方渠道交付内容片段以供使用（只读）时，请使用AEM Content Services和Proxy API页面（视频#2）。 内容服务框架提供了更多关于哪些数据会泄露的灵活性和选项。 开发人员还可以扩展内容服务框架以扩大和／或丰富数据。

2. 当第三方渠道需要修改和／或更新内容片段时，请使用资产HTTP API（视频#3）。 典型用例是在AEM作者环境上引入第三方内容。

## 其他资源 {#additional-resources}

* [创作内容片段](content-fragments-feature-video-use.md)
* [AEM WCM核心组件](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/introduction.html)
* [AEM WCM核心内容片段组件](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/components/content-fragment-component.html)

要从视频系列下载并安装AEM 6.4+实例的下面包，以获得最终状态：\
**[aem_demo_fluid-experiencescontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
