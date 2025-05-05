---
title: 图像、色板、旋转和混合媒体集
description: Dynamic Media Classic最有用且最强大的功能之一是，它支持创建富媒体集，如图像、色板、旋转和混合媒体集。 了解每个富媒体集是什么以及如何在Dynamic Media Classic中创建每种类型。 然后，了解有关批量集预设的更多信息，该功能可在上传时自动创建富媒体集。
feature: Dynamic Media Classic, Image Sets, Mixed Media Sets, Spin Sets
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: 45c86ff2-d991-46a7-a8d1-25c9fec142d9
duration: 280
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1359'
ht-degree: 1%

---

# 图像、色板、旋转和混合媒体集 {#media-sets}

Dynamic Media Classic集合超越了使用单个图像来动态调整大小和缩放的功能，可提供更丰富的在线体验。 本教程的此部分将探讨如何在Dynamic Media Classic中创建以下富媒体集：

- 图像集
- 样本集
- 旋转集
- 混合媒体集

它还将说明如何使用批量集预设通过上传来自动创建集。

## 你一直想知道的关于布景的一切

在基本动态大小和缩放之外，集可能是使用最广泛的Dynamic Media Classic子产品。 集实质上是“虚拟”资产，不包含实际图像，但包含一组与其他图像和/或视频的关系。 这些套装的主要吸引力在于，它们是“现成可用的”小型应用程式。 也就是说，每个集合的查看器都包含其自身的逻辑和界面，您只需在站点上调用它们即可。 此外，它们只需要您跟踪每个集的单个资产ID，而不必自己管理所有成员资产和关系。

在创建集时，该集作为单独的资产进行管理，必须先将其标记为发布并发布，然后才能从URL提供该集。 其所有成员资产也必须发布。

### 集类型

让我们了解一下您可以在Dynamic Media Classic中创建的四种类型的集：图像、色板、旋转和混合媒体集。

## 图像集

这是最常见的集合类型。 您通常将其用于同一项目的替代视图。 它由多个图像组成，您可以通过单击该图像的关联缩略图将其加载到查看器中。

![图像](assets/media-sets/image-set-1.jpg)

_图像集示例_

上述图像集的URL可能显示为：

![图像](assets/media-sets/image-set-url-1.png)

