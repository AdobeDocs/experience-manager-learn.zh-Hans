---
title: 在AEM中传送内容片段
seo-title: Delivering Content Fragments in Adobe Experience Manager
description: 内容片段与布局无关，可直接在包含核心组件的AEM Sites中使用，也可以无头方式交付到下游渠道。
seo-description: Content Fragments, independent of layout, can be used directly in AEM Sites with Core Components or can be delivered in a headless manner to downstream channels.
feature: Content Fragments
topics: authoring, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.4, 6.5
uuid: 045473d2-5abe-4414-b91c-d369f3069ead
discoiquuid: 912e0c41-83cf-49f7-b515-09519b6718c1
topic: Content Management
role: User
level: Beginner
exl-id: 525cd30c-05bf-4f17-b61b-90609ce757ea
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 5%

---

# 交付内容片段 {#delivering-content-fragments}

Adobe Experience Manager(AEM)内容片段是基于文本的编辑内容，可能包含一些与之关联但被视为纯内容的结构化数据元素，而不包含设计或布局信息。 内容片段通常创建为与渠道无关的内容，旨在跨渠道使用和重复使用，进而将内容包装在特定于上下文的体验中。

内容片段与布局无关，可直接在包含核心组件的AEM Sites中使用，也可以无头方式交付到下游渠道。

此视频系列介绍了使用内容片段的交付选项。 有关定义和 [可在此处找到创作内容片段](content-fragments-feature-video-use.md).

1. 在网页上使用内容片段
2. 使用AEM Content Services将内容片段公开为JSON
3. 使用资产HTTP API

## 在网页中使用内容片段 {#using-content-fragments-in-web-pages}

>[!VIDEO](https://video.tv.adobe.com/v/22449/?quality=12&learn=on)

内容片段可以在AEM Sites页面上使用，或通过AEM WCM核心组件以类似方式使用体验片段。 [内容片段组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=zh-Hans).

可以使用AEM样式系统设置内容片段组件的样式，以根据需要显示内容。

## 将内容片段公开为JSON {#exposing-content-fragments-as-json}

>[!VIDEO](https://video.tv.adobe.com/v/22448/?quality=12&learn=on)

AEM Content Services可帮助创建基于AEM页面的HTTP端点，以将内容演绎版为标准化的JSON格式。

以上视频使用 [内容片段组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html) 以显示单个内容片段。 的 [内容片段列表组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-list.html) 是一个新组件，它允许作者定义一个查询，该查询将使用内容片段列表动态填充页面。 当需要公开多个内容片段时，首选使用内容片段列表组件。

*Content Services端点JSON有效负载示例：*\
**[aterys.json](assets/athletes.json)**

## 使用资产HTTP API

>[!VIDEO](https://video.tv.adobe.com/v/26390/?quality=12&learn=on)

AEM 6.5中首次引入，通过Assets HTTP API增强了对内容片段的支持。 这为开发人员提供了一种针对内容片段执行创建、读取、更新和删除(CRUD)操作的简单方法。

*POSTMAN请求示例：*
**[CRUD-CFM-API-We.Retail.postman_collection.json](assets/CRUD-CFM-API-We.Retail.postman_collection.json)**

## 使用哪种投放方法

### Web 渠道

通过Web渠道交付内容片段的方法通过将内容片段组件与AEM Sites结合使用非常简单。

### Headless

在无头用例中，有两个选项用于将内容片段公开为JSON以支持第三方渠道：

1. 当主要用例是交付内容片段以供第三方渠道使用（只读）时，请使用AEM内容服务和代理API页面(视频#2)。 内容服务框架在哪些数据会公开方面提供了更大的灵活性和选项。 开发人员还可以扩展内容服务框架以扩充和/或扩充数据。

2. 当第三方渠道需要修改和/或更新内容片段时，请使用资产HTTP API(视频#3)。 典型的用例是在AEM创作环境中摄取第三方内容。

## 其他资源 {#additional-resources}

* [创作内容片段](content-fragments-feature-video-use.md)
* [AEM WCM核心组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hans)
* [AEM WCM核心内容片段组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html)

要在AEM 6.4+实例上下载并安装以下包，以获取视频系列的最终状态，请执行以下操作：\
**[aem_demo_fluid-experiencecontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
