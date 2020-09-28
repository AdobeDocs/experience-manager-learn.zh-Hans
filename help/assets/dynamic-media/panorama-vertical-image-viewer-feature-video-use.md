---
title: 将全景图和垂直图像查看器与AEM AssetsDynamic Media结合使用
seo-title: 将全景图和垂直图像查看器与AEM AssetsDynamic Media结合使用
description: AEM 6.4中的Dynamic Media Viewer增强功能包括添加全景图像查看器、全景虚拟现实图像查看器和垂直图像查看器。 全景查看器提供了一种轻松的方式，无需任何自定义开发，即可提供引人入胜的房间、房产、位置或景观体验。
seo-description: AEM 6.4中的Dynamic Media Viewer增强功能包括添加全景图像查看器、全景虚拟现实图像查看器和垂直图像查看器。 全景查看器提供了一种轻松的方式，无需任何自定义开发，即可提供引人入胜的房间、房产、位置或景观体验。
sub-product: 动态媒体
feature: video-profiles, video-profiles, vr-360
topics: videos, renditions, authoring
doc-type: feature video
audience: all
activity: use
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 1%

---


# 将全景图和垂直图像查看器与AEM AssetsDynamic Media结合使用{#using-panorama-and-vertical-image-viewer-with-aem-assets-dynamic-media}

AEM 6.4中的Dynamic Media Viewer增强功能包括添加全景图像查看器、全景虚拟现实图像查看器和垂直图像查看器。 全景查看器提供了一种轻松的方式，无需任何自定义开发，即可提供引人入胜的房间、房产、位置或景观体验。

>[!VIDEO](https://video.tv.adobe.com/v/24156/?quality=9&learn=on)

>[!NOTE]
>
>视频假定AEM实例在Dynamic Media S7模式下运行。 [有关使用Dynamic Media设置AEM的说明，请参阅此处。](https://helpx.adobe.com/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## 全景和全景VR查看器

根据图像的长宽比或关键字将图像视为全景图。 默认情况下，长宽比为2的图像将被视为全景图像。 如果图像预览符合上述条件，全景图像查看器预设将可用。 在公司的DMS7配置中，可以通过在/conf/global/settings/cloudconfigs/dmscene7/jcr:content指定多次属性s7PanoramicAR来修改全景图像宽高比标准。 关键字存储在资产元数据节点的dc:keyword属性中。 如果关键字包含以下任意组合：

* 等矩形，
* 球面+全景，
* 球面+全景图，

无论其长宽比如何，它都将被视为全景图像资产。

## 垂直图像查看器

使用水平色板（具体取决于消费者的桌面屏幕大小）时，有时只有用户向下滚动页面后，色板才可见。 通过使用垂直图像查看器并放置垂直色板，它可以确保无论屏幕大小如何，色板都可见。 它还可最大化主图像大小。 使用水平色板时，必须保留页面上的空间，以确保它们很有可能可见，并减小主图像的大小。 使用垂直布局，您无需担心分配此空间，因此可以最大化主图像大小。

<table> 
 <tbody>
  <tr>
   <td> </td>
   <td>全景和VR查看器</td>
   <td>垂直图像查看器</td>
  </tr>
  <tr>
   <td>Dynamic Media运行模式</td>
   <td>仅Dynamic MediaScene7模式</td>
   <td>DMS7和Dynamic Media</td>
  </tr>
  <tr>
   <td>用例</td>
   <td><p>全景查看器和虚拟现实查看器为用户提供了更具吸引力的体验。 用户可以在预订前入住酒店房间，也可以在无需计划预约的情况下入住租赁房。 用户可以查看位置和更多可能性。 此处的主要重点是为消费者在访问您的网站时提供更好的体验，并最终提高转化率。</p> <p> </p> </td> 
   <td><p>垂直图像查看器有助于最大化产品图像查看体验，为消费者提供产品的最佳表现形式，从而推动转化并最大限度地降低回报。</p> <p> </p> </td>
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
>全景查看器可处理全景图像，不能用于正常图像。
