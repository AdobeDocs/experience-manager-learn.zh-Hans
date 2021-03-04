---
title: Dynamic Media与AEM Assets概述
description: 此视频系列概述了如何使用Adobe Experience Manager Dynamic Media作为内容服务服务来管理和访问媒体内容。 Dynamic Media可让您管理和发布动态数字体验 — 这是Experience Manager资产特有的功能。 我们的框架和组件套件使营销人员能够在所有设备上自定义和提供交互式多媒体体验。
sub-product: dynamic-media
feature: 智能裁剪、用户档案、图像用户档案、查看器预设、360 VR视频、图像集、旋转集
version: 6.3, 6.4, 6.5
topic: 内容管理
role: 业务从业者
level: 初学者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '891'
ht-degree: 0%

---


# 将Dynamic Media与AEM Assets {#understanding-aem-dynamic-media}一起使用

此多部分视频系列概述了如何使用Adobe Experience Manager Dynamic Media作为内容服务服务来管理和访问媒体内容。 Dynamic Media可让您管理和发布动态数字体验 — 这是Experience Manager资产特有的功能。 我们的框架和组件套件使营销人员能够在所有设备上自定义和提供交互式多媒体体验。

## Dynamic Media概述

>[!VIDEO](https://video.tv.adobe.com/v/27144/?quality=9&learn=on)

>[!NOTE]
>
>此处演示的功能可用于Dynamic Media DMS7运行模式，我们当前支持的运行模式，但不一定是DMHybrid运行模式，DMS7已替换该模式。

此视频介绍如何使用Adobe Experience Manager Dynamic Media作为内容服务服务来管理和访问媒体内容。 Dynamic Media使用单一主控资产方法进行操作，您可以在该方法中上传图像资产或视频资产，以便请求这些资产满足无限数量的所需消耗变量或衍生演绎版。 包括：

* 单个主控资产到URL产品交付内容说明
* 图像处理选项
* 内容查看器选项
* 将URL复制到图像和响应式查看器
* 智能裁切变体到URL

## Dynamic Media与AEM Sites及任何其他CMS

>[!VIDEO](https://video.tv.adobe.com/v/27145/?quality=9&learn=on)

>[!NOTE]
>
>此处演示的功能可用于Dynamic Media DMS7运行模式，我们当前支持的运行模式，但不一定是DMHybrid运行模式，DMS7已替换该模式。 此视频引用了第1部分视频(Dynamic Media概述)中介绍的概念。

此视频介绍如何在Adobe Experience Manager Dynamic Media中管理媒体内容，以及如何在AEM Sites中借助组件轻松使用媒体内容，以便根据响应式页面宽度进行简单、自动裁剪以进行优化。 轻松创建交互式图像横幅并生成要在任何内容管理系统中使用的复制URL。

* AEM Sites Dynamic Media组件灵活性
* 使用图像预设在本地下载
* 创建和发布交互式横幅

## Dynamic Media：构建混合Media Collection和自定义查看器

>[!VIDEO](https://video.tv.adobe.com/v/27146/?quality=9&learn=on)

>[!NOTE]
>
>此处演示的功能可用于Dynamic Media DMS7运行模式，我们当前支持的运行模式，但不一定是DMHybrid运行模式，DMS7已替换该模式。 此视频引用了第1部分视频(Dynamic Media概述)中介绍的概念。

此视频描述了混合媒体查看器媒体资产集合（包括旋转集、视频和产品图像集）的简单创建过程。 将内容添加到混合媒体集，并创建自定义查看器以在最终的复制URL或AEM Sites组件中进行选择。

* 从相应的产品照片创建旋转集
* 为Dynamic Media Video上传主视频并进行编码
* 从旋转集、视频和照片创建混合媒体集
* 编辑和使用自定义混合媒体查看器

## Dynamic Media图像预设

>[!VIDEO](https://video.tv.adobe.com/v/27320/?quality=9&learn=on)

此视频介绍图像预设的创建方式以及图像预设的内容。图像预设是指一系列图像服务器参数的URL缩短服务器，每当URL请求图像时，这些参数都会对图像进行操作。 了解扩展和编辑图像预设的有价技术。

* 图像预设缩短工具隐藏显式图像服务器命令的集合
* 仅使用一个像素尺寸 — 宽度或高度 — 来调整新调整大小的图像，而无需填充
* 始终使用锐化
* 用于添加额外命令以调整图像预设大小的URL修饰符字段

## Dynamic Media高级URL修饰符

>[!VIDEO](https://video.tv.adobe.com/v/27319/?quality=9&learn=on)

此视频描述了通过调整图像大小来充分利用源文件本身的功能 — 背景透明度，内置在剪切路径中，并将裁切和文本作为变量 — 以及Dynamic Media的URL修饰符。

* 在“Dynamic Media修饰符”字段中使用URL修饰符
* 更改具有透明度的图像的背景颜色
* 剪切到图像路径
* 裁剪到图像路径
* 从Photoshop文件创建文本模板

## Dynamic Media控制JPEG文件大小（以千字节为单位）

>[!VIDEO](https://video.tv.adobe.com/v/27404/?quality=9&learn=on)


>[!NOTE]
>
>图像质量按反向压缩的百分比测量，其中100%的质量压缩最小，结果是高质量图像，但文件大小相对较大。 Jpeg压缩是一种有损压缩方案，压缩设置决定图像质量和文件大小。

使用2个命令调整jpeg压缩设置，使jpeg图像质量与生成的文件大小（以千字节为单位）保持平衡，以提高页面加载速度。 QLT通过调整jpeg压缩质量设置来定义图像质量。 “JPEG大小”命令允许您指定使用压缩需要达到的文件大小。

## 将CC隐藏式字幕添加到Dynamic Media Video

>[!VIDEO](https://video.tv.adobe.com/v/28074/?quality=9&learn=on)

通过附加Copy URL以指向附加的Closed Captioning文件文档（web.VTT附加文件，包含任何视频的CC信息），将Closed Captioning轻松添加到Dynamic Media视频。

## 在AEM Dynamic Media中使用图像锐化

此视频介绍了锐化图像对保持图像保真度至关重要的原因，以及如何使用高级设置制作完美图像的原因。

>[!VIDEO](https://demos-pub.assetsadobe.com/etc/dam/viewers/s7viewers/html5/VideoViewer.html?asset=%2Fcontent%2Fdam%2Fdm-public-facing-upgrade-portal-video%2F04_DynamicImagery_AdvancedSettings_071917_BH.mp4&amp;config=/etc/dam/presets/viewer/Video_social&amp;serverUrl=https%3A%2F%2Fadobedemo62-h.assetsadobe.com%2Fis%2Fimage%2F&amp;contenturl=%2F&amp;config2=/etc/dam/presets/analytics&amp;videoserverurl=https://gateway-na.assetsadobe.com/DMGateway/public/demoCo&amp;posterimage=/content/dam/dm-public-facing-upgrade-portal-video/04_DynamicImagery_AdvancedSettings_071917_BH.mp4)
