---
title: 在AEM中交付内容片段
seo-title: Delivering Content Fragments in Adobe Experience Manager
description: 内容片段与布局无关，可直接在包含核心组件的AEM Sites中使用，也可以以Headless方式交付到下游渠道。
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
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 7%

---

# 交付内容片段 {#delivering-content-fragments}

Adobe Experience Manager (AEM)内容片段是基于文本的编辑内容，其中可能包含一些关联但被视为纯内容（没有设计或布局信息）的结构化数据元素。 内容片段通常作为与渠道无关的内容创建，这些内容旨在跨渠道使用和重复使用，这反过来又会将内容包装在特定上下文的体验中。

内容片段与布局无关，可直接在包含核心组件的AEM Sites中使用，也可以以Headless方式交付到下游渠道。

本视频系列介绍了使用内容片段的交付选项。 有关定义和的详细信息 [可以在此处找到创作内容片段](content-fragments-feature-video-use.md).

1. 在网页上使用内容片段
2. 使用AEM Content Services将内容片段公开为JSON
3. 使用资产HTTP API

## 在网页中使用内容片段 {#using-content-fragments-in-web-pages}

>[!VIDEO](https://video.tv.adobe.com/v/22449?quality=12&learn=on)

内容片段可以在AEM Sites页面上使用，也可以通过类似方式，在AEM WCM核心组件中使用体验片段 [内容片段组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=zh-Hans).

可以使用AEM样式系统对内容片段组件进行样式设置，以根据需要显示内容。

## 将内容片段公开为JSON {#exposing-content-fragments-as-json}

>[!VIDEO](https://video.tv.adobe.com/v/22448?quality=12&learn=on)

AEM Content Services有助于创建基于AEM页面的HTTP端点，以将内容呈现为规范化的JSON格式。

上述视频使用 [内容片段组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=zh-Hans) 以显示单个内容片段。 此 [内容片段列表组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-list.html) 是一个新组件，它允许作者定义一个查询，该查询将使用内容片段列表动态填充页面。 当需要公开多个内容片段时，首选内容片段列表组件。

*示例Content Services端点JSON负载：*\
**[items.json](assets/athletes.json)**

## 使用资产HTTP API

>[!VIDEO](https://video.tv.adobe.com/v/26390?quality=12&learn=on)

最初在AEM 6.5中引入了，通过Assets HTTP API增强了对于内容片段的支持。 这为开发人员提供了一种针对内容片段执行创建、读取、更新和删除(CRUD)操作的简单方法。

*Postman请求示例：*
**[CRUD-CFM-API-We.Retail.postman_collection.json](assets/CRUD-CFM-API-We.Retail.postman_collection.json)**

## 使用哪种投放方法

### Web 渠道

通过将内容片段组件与AEM Sites结合使用，通过Web渠道交付内容片段的方法非常简单。

### Headless

在Headless用例中，有两种方法可以将内容片段公开为JSON以支持第三方渠道：

1. 当主要用例是交付内容片段供第三方渠道使用（只读）时，请使用AEM Content Services和代理API页面(视频#2)。 Content Services框架在公开哪些数据方面提供了更多灵活性和选项。 开发人员还可以扩展内容服务框架以扩充和/或扩充数据。

2. 当第三方渠道需要修改和/或更新内容片段时，使用Assets HTTP API(视频#3)。 典型用例是在AEM创作环境中摄取第三方内容。

## 其他资源 {#additional-resources}

* [创作内容片段](content-fragments-feature-video-use.md)
* [AEM WCM 核心组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hans)
* [AEM WCM核心内容片段组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=zh-Hans)

要从视频系列中下载包并将其安装在最终状态的AEM 6.4+实例上：\
**[aem_demo_fluid-experiencescontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