- 通过[图像集快速入门](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sets/quick-start-image-sets.html?lang=zh-Hans)了解有关图像集的更多信息。
- 了解如何[创建图像集](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sets/creating-image-set.html?lang=zh-Hans#creating-an-image-set)。

### 样本集

此类型的集通常用于显示同一项目的彩色视图。 它由成对的图像和色板组成。

样本和图像集的主要区别在于，样本集使用不同的图像作为可单击的样本，而图像集使用原始图像的缩放、可单击的缩略图版本。

样本集不会对图像进行着色（这是一种常见的误解）。 图像只是简单地交换而已，与图像集中完全相同。 这些迷你色板图像可能是使用Photoshop创作的，每种颜色都可以单独拍摄，或者Dynamic Media Classic中的裁切工具也可以用来从一幅彩色图像制作色板。

![图像](assets/media-sets/image-set-2.jpg)

_样本集示例_

上述样本集的URL可能显示为：

![图像](assets/media-sets/image-set_url.png)

- 通过[样本集快速入门](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/swatch-sets/quick-start-swatch-sets.html?lang=zh-Hans)了解有关样本集的更多信息。
- 了解如何[创建样本集](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/swatch-sets/creating-swatch-set.html?lang=zh-Hans#creating-a-swatch-set)。

### 旋转集

此集通常用于显示项目的360度视图。 与样本集一样，旋转集不使用3D魔术 — 真正的工作是从各个角度创建图像的许多照片。 查看器仅允许您像停止动画一样在图像之间切换。

旋转集可以沿单个轴在一个方向旋转，也可以交替创建为2D旋转集 — 在多个轴上旋转。 例如，汽车可以在所有车轮都位于地面时旋转，然后可以“翻转”并在其后轮上旋转。 对于正确设置的2D旋转集，每个轴的每行图像数应该相同。 换言之，如果在两个轴上旋转，则需要两倍于一个角度旋转的图像。

![图像](assets/media-sets/image-set-3.png)

_旋转集示例_

上述旋转集的URL可能显示为：

![图像](assets/media-sets/spin-set.png)

- 通过[旋转集快速入门](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/spin-sets/quick-start-spin-sets.html?lang=zh-Hans)了解有关旋转集的更多信息。
- 了解如何[创建旋转集](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/spin-sets/creating-spin-set.html?lang=zh-Hans#creating-a-spin-set)。

## 混合媒体集

这是一个组合集。 它允许您组合以前的任何集合，并将视频添加到单个查看器中。 在此工作流中，首先创建任何组件集，然后将它们组合到混合媒体集中。

![图像](assets/media-sets/image-set-4.png)

_混合媒体集示例_

上述混合媒体集的URL可能显示为：

![图像](assets/media-sets/image-set-url-1.png)

- 通过[混合媒体集快速入门](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/mixed-media-sets/quick-start-mixed-media-sets.html?lang=zh-Hans)了解有关混合媒体集的更多信息。

- 了解如何[创建混合媒体集](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/mixed-media-sets/creating-mixed-media-set.html?lang=zh-Hans#creating-a-mixed-media-set)。

要在网站上显示缩放、设置或视频的图像，请在Dynamic Media Classic“查看器”中将其称为。 Dynamic Media Classic包括富媒体资产（如样本集、旋转集、视频和许多其他资产）的查看器。

详细了解AEM Assets和Dynamic Media Classic的[查看器](https://experienceleague.adobe.com/docs/dynamic-media-developer-resources/library/viewers-aem-assets-dmc/c-html5-s7-aem-asset-viewers.html?lang=zh-Hans)。

## 批次集预设

在此之前，我们一直在讨论如何使用Dynamic Media Classic的“生成”功能手动生成集。 但是，只要具有标准化的命名约定，就可以使用批次集预设自动创建图像集和旋转集。

每个预设都是一组唯一命名的、自包含的指令，用于定义如何使用与定义的命名约定匹配的图像来构建该集合。 在预设中，首先为要分组到一组中的资产定义命名约定。 然后，可以创建批次集预设以引用这些图像。

虽然可以自行创建预设（可在&#x200B;**设置>应用程序设置>批次集预设**&#x200B;下找到），但最佳做法是让咨询团队或技术支持人员为您设置预设。 原因如下：

- 批量集预设的设置可能比较复杂 — 它们由正则表达式提供支持，除非您是开发人员，否则此语法可能不熟悉或令人困惑。
- 创建后，它们会默认打开。 没有“撤消”功能。 如果您开始上传数千个图像并且预设配置不正确，则最终可能会产生成百上千个损坏的集，您必须手动查找并删除这些集。

之前建议使用一个简单的命名约定，该约定很容易构建到批次集预设中。 但是，由于预设非常灵活，因此它们可以处理复杂的命名策略。 简而言之，属于某个集的图像应该通过某种通用名称捆绑在一起 — 通常是SKU号或产品ID。 在Dynamic Media Classic中，您可以为所有要用于预设的图像指定默认命名约定，也可以创建多个预设，每个预设具有不同的命名规则。

批量集预设仅在上传时应用；上传图像后无法运行批量集预设。 因此，在开始加载所有图像之前，规划命名惯例并构建预设很重要。

创建预设后，公司管理员可以选择它们是处于活动状态还是不活动状态。 “活动”表示它们将出现在上载页面的&#x200B;**作业选项**&#x200B;下，而非活动预设将保持隐藏状态。

了解如何[创建批次集预设](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html?lang=zh-Hans#creating-a-batch-set-preset)。

### 在上传时使用批次集预设

以下是创建批量集预设后如何在上传时使用它们：

1. 单击&#x200B;**上传**，然后选择&#x200B;**从桌面**&#x200B;或&#x200B;**通过FTP**。
2. 单击&#x200B;**作业选项**。
3. 打开&#x200B;**批次集预设**&#x200B;选项，然后选中或取消选中预设以将其用于上传。
4. 上传完成后，在文件夹中查找完成的集。

了解有关[批次集预设](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html?lang=zh-Hans#batch-set-presets)的更多信息。
