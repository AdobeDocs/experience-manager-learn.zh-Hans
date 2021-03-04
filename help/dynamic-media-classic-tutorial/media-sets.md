---
title: 图像、色板、旋转和混合媒体集
description: Dynamic Media Classic最有用、最强大的功能之一是支持创建富媒体集，如图像、色板、旋转和混合媒体集。 了解每个富媒体集的内容以及如何在Dynamic Media Classic中创建每种类型。 然后进一步了解批集预设，它可以在上传时自动创建富媒体集。
sub-product: dynamic-media
feature: Dynamic Media经典、图像集、混合媒体集、旋转集
doc-type: tutorial
topics: sets, development, authoring, configuring
audience: all
activity: use
topic: 内容管理
role: 业务从业者
level: 初学者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1471'
ht-degree: 1%

---


# 图像、色板、旋转和混合媒体集{#media-sets}

Dynamic Media Classic集合不仅可用于动态调整大小和缩放单个图像，还可提供更丰富的在线体验。 本教程的本节将介绍如何在Dynamic Media Classic中创建以下富媒体集：

- 图像集
- 色板集
- 旋转集
- 混合媒体集

此外，还将说明如何使用批量集预设通过上传自动创建集。

## 你一直想了解的关于场景的一切

除了基本的动态大小调整和缩放之外，集合可能是使用最广泛的Dynamic Media Classic子产品。 这些集本质上是“虚拟”资产，不包含任何实际图像，但由与其他图像和/或视频的一组关系组成。 集合的主要吸引力在于，它们是已“现成”的微型应用程序。 也就是说，每个集查看器都包含其自己的逻辑和界面，这样您只需在网站上调用它们即可。 此外，他们只要求您跟踪每个集的单个资产ID，而不必自己管理所有成员资产和关系。

当您创建集时，该集将作为单独的资产进行管理，必须先将其标记为发布并发布，然后才能通过URL提供该集。 其所有成员资产也必须发布。

### 集的类型

让我们来了解您可以在Dynamic Media Classic中创建的四种类型的集：图像、色板、旋转和混合媒体集。

## 图像集

这是最常见的集类型。 您通常会将它用于同一项目的替代视图。 它包含多个图像，通过单击该图像的关联缩略图加载到查看器中。

![图像](assets/media-sets/image-set-1.jpg)

_图像集示例_

上述图像集的URL可能显示为：

![图像](assets/media-sets/image-set-url-1.png)

