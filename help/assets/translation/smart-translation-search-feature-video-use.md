---
title: 在AEM Assets中使用智能翻译搜索
description: Smart Translation Search支持跨AEM内容（资产和页面）自动进行跨语言搜索和发现，支持50多种语言，并减少对手动内容翻译的需求。
version: 6.4, 6.5
feature: Search
topic: Content Management
role: User
level: Beginner
last-substantial-update: 2022-09-03T00:00:00Z
thumbnail: 21297.jpg
exl-id: 4f35e3f7-ae29-4f93-bba9-48c60b800238
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 0%

---

# 在AEM Assets中使用智能翻译搜索{#using-smart-translation-search-with-aem-assets}

Smart Translation Search支持跨AEM内容（资产和页面）自动进行跨语言搜索和发现，支持50多种语言，并减少对手动内容翻译的需求。

>[!VIDEO](https://video.tv.adobe.com/v/21297?quality=12&learn=on)

AEM Smart Translation Search允许用户在AEM中使用非英语术语执行内容搜索，以匹配AEM中具有等同英语术语的资源。

智能翻译搜索是对应用于英语资源的AEM智能标记的完美补充。

本视频假定 [AEM Smart Translation搜索](smart-translation-search-technical-video-setup.md) 已设置。

## 智能翻译搜索的工作原理 {#how-smart-translation-search-works}

![智能翻译搜索流程图](assets/smart-translation-search-flow.png)

1. AEM用户执行全文搜索，提供本地化的搜索词(例如 西班牙语中的“man”、“hombre”一词)。
2. 参与由Apache Oak机器翻译OSGi捆绑包提供的智能翻译搜索，并评估所提供的搜索词是否可以使用注册的语言包进行翻译。
3. 将收集步骤#2中的所有翻译词，并且查询将在内部扩充以包含这些翻译词作为搜索词。 如果针对查找相关匹配项的AEM搜索索引进行正常评估，则此搜索词集将会增加。
4. 将收集与原始搜索词(“hombre”)或翻译搜索词(“man”)匹配的搜索结果，并将用户作为搜索结果返回。

## 其他资源{#additional-resources}

* [使用AEM Assets设置智能翻译搜索](smart-translation-search-technical-video-setup.md)
* [Apache Joshua语言包](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
