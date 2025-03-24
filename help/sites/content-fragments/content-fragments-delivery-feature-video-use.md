---
title: 在AEM中交付内容片段
description: 内容片段与布局无关，可直接在包含核心组件的AEM Sites中使用，也可以以Headless方式交付到下游渠道。
feature: Content Fragments
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 525cd30c-05bf-4f17-b61b-90609ce757ea
duration: 878
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '520'
ht-degree: 2%

---

# 交付内容片段 {#delivering-content-fragments}

Adobe Experience Manager (AEM)内容片段是基于文本的编辑内容，其中可能包含一些关联但被视为纯内容的结构化数据元素，而无设计或布局信息。 内容片段通常作为与渠道无关的内容创建，这些内容旨在跨渠道使用和重复使用，这反过来又会将内容包装在特定上下文的体验中。

内容片段与布局无关，可直接在包含核心组件的AEM Sites中使用，也可以以Headless方式交付到下游渠道。

本视频系列介绍了使用内容片段的交付选项。 有关定义和[创作内容片段的详细信息见此处](content-fragments-feature-video-use.md)。

1. 在网页上使用内容片段
2. 使用AEM Content Services将内容片段公开为JSON
3. 使用Assets HTTP API

## 在网页中使用内容片段 {#using-content-fragments-in-web-pages}

>[!VIDEO](https://video.tv.adobe.com/v/22449?quality=12&learn=on)

内容片段可以在AEM Sites页面上使用，或者以类似的方式在体验片段中使用AEM WCM核心组件的[内容片段组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=zh-Hans)。

可以使用AEM的样式系统对内容片段组件进行样式设置，以根据需要显示内容。

## 以JSON形式公开内容片段 {#exposing-content-fragments-as-json}

>[!VIDEO](https://video.tv.adobe.com/v/22448?quality=12&learn=on)

AEM Content Services有助于创建基于AEM页面的HTTP端点，以将内容呈现为规范化JSON格式。

上述视频使用[内容片段组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=zh-Hans)公开单个内容片段。 [内容片段列表组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-list.html)是一个新组件，它允许作者定义一个查询，该查询将动态地使用内容片段的列表填充页面。 当需要公开多个内容片段时，首选内容片段列表组件。

*示例Content Services端点JSON有效负载：*\
**[运动员.json](assets/athletes.json)**

## 使用Assets HTTP API

>[!VIDEO](https://video.tv.adobe.com/v/26390?quality=12&learn=on)

AEM 6.5中首次引入了Assets HTTP API，用于增强对内容片段的支持。 这为开发人员提供了一种针对内容片段执行创建、读取、更新和删除(CRUD)操作的简单方法。

*示例POSTMAN请求：*
**[CRUD-CFM-API-We.Retail.postman_collection.json](assets/CRUD-CFM-API-We.Retail.postman_collection.json)**

## 要使用的投放方法

### Web 渠道

通过将内容片段组件与AEM Sites结合使用，通过Web渠道交付内容片段的方法非常直接。

### Headless

在Headless用例中，有两个选项可将内容片段公开为JSON以支持第三方渠道：

1. 当主要用例是交付内容片段以供第三方渠道使用（只读）时，使用AEM内容服务和代理API页面(视频#2)。 Content Services框架在公开哪些数据方面提供了更多灵活性和选项。 开发人员还可以扩展内容服务框架以扩充和/或丰富数据。

2. 当第三方渠道需要修改和/或更新内容片段时，使用Assets HTTP API(视频#3)。 典型用例是在AEM创作环境中摄取第三方内容。

## 其他资源 {#additional-resources}

* [创作内容片段](content-fragments-feature-video-use.md)
* [AEM WCM 核心组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-hans)
* [AEM WCM核心内容片段组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=zh-Hans)

要从视频系列中下载并将以下包安装到最终状态的AEM 6.4+实例上：\
**[aem_demo_fluid-experiencescontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
