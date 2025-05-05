---
title: AEM内容片段的翻译支持
description: 了解如何使用Adobe Experience Manager本地化并翻译内容片段。 还有资格提取和翻译与内容片段关联的混合媒体资产。
feature: Content Fragments, Multi Site Manager
topic: Localization
role: User
level: Intermediate
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
jira: KT-201
thumbnail: 18131.jpg
doc-type: Feature Video
exl-id: cc4ffbd0-207a-42e4-bfcb-d6c83fb97237
duration: 223
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 2%

---

# AEM内容片段的翻译支持 {#translation-support-content-fragments}

了解如何使用Adobe Experience Manager本地化并翻译内容片段。 还有资格提取和翻译与内容片段关联的混合媒体资产。

>[!VIDEO](https://video.tv.adobe.com/v/3410322?quality=12&learn=on&captions=chi_hans)

## 内容片段翻译用例 {#content-fragment-translation-use-cases}

内容片段是AEM提取并发送到外部翻译服务的可识别内容类型。 支持开箱即用的多个用例：

1. 可以直接在Assets控制台中为语言副本和翻译[&#128279;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/translate-assets.html?lang=zh-Hans)选择内容片段。
2. 为语言复制选择站点页面时，在站点页面上引用的内容片段将复制到相应的语言文件夹并提取以供翻译。
3. 嵌入在内容片段中的内联媒体资产有资格进行提取和翻译。
4. 与内容片段关联的资产收藏集符合提取和翻译的条件。

## 翻译规则编辑器 {#translation-rules-editor}

可以使用&#x200B;**翻译规则编辑器**&#x200B;更新Experience Manager翻译行为。 要更新翻译，请在[http://localhost:4502/libs/cq/translation/translationrules/contexts.html](http://localhost:4502/libs/cq/translation/translationrules/contexts.html)上导航到&#x200B;**工具** > **常规** > **翻译配置**。

开箱即用的配置引用资源类型为`core/wcm/components/contentfragment/v1/contentfragment`的`fragmentPath`处的内容片段。 默认配置可识别从`v1/contentfragment`继承的所有组件。

![翻译规则编辑器](assets/translation-configuration.png)
