---
title: 图像、色板、旋转和混合媒体集
description: Dynamic Media Classic最有用、最强大的功能之一是，它支持创建富媒体集，如图像、色板、旋转和混合媒体集。 了解每个富媒体集的含义以及如何在Dynamic Media Classic中创建每种类型。 然后，了解有关批量集预设的更多信息，批量集预设可在上传时自动创建富媒体集。
sub-product: dynamic-media
feature: Dynamic Media Classic、图像集、混合媒体集、旋转集
topic: 内容管理
role: User
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '1446'
ht-degree: 1%

---


# 图像、色板、旋转和混合媒体集 {#media-sets}

Dynamic Media Classic集合不仅允许对单个图像进行动态大小调整和缩放，还允许提供更丰富的在线体验。 本教程的本部分将探讨如何在Dynamic Media Classic中创建以下富媒体集：

- 图像集
- 色板集
- 旋转集
- 混合媒体集

本文还将介绍如何使用批量集预设通过上传自动创建集。

## 你一直想了解的关于场景的一切

除了基本的动态大小调整和缩放之外，集合可能是使用最广泛的Dynamic Media Classic子产品。 这些集实质上是“虚拟”资产，不包含实际图像，但由与其他图像和/或视频的关系组成。 设备的主要吸引力在于，它们是已“现成”的微型应用程序。 也就是说，每个集查看器都包含其自己的逻辑和界面，因此您只需在网站上调用它们即可。 此外，它们只要求您跟踪每个集的单个资产ID，而无需自行管理所有成员资产和关系。

在创建集合时，该集会作为单独的资产进行管理，必须先将其标记为发布，然后才能通过URL提供该集合。 其所有成员资产也必须发布。

### 集类型

让我们了解您可以在Dynamic Media Classic中创建的四种类型的集：图像、色板、旋转和混合媒体集。

## 图像集

这是最常见的集类型。 您通常会将其用于同一项目的替代视图。 它由多幅图像组成，您可以通过单击该图像的关联缩略图加载到查看器中。

![图像](assets/media-sets/image-set-1.jpg)

_图像集的示例_

上述图像集的URL可能显示为：

![图像](assets/media-sets/image-set-url-1.png)

