---
title: 将全景和垂直图像查看器与AEM Assets Dynamic Media结合使用
description: AEM 6.4中的Dynamic Media Viewer增强功能包括添加了全景图像查看器、全景虚拟现实图像查看器和垂直图像查看器。 全景查看器提供了一种简单的方法，无需任何自定义开发即可为房间、财产、位置或景观提供引人入胜的沉浸式体验。
feature: Video Profiles, Video Profiles, 360 VR Video
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 6b2f7533-8ce0-4134-b1ae-b3c5d15a05e6
duration: 563
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '518'
ht-degree: 0%

---

# 将全景和垂直图像查看器与AEM Assets Dynamic Media结合使用{#using-panorama-and-vertical-image-viewer-with-aem-assets-dynamic-media}

AEM 6.4中的Dynamic Media Viewer增强功能包括添加了全景图像查看器、全景虚拟现实图像查看器和垂直图像查看器。 全景查看器提供了一种简单的方法，无需任何自定义开发即可为房间、财产、位置或景观提供引人入胜的沉浸式体验。

>[!VIDEO](https://video.tv.adobe.com/v/24156?quality=12&learn=on)

>[!NOTE]
>
>视频假定您的AEM实例在Dynamic Media S7模式下运行。 [有关使用Dynamic Media设置AEM的说明，请参阅此处。](https://helpx.adobe.com/cn/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## 全景和全景VR查看器

根据图像的纵横比或关键字，将图像视为全景。 默认情况下，纵横比为2的图像被视为全景图像。 如果满足上述条件，则全景图像查看器预设将可用于图像预览。 通过指定位于/conf/global/settings/cloudconfigs/dmscene7/jcr：content的双属性s7PanoramicAR，可以修改公司的DMS7配置中的全景图像长宽比标准。 关键字存储在资产元数据节点的dc：keyword属性中。 如果关键字包含以下任意组合：

* 等矩形，
* 球形+全景，
* 球形+全景，

它被视为全景图像资产，而不管其纵横比如何。

## 垂直图像查看器

对于水平色板，根据消费者的桌面屏幕大小，有时色板在用户向下滚动页面之前不可见。 通过使用垂直图像查看器并放置垂直色板，可以确保色板无论屏幕大小如何均可见。 它还可以最大化主图像大小。 对于水平色板，需要保留页面上的空间，以确保它们具有高度的可见性，并会减小主图像的大小。 对于垂直布局，无需担心分配此空间，因此可以最大化主图像大小。

<table> 
 <tbody>
  <tr>
   <td> </td>
   <td>全景和VR查看器</td>
   <td>垂直图像查看器</td>
  </tr>
  <tr>
   <td>Dynamic Media运行模式</td>
   <td>仅限Dynamic Media Scene7模式</td>
   <td>DMS7和Dynamic Media</td>
  </tr>
  <tr>
   <td>用例</td>
   <td><p>全景查看器和虚拟现实查看器为用户提供更加引人入胜的体验。 用户甚至可以在预订酒店之前就退房了，或者无需预约即可退房了。 用户可以签出位置和更多可能性。 这里的主要焦点是在消费者访问您的网站时为其提供更好的体验，并最终提高您的转化率。</p> <p> </p> </td> 
   <td><p>垂直图像查看器有助于最大化产品图像查看体验，为消费者提供最佳产品呈现，从而推动转化并最大程度地降低回报。</p> <p> </p> </td>
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
>全景查看器使用全景图像，不适用于正常图像。
