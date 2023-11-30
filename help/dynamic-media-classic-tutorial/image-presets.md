---
title: 图像预设
description: Dynamic Media Classic中的图像预设包含以特定大小、格式、质量和锐化创建图像所需的所有设置。 图像预设是动态调整大小的关键组件。 当您在Dynamic Media Classic中查看URL时，可以轻松查看是否正在使用图像预设。 了解图像预设、它们为什么如此有用以及如何创建一个。
feature: Dynamic Media Classic, Image Presets
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: e472db7c-ac3f-4f66-85af-5a4c68ba609e
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '670'
ht-degree: 1%

---

# 图像预设 {#image-presets}

图像预设本质上是一种方法，包含以特定大小、格式、质量和锐化创建图像所需的所有设置。 图像预设是动态调整大小的关键组件。

如果您查看几乎任何Dynamic Media Classic客户的URL，您可能会看到某个图像预设正在使用中。 只需在URL末尾查找$name$（使用任何单词或单词替换名称）。

图像预设可缩短URL，因此您可以编写单个图像预设，而不是每次请求都写出多个图像服务指令。 例如，这两个URL使用锐化生成相同的300 x 300JPEG图像，但第二个使用图像预设：

![图像](assets/image-presets/image-preset-2.png)

图像预设的真正价值在于，任何公司管理员都可以更新该图像预设的定义，并使用该格式影响每个图像，而无需更改任何Web代码。 在URL缓存清除后，您将看到对图像预设所做任何更改的结果。

>[!IMPORTANT]
>
>在调整图像大小时，宽高比（图像的宽度与高度的比率）应始终保持正比，以便图像不会失真。

图像预设的名称两侧有一个美元符号($)，并且跟在问号(？)后面。 分隔符.

>[!TIP]
>
>在网站上为每个唯一图像大小创建一个图像预设。 例如，如果产品详细信息页面需要350 X 350图像，浏览/搜索页面需要120 X 120图像，交叉销售/精选项目需要90 X 90图像，那么无论您有500个图像还是500,000个图像，都需要三个图像预设。

- 了解有关 [图像预设](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sizing/setting-image-presets.html).
- 了解如何 [创建图像预设](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sizing/setting-image-presets.html#creating-an-image-preset).

## 图像预设和锐化

图像预设通常会调整图像大小，无论何时根据原始大小调整图像大小，都应添加锐化。 这是因为调整大小会导致许多像素合并并混合到一个较小的空间中，从而使图像看起来既柔和又模糊。 锐化会增加图像中边缘和高对比度区域的对比度。

我们期望您上传到Dynamic Media Classic的高分辨率图像在完整尺寸查看时不需要任何锐化 — 当缩放到时。 但是，在任何较小的尺寸下，通常都需要进行某些锐化。

>[!TIP]
>
>调整图像大小时始终锐化！ 这意味着您需要为每个图像预设（和查看器预设，我们将在后面讨论）添加锐化。
>
>如果你的图像看起来不好，很可能是因为它们需要锐化，或者质量本来就很差。

锐化程度的增加完全是主观的。 有些人喜欢更柔和的图像，而另一些人则喜欢非常锐利的图像。 通过在图像上运行锐化滤镜的组合可以轻松增强图像。 但是，也很容易超出范围并过度锐化图像。

下图显示了三个锐化级别。 从右到左，没有锐化，只有适当的锐化，而且锐化程度过高。

![图像](assets/image-presets/image-presets-1.jpg)

Dynamic Media Classic允许三种类型的锐化：简单锐化、重新取样模式和钝化蒙版。

了解有关 [Dynamic Media Classic锐化选项](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/master-files/sharpening-image.html#sharpening_an_image).

## 其他资源

[图像预设指南](https://www.adobe.com/content/dam/www/us/en/experience-manager/pdfs/dynamic-media-image-preset-guide.pdf). 用于优化图像质量和加载速度的设置。

[图像是所有内容第2部分：从不只有模糊 — 质量与速度](https://theblog.adobe.com/image-is-everything-part-2-its-never-just-a-blur-quality-versus-speed/). 讨论使用图像预设交付高质量、快速加载图像的博客文章。
