---
title: Dynamic Media与AEM Assets概述
description: 此视频系列概述了如何使用Adobe Experience Manager Dynamic Media作为内容服务来管理和访问媒体内容。 Dynamic Media允许您管理和发布动态数字体验 — 这是Experience Manager资产特有的功能。 我们的框架和组件套件允许营销人员在所有设备上自定义和提供交互式多媒体体验。
sub-product: dynamic-media
feature: 智能裁剪、视频配置文件、图像配置文件、查看器预设、360 VR视频、图像集、旋转集
version: 6.3, 6.4, 6.5
topic: 内容管理
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '888'
ht-degree: 0%

---


# 将Dynamic Media与AEM Assets结合使用 {#understanding-aem-dynamic-media}

此多部分视频系列概述了如何使用Adobe Experience Manager Dynamic Media作为内容服务来管理和访问媒体内容。 Dynamic Media允许您管理和发布动态数字体验 — 这是Experience Manager资产特有的功能。 我们的框架和组件套件允许营销人员在所有设备上自定义和提供交互式多媒体体验。

## Dynamic Media概述

>[!VIDEO](https://video.tv.adobe.com/v/27144/?quality=9&learn=on)

>[!NOTE]
>
>此处演示的功能可以在Dynamic Media DMS7运行模式中使用，我们当前支持的运行模式不一定是DMHybrid运行模式，DMS7已取代了该模式。

此视频介绍如何使用Adobe Experience Manager Dynamic Media作为内容服务来管理和访问媒体内容。 Dynamic Media在单个主控资产方法上运行，在该方法中，您可以上传图像资产或视频资产，以执行无限数量的所需可消耗变体或衍生演绎版。 包括：

* 单个主控资产到URL产品交付结果说明
* 图像处理选项
* 内容查看器选项
* 将URL复制到图像和响应式查看器
* 智能裁剪URL变体

## Dynamic Media与AEM Sites以及任何其他CMS

>[!VIDEO](https://video.tv.adobe.com/v/27145/?quality=9&learn=on)

>[!NOTE]
>
>此处演示的功能可以在Dynamic Media DMS7运行模式中使用，我们当前支持的运行模式不一定是DMHybrid运行模式，DMS7已取代了该模式。 此视频引用第1部分视频(Dynamic Media概述)中描述的概念。

此视频介绍如何在Adobe Experience Manager Dynamic Media中管理媒体内容，以及如何通过组件在AEM Sites中轻松使用媒体内容，以便进行简单且自动的裁剪以根据响应式页面宽度进行优化。 轻松创建交互式图像横幅并生成要在任何内容管理系统中使用的复制URL。

* AEM Sites Dynamic Media组件灵活性
* 使用图像预设从本地下载
* 创建和发布交互式横幅

## Dynamic Media构建混合媒体收集和自定义查看器

>[!VIDEO](https://video.tv.adobe.com/v/27146/?quality=9&learn=on)

>[!NOTE]
>
>此处演示的功能可以在Dynamic Media DMS7运行模式中使用，我们当前支持的运行模式不一定是DMHybrid运行模式，DMS7已取代了该模式。 此视频引用第1部分视频(Dynamic Media概述)中描述的概念。

此视频介绍了混合媒体查看器媒体资产集合的简单创建过程，包括旋转集、视频和产品图像集合。 将内容添加到混合媒体集，并创建一个自定义查看器，以在最终的复制URL或AEM Sites组件中进行选择。

* 从相应的产品照片创建旋转集
* 上传和编码Dynamic Media视频的主视频
* 从旋转集、视频和照片创建混合媒体集
* 编辑和使用自定义混合媒体查看器

## Dynamic Media图像预设

>[!VIDEO](https://video.tv.adobe.com/v/27320/?quality=9&learn=on)

此视频介绍图像预设的创建方式以及图像预设的内容。图像预设是一种URL缩短服务，可让一系列图像服务器参数在URL请求时对图像执行操作。 了解扩展和编辑图像预设的有用技术。

* 图像预设缩短服务隐藏显式图像服务器命令的集合
* 仅使用一个像素尺寸 — 宽度或高度 — 来符合新的大小调整图像，而无需填充
* 始终使用锐化
* 用于为调整图像预设大小添加额外命令的URL修饰符字段

## Dynamic Media高级URL修饰符

>[!VIDEO](https://video.tv.adobe.com/v/27319/?quality=9&learn=on)

此视频介绍如何调整图像大小以利用源文件本身的功能 — 背景透明度、内置剪贴路径以及作为变量的裁剪和文本 — 以及Dynamic Media的URL修饰符。

* 在“Dynamic Media修饰符”字段中使用URL修饰符
* 更改具有透明度的图像的背景颜色
* 剪切到图像路径
* 裁剪到图像路径
* 从Photoshop文件创建文本模板

## Dynamic Media控制JPEG文件大小（千字节）

>[!VIDEO](https://video.tv.adobe.com/v/27404/?quality=9&learn=on)


>[!NOTE]
>
>图像质量以反向压缩的百分比进行测量，其中100%的质量压缩最小，从而生成高质量图像，但文件大小相对较大。 Jpeg压缩是一种有损压缩方案，其中压缩设置决定图像质量和文件大小。

使用2个命令调整jpeg压缩设置，以平衡jpeg图像质量与生成的文件大小（以千字节为单位），从而提高页面加载速度。 QLT通过调整jpeg压缩质量设置来定义图像质量。 “JPEG大小”命令允许您指定使用压缩需要实现的文件大小。

## 将CC隐藏式字幕添加到Dynamic Media视频

>[!VIDEO](https://video.tv.adobe.com/v/28074/?quality=9&learn=on)

通过附加复制URL以指向附加的隐藏式字幕文件文档（web.VTT Sidecar文件，其中包含任何视频的CC信息），可轻松地将隐藏式字幕添加到Dynamic Media视频。

## 在AEM Dynamic Media中使用图像锐化

此视频介绍了锐化图像对保持图像保真度至关重要的原因，以及如何使用高级设置来制作完美图像的原因。

>[!VIDEO](https://demos-pub.assetsadobe.com/etc/dam/viewers/s7viewers/html5/VideoViewer.html?asset=%2Fcontent%2Fdam%2Fdm-public-facing-upgrade-portal-video%2F04_DynamicImagery_AdvancedSettings_071917_BH.mp4&amp;config=/etc/dam/presets/viewer/Video_social&amp;serverUrl=https%3A%2F%2Fadobedemo62-h.assetsadobe.com%2Fis%2Fimage%2F&amp;contenturl=%2F&amp;config2=/etc/dam/presets/analytics&amp;videoserverurl=https://gateway-na.assetsadobe.com/DMGateway/public/demoCo&amp;posterimage=/content/dam/dm-public-facing-upgrade-portal-video/04_DynamicImagery_AdvancedSettings_071917_BH.mp4)
