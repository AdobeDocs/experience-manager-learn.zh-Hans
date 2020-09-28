---
title: 使用AEM Experience Fragments
description: 体验片段允许内容作者在包括站点页面和第三方系统在内的渠道中重复使用内容。
sub-product: 站点，内容服务
feature: experience-fragments
topics: authoring, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
kt: 971
translation-type: tm+mt
source-git-commit: 3b5dd583a458393a41dbce1d8eeb0095a22db734
workflow-type: tm+mt
source-wordcount: '326'
ht-degree: 0%

---


# 使用体验片段{#using-aem-experience-fragments}

体验片段是Adobe Experience Manager(AEM)的一个功能，最初在AEM 6.3中引入。体验片段允许内容作者在包括站点页面和第三方系统在内的渠道中重复使用内容。

## 体验片段概述

>[!VIDEO](https://video.tv.adobe.com/v/17028/?quality=9&learn=on)

体验片段是一组内容，分组后可以组成一个体验，而体验本身应具有意义。

通过体验片段，营销人员可以：

* 在渠道(所有渠道和第三方接触点)之间重复使用体验
* 为特定用例创建各种体验
* 使用Live Copy时使各种变量保持同步
* Facebook和Pinterest的社交帖子体验即装即用

## 包含体验片段的构建块

构建块是AEM 6.4+中添加到体验片段的新增功能。 它允许内容作者创建一个由组件组成的构建块，这些组件可以重复使用，跨不同的变体和模板创建内容。

>[!VIDEO](https://video.tv.adobe.com/v/21289/?quality=9&learn=on)

>[!NOTE]
>
> 用于创建体验片段的可编辑模板应将构建块组件添加到其策略中。

* 通过创建构建块，内容作者可以轻松地在不同变体之间重复使用内容。
* 更改主控复制构建块应自动滚出对其引用的更改，而不取消继承或任何布局更改。
* 内容作者可以轻松重命名现有构建块或删除它。
* 从体验片段中删除构建块不会删除其引用。

## 体验片段全文搜索

AEM 6.5现在支持Experience Fragments的全文搜索功能。

>[!VIDEO](https://video.tv.adobe.com/v/27720/?quality=9&learn=on)

* **内容作者** （内部搜索）现在可以在体验片段中搜索文本部分，结果将包括包含文本的体验片段以及引用体验片段的页面。
* **站点用户** （外部搜索）现在可以使用搜索组件执行全文搜索，结果将包括引用包含搜索关键字的体验片段的站点页面。
