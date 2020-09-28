---
title: 将翻译与AEM内容片段结合使用
description: AEM 6.3引入了翻译内容片段的功能。 与内容片段关联的混合媒体资产和资产集合也有资格进行提取和翻译。
sub-product: 站点，资产，内容服务
feature: content-fragments, multi-site-manager
topics: localization, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '252'
ht-degree: 6%

---


# 将翻译与AEM内容片段结合使用{#using-translation-with-aem-content-fragments}

AEM 6.3引入了翻译内容片段的功能。 与内容片段关联的混合媒体资产和资产集合也有资格进行提取和翻译。

>[!VIDEO](https://video.tv.adobe.com/v/18131/?quality=9&learn=on)

## 内容片段翻译用例 {#content-fragment-translation-use-cases}

内容片段是可识别的内容类型，AEM将提取该类型以将其发送到外部翻译服务。 现成支持多种用例：

1. 内容片段可以直接在“资产”控制台中进行选择，以便进行语言复制和翻译
2. 在为语言副本选择站点页面时，在站点页面上引用的内容片段将被复制到相应的语言文件夹，并提取以进行翻译
3. 嵌入在内容片段中的内联媒体资产有资格进行提取和翻译。
4. 与内容片段关联的资产集合有资格进行提取和翻译

## 转换配置选项 {#translation-config-options}

开箱即用的转换配置支持翻译内容片段的多个选项。 默认情况下，内联媒体资产和关联的资产集合不会进行翻译。 要更新转换配置，请导航到 [http://localhost:4502/etc/cloudservices/translation/default_translation.html](http://localhost:4502/etc/cloudservices/translation/default_translation.html)。

转换内容片段资产有四个选项：

1. **不翻译（默认）**
2. **仅内嵌媒体资产**
3. **仅关联的资产收藏集**
4. **内嵌媒体资产和关联的收藏集**

![翻译配置](assets/classic-ui-dialog.png)
