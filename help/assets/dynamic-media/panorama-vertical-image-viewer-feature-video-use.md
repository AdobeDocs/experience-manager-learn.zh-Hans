---
title: 将全景和垂直图像查看器与AEM Assets Dynamic Media结合使用
description: AEM 6.4中的Dynamic Media查看器增强功能包括添加全景图像查看器、虚拟现实全景图像查看器和垂直图像查看器。 全景查看器提供了一种轻松的方式，无需任何自定义开发，即可提供房间、资产、位置或景观的迷人、沉浸式体验。
feature: Video Profiles, Video Profiles, 360 VR Video
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: 6b2f7533-8ce0-4134-b1ae-b3c5d15a05e6
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '533'
ht-degree: 2%

---

# 将全景和垂直图像查看器与AEM Assets Dynamic Media结合使用{#using-panorama-and-vertical-image-viewer-with-aem-assets-dynamic-media}

AEM 6.4中的Dynamic Media查看器增强功能包括添加全景图像查看器、虚拟现实全景图像查看器和垂直图像查看器。 全景查看器提供了一种轻松的方式，无需任何自定义开发，即可提供房间、资产、位置或景观的迷人、沉浸式体验。

>[!VIDEO](https://video.tv.adobe.com/v/24156/?quality=9&learn=on)

>[!NOTE]
>
>视频假定您的AEM实例在Dynamic Media S7模式下运行。 [有关在Dynamic Media中设置AEM的说明，请参阅此处。](https://helpx.adobe.com/cn/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## 全景和全景VR查看器

根据图像的宽高比或关键字，将图像视为全景图。 默认情况下，宽高比为2的图像被视为全景图像。 如果图像符合上述条件，全景图像查看器预设便可用于图像预览。 在公司的DMS7配置中，可以通过在/conf/global/settings/cloudconfigs/dmscene7/jcr:content处指定双属性s7PanoramicAR来修改全景图像宽高比标准。 关键字存储在资产元数据节点的dc:keyword属性中。 如果关键词包含以下任意组合：

* 等矩形，
* 球面+全景，
* 球面+全景，

无论其长宽比如何，都会将其视为全景图像资产。

## 垂直图像查看器

使用水平色板时，根据消费者的桌面屏幕大小，有时只有用户向下滚动页面后，才会显示色板。 通过使用垂直图像查看器并放置垂直色板，它可确保无论屏幕大小如何，都可以看到色板。 它还可最大化主图像大小。 使用水平色板时，必须保留页面上的空间，以确保这些色板具有很高的可见性，并且会减小主图像的大小。 使用垂直布局，您无需担心分配此空间，因此可以最大化主图像大小。

<table> 
 <tbody>
  <tr>
   <td> </td>
   <td>全景和VR查看器</td>
   <td>垂直图像查看器</td>
  </tr>
  <tr>
   <td>Dynamic Media运行模式</td>
   <td>仅Dynamic Media Scene7模式</td>
   <td>DMS7和Dynamic Media</td>
  </tr>
  <tr>
   <td>用例</td>
   <td><p>全景查看器和虚拟现实查看器为用户提供了更吸引人的体验。 用户可以在预订前入住酒店房间，或者无需预约即可入住租赁房产。 用户可以查出位置，并且还有许多可能性。 这里的主要重点是为消费者在访问您的网站时提供更好的体验，并最终提高您的转化率。</p> <p> </p> </td> 
   <td><p>垂直图像查看器有助于最大限度地提升产品图像查看体验，从而为消费者提供产品的最佳呈现方式，从而促进转化并最大限度地降低回报。</p> <p> </p> </td>
  </tr>
  <tr>
   <td>可用 </td>
   <td>OOTB</td>
   <td>OOTB</td>
  </tr>
 </tbody>
</table>

[在Scene7模式下配置Dynamic Media](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)

[在混合模式下配置Dynamic Media](https://helpx.adobe.com/cn/experience-manager/6-5/assets/using/config-dynamic.html)

>[!NOTE]
>
>全景查看器可处理全景图像，不适用于普通图像。