- 了解有关使用[图像集快速入门](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sets/quick-start-image-sets.html)的图像集的更多信息。
- 了解如何[创建图像集](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sets/creating-image-set.html#creating-an-image-set)。

### 色板集

此类集通常用于显示同一项目的彩色视图。 它由成对的图像和颜色色板组成。

样本集和图像集的主要区别在于，样本集使用不同的图像作为可单击的样本，而图像集使用原始图像的缩略图版本。

色板集不会对图像着色（一种常见的误解）。 只是在交换图像，与在图像集中完全相同。 迷你色板图像可以使用Photoshop创作，每种颜色可以单独拍摄，或者Dynamic Media Classic中的裁剪工具可以用其中一种彩色图像制作色板。

![图像](assets/media-sets/image-set-2.jpg)

_样本集示例_

上述样本集的URL可能显示为：

![图像](assets/media-sets/image-set_url.png)

- 了解有关[快速入门到样本集](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/swatch-sets/quick-start-swatch-sets.html)的样本集的更多信息。
- 了解如何[创建色板集](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/swatch-sets/creating-swatch-set.html#creating-a-swatch-set)。

### 旋转集

此集通常用于显示项目的360度视图。 与样本集一样，旋转集也不使用3D魔法 — 真正的工作是从各个方面创建一幅图像的许多照片。 查看器仅允许您在图像之间进行切换，如停止动画。

旋转集可以沿单个轴沿一个方向旋转，也可以作为2D旋转集交替创建 — 在多个轴上旋转。 例如，汽车可以在所有车轮都在地面上时进行旋转，然后可以“翻转”，并在后轮上进行旋转。 对于正确设置的2D旋转集，每个轴的每行图像数应相同。 换言之，如果您在两个轴上旋转，则需要的图像数量是单角度旋转的两倍。

![图像](assets/media-sets/image-set-3.png)

_旋转集示例_

上述旋转集的URL可能显示为：

![图像](assets/media-sets/spin-set.png)

- 通过[快速启动旋转集](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/spin-sets/quick-start-spin-sets.html)了解有关旋转集的更多信息。
- 了解如何[创建旋转集](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/spin-sets/creating-spin-set.html#creating-a-spin-set)。

## 混合媒体集

这是一个组合集。 利用该功能，可将之前的任意集合并到单个查看器中，并添加视频。 在此工作流中，您首先创建任何组件集，然后将它们组合到一个混合媒体集中。

![图像](assets/media-sets/image-set-4.png)

_混合媒体集的示例_

上述混合媒体集的URL可能显示为：

![图像](assets/media-sets/image-set-url-1.png)

- 通过[快速入门到混合媒体集](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/mixed-media-sets/quick-start-mixed-media-sets.html)了解混合媒体集的更多信息。

- 了解如何[创建混合媒体集](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/mixed-media-sets/creating-mixed-media-set.html#creating-a-mixed-media-set)。

要在您的网站上显示图像以进行缩放、设置或视频，可在Dynamic Media Classic“查看器”中将其命名为。 Dynamic Media Classic包含富媒体资产（如色板集、旋转集、视频和许多其他内容）的查看器。

了解有关[AEM Assets和Dynamic Media Classic查看器的更多信息](https://experienceleague.adobe.com/docs/dynamic-media-developer-resources/library/viewers-aem-assets-dmc/c-html5-s7-aem-asset-viewers.html)。

## 批次集预设

迄今为止，我们一直在讨论如何使用Dynamic Media Classic Build函数手动构建集。 但是，只要您有标准的命名约定，就可以使用批集预设自动创建图像集和旋转集。

每个预设都是一组唯一命名的自包含说明，这些说明定义了如何使用符合所定义命名约定的图像来构建预设集。 在预设中，您首先需要为要分组到一组中的资产定义命名约定。 然后，可以创建批集预设以引用这些图像。

虽然您可以自行创建预设（这些预设位于&#x200B;**设置>应用程序设置>批量集预设**&#x200B;下），但最佳做法是让咨询团队或技术支持团队为您设置预设。 原因如下：

- 批集预设的设置可能非常复杂 — 它们由正则表达式提供支持，除非您是开发人员，否则此语法可能不熟悉或令人困惑。
- 创建后，默认情况下会开启这些功能。 没有“撤消”函数。 如果您开始上传数千个图像，并且您的预设配置不正确，则最终可能会有数百个或数千个损坏的集，您必须手动查找和删除这些集。

之前建议使用一种简单的命名约定，这样很容易构建到批集预设中。 但是，由于预设非常灵活，因此它们可以处理复杂的命名策略。 简而言之，集合中的图像应该通过一些通用名称绑定在一起，通常是SKU号或产品ID。 在Dynamic Media Classic中，您可以为要用于预设的所有图像指定默认的命名约定，也可以创建多个预设，每个预设具有不同的命名规则。

批集预设仅在上传时应用；在上传图像后，无法运行这些命令。 因此，在开始加载所有图像之前，务必要规划命名约定并构建预设。

创建预设后，公司管理员可以选择这些预设是处于活动状态还是非活动状态。 活动表示这些预设将显示在上传页面的&#x200B;**作业选项**&#x200B;下，而不活动的预设将保持隐藏状态。

了解如何[创建批集预设](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html#creating-a-batch-set-preset)。

### 在上传时使用批集预设

以下是创建批集预设后，在上传时如何使用这些预设：

1. 单击&#x200B;**Upload**，然后选择&#x200B;**From Desktop**&#x200B;或&#x200B;**Via FTP**。
2. 单击&#x200B;**作业选项**。
3. 打开&#x200B;**批量集预设**&#x200B;选项，然后选中或取消选中预设以将其用于上传。
4. 上传完成后，在您的文件夹中查找已完成的集。

了解有关[批集预设](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html#batch-set-presets)的更多信息。
