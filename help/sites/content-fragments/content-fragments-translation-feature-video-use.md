---
title: 对AEM内容片段的翻译支持
description: 了解如何使用Adobe Experience Manager本地化和翻译内容片段。 与内容片段关联的混合媒体资产也有资格进行提取和翻译。
feature: Content Fragments, Multi Site Manager
topic: Localization
role: User
level: Intermediate
version: 6.4, 6.5, Cloud Service
kt: 201
thumbnail: 18131.jpg
exl-id: cc4ffbd0-207a-42e4-bfcb-d6c83fb97237
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '250'
ht-degree: 2%

---

# 对AEM内容片段的翻译支持 {#translation-support-content-fragments}

了解如何使用Adobe Experience Manager本地化和翻译内容片段。 与内容片段关联的混合媒体资产也有资格进行提取和翻译。

>[!VIDEO](https://video.tv.adobe.com/v/18131/?quality=12&learn=on)

## 内容片段翻译用例 {#content-fragment-translation-use-cases}

内容片段是AEM提取的可识别内容类型，可发送到外部翻译服务。 开箱即用地支持以下几个用例：

1. 内容片段可以是 [直接在“资产”控制台中选择以进行语言复制和翻译](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/translate-assets.html).
2. 在为语言副本选择站点页面时，在站点页面上引用的内容片段将会复制到相应的语言文件夹中，并提取以供翻译。
3. 内容片段中嵌入的内联媒体资产有资格进行提取和翻译。
4. 与内容片段关联的资产集合有资格进行提取和翻译。

## 翻译规则编辑器 {#translation-rules-editor}

Experience Manager翻译行为可使用 **翻译规则编辑器**. 要更新翻译，请导航至 **工具** > **常规** > **翻译配置** at [http://localhost:4502/libs/cq/translation/translationrules/contexts.html](http://localhost:4502/libs/cq/translation/translationrules/contexts.html).

开箱即用的配置引用了 `fragmentPath` 资源类型为 `core/wcm/components/contentfragment/v1/contentfragment`. 继承自 `v1/contentfragment` 被默认配置识别。

![翻译规则编辑器](assets/translation-configuration.png)
