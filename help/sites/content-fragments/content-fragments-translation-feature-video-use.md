---
title: AEM内容片段的翻译支持
description: 了解如何使用Adobe Experience Manager本地化和翻译内容片段。 与内容片段关联的混合媒体资产也有资格进行提取和翻译。
feature: 内容片段，多站点管理器
topic: 本地化
role: 业务从业者
level: 中间
version: 6.3, 6.4, 6.5, cloud-service
kt: 201
thumbnail: 18131.jpg
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 2%

---


# AEM内容片段{#translation-support-content-fragments}的翻译支持

了解如何使用Adobe Experience Manager本地化和翻译内容片段。 与内容片段关联的混合媒体资产也有资格进行提取和翻译。

>[!VIDEO](https://video.tv.adobe.com/v/18131/?quality=12&learn=on)

## 内容片段翻译用例{#content-fragment-translation-use-cases}

内容片段是AEM提取的要发送到外部翻译服务的可识别内容类型。 支持多种开箱即用的用例：

1. 内容片段可以直接在“资产”控制台中进行选择，以便进行语言复制和翻译
2. 在站点页面上引用的内容片段将复制到相应的语言文件夹，并在为语言副本选择站点页面时提取以进行翻译
3. 嵌入在内容片段中的内联媒体资产有资格进行提取和翻译。
4. 与内容片段关联的资产集合有资格进行提取和翻译

## 翻译规则编辑器 {#translation-rules-editor}

Experience Manager转换行为可使用&#x200B;**转换规则编辑器**&#x200B;进行更新。 要更新翻译，请导航至&#x200B;**Tools** > **General** > **Translation Configuration**，地址为[http://localhost:4502/libs/cq/translation/translationrules/contexts.html](http://localhost:4502/libs/cq/translation/translationrules/contexts.html)。

开箱即用配置引用`fragmentPath`中资源类型为`core/wcm/components/contentfragment/v1/contentfragment`的内容片段。 默认配置会识别从`v1/contentfragment`继承的所有组件。

![翻译规则编辑器](assets/translation-configuration.png)