- 使用[图像集快速开始](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/image-sets/quick-start-image-sets.html)进一步了解图像集。
- 了解如何[创建图像集](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/image-sets/creating-image-set.html#creating-an-image-set)。

### 色板集

此类集通常用于显示同一项目的彩色视图。 它由多对图像和色板组成。

样本集和图像集的主要区别在于，样本集使用不同的图像作为可点击的样本，而图像集使用原始图像的缩览图缩览图缩览图。

色板集不对图像着色（一个常见的误解）。 只需交换图像，就像在图像集中一样。 可以使用Photoshop创作迷你色板图像，可以单独拍摄每种颜色，也可以使用Dynamic Media Classic中的裁剪工具从其中一种彩色图像制作色板。

![图像](assets/media-sets/image-set-2.jpg)

_样本集示例_

上述样本集的URL可能显示为：

![图像](assets/media-sets/image-set_url.png)

- 了解有关使用[快速开始到样本集](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/swatch-sets/quick-start-swatch-sets.html)的样本集的更多信息。
- 了解如何[创建色板集](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/swatch-sets/creating-swatch-set.html#creating-a-swatch-set)。

### 旋转集

此集通常用于显示项目的360度视图。 与样本集一样，旋转集不使用3D功能 — 真正的工作是从各个方面创建图像的许多照片。 查看器只允许您在图像之间切换，如停格动画。

旋转集可以沿单个轴沿一个方向旋转，或者如果交替创建为2D旋转集，则可以在多个轴上旋转。 例如，汽车可以在所有车轮都在地面时旋转，然后也可以“翻转”，并在其后轮上旋转。 要正确设置2D旋转集，每个轴的每行图像数应相同。 换句话说，如果您在两个轴上旋转，则需要的图像是单个角度旋转的两倍。

![图像](assets/media-sets/image-set-3.png)

_旋转集示例_

上述旋转集的URL可能显示为：

![图像](assets/media-sets/spin-set.png)

- 通过[旋转集快速开始](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/spin-sets/quick-start-spin-sets.html)进一步了解旋转集。
- 了解如何[创建旋转集](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/spin-sets/creating-spin-set.html#creating-a-spin-set)。

## 混合媒体集

这是组合集。 它允许您将以前的任何一组合并到单个查看器中，并向单个查看器中添加视频。 在此工作流中，您首先创建任何组件集，然后将它们组合到一个混合媒体集中。

![图像](assets/media-sets/image-set-4.png)

_混合媒体集的示例_

上述混合媒体集的URL可能显示为：

![图像](assets/media-sets/image-set-url-1.png)

- 了解有关混合媒体集的详细信息，请使用[混合媒体集的快速开始](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/mixed-media-sets/quick-start-mixed-media-sets.html)。

- 了解如何[创建混合媒体集](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/mixed-media-sets/creating-mixed-media-set.html#creating-a-mixed-media-set)。

要在您的网站上显示缩放、集合或视频的图像，您可在Dynamic Media Classic的“查看器”中将其称为“查看器”。 Dynamic Media Classic包含色板集、旋转集、视频等富媒体资产的查看器。

了解有关[AEM Assets和Dynamic Media Classic](https://docs.adobe.com/content/help/en/dynamic-media-developer-resources/library/viewers-aem-assets-dmc/c-html5-s7-aem-asset-viewers.html)的查看器的更多信息。

## 批次集预设

到目前为止，我们一直在讨论如何使用Dynamic Media经典构建功能手动构建集。 但是，只要您有标准化的命名约定，就可以使用批集预设自动创建图像集和旋转集。

每个预设都是一组唯一命名的自包含指令，用于定义如何使用与定义的命名约定匹配的图像构建该预设。 在预设中，您首先为要组合到一组中的资产定义命名约定。 然后，可以创建批集预设来引用这些图像。

虽然您可以自行创建预设（位于&#x200B;**设置>应用程序设置>批量集预设**&#x200B;下），但作为最佳实践，您应让咨询团队或技术支持为您设置预设。 原因如下：

- 批集预设设置可能很复杂 — 它们由常规表达式提供支持，除非您是开发人员，否则此语法可能不熟悉或令人混淆。
- 创建后，默认情况下会打开它们。 没有“撤消”功能。 如果您开始上传了数千个图像并且您的预设配置不正确，则最终可能会有数百个或数千个损坏的集，您必须手动查找并删除这些集。

之前曾建议过一种简单的命名约定，可轻松构建到批集预设中。 但是，由于预设非常灵活，它们可以处理复杂的命名策略。 简而言之，属于某个集合的图像应该通过一些通用名称来绑定在一起，通常是SKU编号或产品ID。 在Dynamic Media经典中，您可以告诉它要用于预设的所有图像的默认命名规范，也可以创建多个预设，每个预设的命名规则各不相同。

批集预设仅在上传时应用；在上传图像后无法运行它们。 因此，在开始加载所有图像之前，务必规划命名惯例并构建预设。

创建预设后，公司管理员可以选择这些预设是处于活动状态还是非活动状态。 活动表示这些预设将显示在上传页面的&#x200B;**作业选项**&#x200B;下，而不活动的预设将保持隐藏。

了解如何[创建批集预设](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/setup/application-setup.html#creating-a-batch-set-preset)。

### 在上传时使用批集预设

以下是创建批集预设后，在上传时如何使用这些预设：

1. 单击&#x200B;**上载**，然后选择&#x200B;**从桌面**&#x200B;或&#x200B;**通过FTP**。
2. 单击&#x200B;**作业选项**。
3. 打开&#x200B;**批集预设**&#x200B;选项，选中或取消选中预设以将其用于上传。
4. 上载完成后，在您的文件夹中查找已完成的集。

了解有关[批集预设](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/setup/application-setup.html#batch-set-presets)的更多信息。
