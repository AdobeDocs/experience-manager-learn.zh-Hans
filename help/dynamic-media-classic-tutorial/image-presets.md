---
title: 图像预设
description: Dynamic Media Classic中的图像预设包含创建特定大小、格式、质量和锐化的图像所需的所有设置。 图像预设是动态调整大小的关键组件。 当您查看Dynamic Media Classic中的URL时，您可以轻松查看是否使用了图像预设。 了解图像预设、它们为何如此有用以及如何创建它们。
sub-product: dynamic-media
feature: Dynamic Media经典，图像预设
doc-type: tutorial
topics: development, authoring, configuring
audience: all
activity: use
topic: 内容管理
role: 业务从业者
level: 初学者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '715'
ht-degree: 1%

---


# 图像预设 {#image-presets}

图像预设本质上是一种菜谱，其中包含以特定大小、格式、质量和锐化创建图像所需的所有设置。 图像预设是动态调整大小的关键组件。

如果您查看任何Dynamic Media Classic客户的URL，您可能会看到正在使用的图像预设。 只需在URL的末尾查找$name$（名称中有任何单词或单词替换）。

图像预设可缩短URL，因此您无需每次请求写出多个图像服务说明，而是可以编写一个图像预设。 例如，这两个URL生成的JPEG图像与锐化的图像大小相同，但第二个URL使用图像预设：

![图像](assets/image-presets/image-preset-2.png)

图像预设的真正价值在于，任何公司管理员可以更新该图像预设的定义，并使用该格式影响每个图像，而无需更改任何Web代码。 清除URL的缓存后，您将看到对图像预设进行任何更改的结果。

>[!IMPORTANT]
>
>在调整图像大小时，长宽比、图像宽度与其高度的比值应始终保持成比例，以使图像不扭曲。

图像预设的名称两侧都有一个美元符号($)，并在问号(?)后跟 分隔符.

>[!TIP]
>
>根据网站上的唯一图像大小创建一个图像预设。 例如，如果您需要350 X 350图像作为产品详细信息页面，120 X 120图像作为浏览/搜索页面，而90 X 90图像作为交叉销售/特色项目，则您需要三个图像预设，无论您有500张图像还是500,000张图像。

- 了解有关[图像预设](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/image-sizing/setting-image-presets.html)的更多信息。
- 了解如何[创建图像预设](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/image-sizing/setting-image-presets.html#creating-an-image-preset)。

## 图像预设和锐化

图像预设通常会调整图像大小，每当您调整图像的原始大小时，都应该添加锐化功能。 这是因为调整大小会导致许多像素合并并混合到较小的空间中，使图像看起来柔和而模糊。 锐化可提高图像中边缘和高对比度区域的对比度。

我们预计，在放大后全尺寸查看时，上传到Dynamic Media Classic的高分辨率图像不需要任何锐化。 但是，在任何较小的大小，通常都需要进行锐化。

>[!TIP]
>
>在调整图像大小时始终锐化！ 这意味着您需要为每个图像预设（和查看器预设）添加锐化，我们稍后将讨论这些预设)。
>
>如果图像看起来不好，可能是因为图像需要锐化，或者一开始质量很差。

增加多少锐化完全是主观的。 有些人喜欢柔和的图像，而另一些人则非常喜欢它们。 通过对图像运行一系列锐化过滤器，可以轻松地增强图像。 但是，也可以轻松过头和过度锐化图像。

下图显示了三个锐化级别。 从右到左，您无需锐化、只需适当的数量，且数量过多。

![图像](assets/image-presets/image-presets-1.jpg)

Dynamic Media Classic允许三种类型的锐化：简单锐化、重新取样模式和USM锐化。

了解有关[Dynamic Media Classic锐化选项](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/master-files/sharpening-image.html#sharpening_an_image)的更多信息。

## 其他资源

[图像预设指南](https://www.adobe.com/content/dam/www/us/en/experience-manager/pdfs/dynamic-media-image-preset-guide.pdf)。用于优化图像质量和加载速度的设置。

[图像是一切第2部分：这绝不只是模糊 — 品质与速度之比](https://theblog.adobe.com/image-is-everything-part-2-its-never-just-a-blur-quality-versus-speed/)。一篇博客文章，讨论如何使用图像预设来传送高质量、快速加载的图像。

[图像是所有网络研讨会](https://dynamicmediaseries2019.enterprise.adobeevents.com/)。链接到&#x200B;_图像是所有_&#x200B;系列中所有三个网络研讨会的录制内容。 [网络研讨会2](https://seminars.adobeconnect.com/p6lqaotpjnd3) 讨论图像预设。
