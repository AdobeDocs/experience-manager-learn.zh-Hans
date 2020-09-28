---
title: 将智能翻译搜索与AEM Assets结合使用
seo-title: 将智能翻译搜索与AEM Assets结合使用
description: 智能翻译搜索支持跨AEM内容（资产和页面）的跨语言搜索和发现，支持50多种语言，并减少了手动内容翻译的需求。
seo-description: 智能翻译搜索支持跨AEM内容（资产和页面）的跨语言搜索和发现，支持50多种语言，并减少了手动内容翻译的需求。
uuid: daa6f20f-a4d3-402d-83b9-57d852062a89
discoiquuid: eb2e484a-0068-458f-acff-42dd95a40aab
topics: authoring, search, metadata, localization
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---


# 将智能翻译搜索与AEM Assets结合使用{#using-smart-translation-search-with-aem-assets}

智能翻译搜索支持跨AEM内容（资产和页面）的跨语言搜索和发现，支持50多种语言，并减少了手动内容翻译的需求。

>[!VIDEO](https://video.tv.adobe.com/v/21297/?quality=9&learn=on)

AEM智能翻译搜索允许用户使用非英语术语在AEM中对内容进行搜索，以匹配AEM中具有等效英语术语的资产。

智能翻译搜索是AEM智能标记的完美补充，这些标记以英语应用于资产。

此视频假 [设AEM智能翻译](smart-translation-search-technical-video-setup.md) 搜索已设置。

## 智能翻译搜索的工作原理 {#how-smart-translation-search-works}

![智能翻译搜索流程图](assets/smart-translation-search-flow.png)

1. AEM用户执行全文搜索，提供本地化的搜索词(例如， 西班牙语中“man”、“hombre”)。
2. 由Apache Oak Machine Translation OSGi捆绑提供的智能翻译搜索服务将参与，并评估所提供的搜索词是否可以使用注册语言包进行翻译。
3. 将收集第2步中的所有译文术语，并在内部对查询进行增强，以将其作为搜索术语包含在内。 如果通常根据AEM搜索索引评估查找相关匹配项，则此增强的搜索词集。
4. 将收集与原始词语(“hombre”)或译文词语(“man”)匹配的搜索结果，并将用户作为搜索结果返回。

## 其他资源{#additional-resources}

* [使用AEM Assets设置智能翻译搜索](smart-translation-search-technical-video-setup.md)
* [Apache Joshua Language Pack](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)