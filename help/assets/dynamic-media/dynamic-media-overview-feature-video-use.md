---
title: 动态媒体与AEM Assets概述
seo-title: 动态媒体与AEM Assets概述
description: 此视频系列概述了如何使用Adobe Experience Manager动态媒体作为内容服务来管理和访问媒体内容。 Dynamic Media可让您管理和发布动态数字体验——这是Experience Manager资产特有的功能。 我们的框架和组件套件使营销人员能够在所有设备上自定义和提供交互式多媒体体验。
seo-description: 此视频系列概述了如何使用Adobe Experience Manager动态媒体作为内容服务来管理和访问媒体内容。 Dynamic Media可让您管理和发布动态数字体验——这是Experience Manager资产特有的功能。 我们的框架和组件套件使营销人员能够在所有设备上自定义和提供交互式多媒体体验。
sub-product: 动态媒体
feature: smart-crop, video-profiles, image-profiles, viewer-presets, vr-360, sets
topics: videos, renditions, authoring, best-practices
doc-type: feature video
audience: all
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '940'
ht-degree: 0%

---


# 将Dynamic Media与AEM Assets {#understanding-aem-dynamic-media}

此多部分视频系列概述了如何使用Adobe Experience Manager动态媒体作为内容服务来管理和访问媒体内容。 Dynamic Media可让您管理和发布动态数字体验——这是Experience Manager资产特有的功能。 我们的框架和组件套件使营销人员能够在所有设备上自定义和提供交互式多媒体体验。

## Dynamic Media概述

>[!VIDEO](https://video.tv.adobe.com/v/27144/?quality=9&learn=on)

>[!NOTE]
>
>此处演示的功能可用于Dynamic Media DMS7运行模式，我们当前支持的运行模式，不一定是DMHybrid运行模式，DMS7已替换它。

此视频介绍如何使用Adobe Experience ManagerDynamic Media作为内容服务来管理和访问媒体内容。 Dynamic Media采用单一主控资产方法操作，在该方法中，您可以上传图像资产或视频资产，然后请求该资产来满足无限组所需的消耗品变体或衍生演绎版。 包括：

* 单个主控资产到URL产品交付内容说明
* 图像处理选项
* 内容查看器选项
* 将URL复制到图像和响应式查看器
* 智能裁切URL变体

## Dynamic Media与AEM Sites及任何其他CMS

>[!VIDEO](https://video.tv.adobe.com/v/27145/?quality=9&learn=on)

>[!NOTE]
>
>此处演示的功能可用于Dynamic Media DMS7运行模式，我们当前支持的运行模式，不一定是DMHybrid运行模式，DMS7已替换它。 此视频引用第1部分视频（Dynamic Media概述）中描述的概念。

此视频介绍如何在Adobe Experience Manager动态媒体中管理媒体内容，以及如何在AEM Sites借助组件轻松使用这些内容，从而根据响应式页面宽度进行简单、自动裁剪以优化。 轻松创建交互式图像横幅并生成要在任何内容管理系统中使用的复制URL。

* AEM Sites动态媒体组件灵活性
* 使用图像预设本地下载
* 创建和发布交互式横幅

## 用于构建混合媒体集合和自定义查看器的Dynamic Media

>[!VIDEO](https://video.tv.adobe.com/v/27146/?quality=9&learn=on)

>[!NOTE]
>
>此处演示的功能可用于Dynamic Media DMS7运行模式，我们当前支持的运行模式，不一定是DMHybrid运行模式，DMS7已替换它。 此视频引用第1部分视频（Dynamic Media概述）中描述的概念。

此视频描述了媒体资产（包括旋转集、视频和产品图像集合）的混合媒体查看器集合的简单创建过程。 将内容添加到混合媒体集，并创建自定义查看器以在最终的复制URL或AEM Sites组件中进行选择。

* 从相应的产品照片创建旋转集
* 为Dynamic Media视频上传主视频并对其进行编码
* 从旋转集、视频和照片创建混合媒体集
* 编辑和使用自定义混合媒体查看器

## Dynamic Media图像预设

>[!VIDEO](https://video.tv.adobe.com/v/27320/?quality=9&learn=on)

此视频介绍图像预设的创建方式以及图像预设是什么。图像预设是一种URL缩短工具，用于缩短每当URL请求图像时对图像执行操作的一系列图像服务器参数。 了解扩展和编辑图像预设的有价值技术。

* 图像预设缩短工具隐藏显式图像服务器命令集合
* 仅使用一个像素尺寸——宽度或高度——来调整新调整大小的图像，而无需填充
* 始终使用锐化
* URL修饰符字段，可添加额外命令来调整图像预设的大小

## Dynamic Media高级URL修饰符

>[!VIDEO](https://video.tv.adobe.com/v/27319/?quality=9&learn=on)

此视频介绍如何通过调整图像大小并利用源文件本身的功能——背景透明度、内置在剪切路径中以及将文本作为变量——以及Dynamic Media的URL修饰符。

* 在Dynamic Media修改量字段中使用URL修改量
* 更改具有透明度的图像上的背景颜色
* 剪切到图像路径
* 裁剪到图像路径
* 从Photoshop文件创建文本模板

## Dynamic Media控制JPEG文件大小（以千字节为单位）

>[!VIDEO](https://video.tv.adobe.com/v/27404/?quality=9&learn=on)


>[!NOTE]
>
>图像质量以反向压缩的百分比进行测量，其中100%的质量最少被压缩，从而生成高质量图像，但文件大小相对较大。 Jpeg压缩是一种有损压缩方案，压缩设置决定图像质量和文件大小。

使用2个命令调整jpeg压缩设置，使jpeg图像质量与生成的文件大小（以千字节为单位）保持平衡，以提高页面加载速度。 QLT通过调整jpeg压缩质量设置来定义图像质量。 “JPEG大小”命令允许您指定需要使用压缩实现的文件大小。

## 将CC隐藏式字幕添加到Dynamic Media视频

>[!VIDEO](https://video.tv.adobe.com/v/28074/?quality=9&learn=on)

通过附加复制URL以指向附加的隐藏字幕文件文档（web.VTT附加文件，包含任何视频的CC信息），轻松将隐藏字幕添加到Dynamic Media视频。

## 将图像锐化与AEM Dynamic Media结合使用

此视频介绍锐化图像对保持图像保真度至关重要的原因，以及如何使用高级设置制作完美图像。

>[!VIDEO](https://demos-pub.assetsadobe.com/etc/dam/viewers/s7viewers/html5/VideoViewer.html?asset=%2Fcontent%2Fdam%2Fdm-public-facing-upgrade-portal-video%2F04_DynamicImagery_AdvancedSettings_071917_BH.mp4&amp;config=/etc/dam/presets/viewer/Video_social&amp;serverUrl=https%3A%2F%2Fadobedemo62-h.assetsadobe.com%2Fis%2Fimage%2F&amp;contenturl=%2F&amp;config2=/etc/dam/presets/analytics&amp;videoserverurl=https://gateway-na.assetsadobe.com/DMGateway/public/demoCo&amp;posterimage=/content/dam/dm-public-facing-upgrade-portal-video/04_DynamicImagery_AdvancedSettings_071917_BH.mp4)
