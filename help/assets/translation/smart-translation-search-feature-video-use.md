---
title: 在AEM Assets中使用智能翻译搜索
description: 智能翻译搜索支持跨AEM内容（包括资产和页面）自动进行跨语言搜索和发现，支持超过50种语言，并减少了手动内容翻译的需求。
version: 6.3, 6.4, 6.5
feature: 搜索
topic: 内容管理
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 1%

---


# 在AEM Assets中使用智能翻译搜索{#using-smart-translation-search-with-aem-assets}

智能翻译搜索支持跨AEM内容（包括资产和页面）自动进行跨语言搜索和发现，支持超过50种语言，并减少了手动内容翻译的需求。

>[!VIDEO](https://video.tv.adobe.com/v/21297/?quality=9&learn=on)

AEM智能翻译搜索允许用户使用非英语术语在AEM中搜索内容，以匹配AEM中具有相同英文术语的资产。

智能翻译搜索是AEM智能标记的完美补充，智能标记以英语应用于资产。

此视频假定已设置[AEM智能翻译搜索](smart-translation-search-technical-video-setup.md)。

## 智能翻译搜索的工作原理{#how-smart-translation-search-works}

![智能翻译搜索流程图](assets/smart-translation-search-flow.png)

1. AEM用户执行全文搜索，提供本地化的搜索词(例如 西班牙语中“man”、“hombre”的词)。
2. Apache Oak机器翻译OSGi包提供的智能翻译搜索将参与，并评估提供的搜索词是否可以使用注册的语言包进行翻译。
3. 将收集第#2步中的所有翻译术语，并在内部扩展查询，以将它们作为搜索术语包含在内。 如果根据查找相关匹配的AEM搜索索引正常评估，则此扩展搜索词集。
4. 将收集与原始术语(“hombre”)或翻译术语(“man”)匹配的搜索结果，并将用户作为搜索结果返回。

## 其他资源{#additional-resources}

* [使用AEM Assets设置智能翻译搜索](smart-translation-search-technical-video-setup.md)
* [Apache Joshua语言包](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)