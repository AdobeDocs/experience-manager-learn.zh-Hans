---
title: 在AEM Dynamic Media中使用视频播放器
description: AEM Dynamic Media视频播放器过去依赖Flash运行时支持桌面客户端上的自适应视频流播放，而在基于flash的内容流播放中，浏览器变得更加积极。 随着HLS(Apple的HTTP实时流视频交付协议)的推出，现在无需依赖闪存即可对内容进行流式处理。
sub-product: dynamic-media
feature: Video Profiles
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: 7e4cb782-836d-4ec0-97d0-645b91ea43e0
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 5%

---


# 在AEM Dynamic Media中使用视频播放器{#using-the-video-player-in-aem-dynamic-media}

AEM Dynamic Media视频播放器过去依赖Flash运行时支持桌面客户端上的自适应视频流播放，而在基于flash的内容流播放中，浏览器变得更加积极。 随着HLS(Apple的HTTP实时流视频交付协议)的推出，现在无需依赖闪存即可对内容进行流式处理。

>[!VIDEO](https://video.tv.adobe.com/v/16791/?quality=9&learn=on)

## 快速查看非Flash视频播放器 {#quick-look-into-non-flash-video-player}

>[!VIDEO](https://video.tv.adobe.com/v/17429/?quality=9&learn=on)

HLS浏览器支持如下所示，对于不受支持的浏览器，我们回退到渐进式视频交付

>[!NOTE]
>
> Dynamic Media Hybrid自2022年3月15日起不支持在Internet Explorer 11上进行视频流播放。 请升级到6.5.12或更高版本，以便回退到IE 11上的渐进式播放。

<table> 
 <thead> 
  <tr> 
   <th> <p>设备</p> </th>
   <th> <p>浏览器</p> </th>
   <th > <p>视频播放模式</p> </th>
  </tr>
 </thead>
 <tbody>
  <tr> 
   <td> <p>桌面设备</p> </td>
   <td> <p>Internet Explorer 9和10</p> </td>
   <td> <p>渐进式下载</p> </td>
  </tr>
  <tr>
   <td> <p>桌面设备</p> </td>
   <td> <p>Internet Explorer 11+</p> </td>
   <td> <p>Dynamic Media - Scene 7模式：HLS视频流</p> 
        <p>Dynamic Media — 混合模式：渐进式下载</p>
   </td>
  </tr>
  <tr>
   <td> <p>桌面设备</p> </td>
   <td> <p>Firefox 23-44</p> </td>
   <td> <p>渐进式下载</p> </td>
  </tr>
  <tr> 
   <td> <p>桌面设备</p> </td>
   <td> <p>Firefox 45或更高版本</p> </td>
   <td> <p>HLS视频流</p> </td>
  </tr>
  <tr> 
   <td> <p>桌面设备</p> </td>
   <td> <p>铬黄</p> </td>
   <td> <p>HLS视频流</p> </td>
  </tr>
  <tr> 
   <td> <p>桌面设备</p> </td>
   <td> <p>Safari(Mac)</p> </td>
   <td> <p>HLS视频流</p> </td>
  </tr>
  <tr> 
   <td> <p>移动设备</p> </td>
   <td> <p>Chrome（Android 6或更早版本）</p> </td>
   <td> <p>渐进式下载</p> </td>
  </tr>
  <tr> 
   <td> <p>移动设备</p> </td>
   <td> <p>Chrome（Android 7或更高版本）</p> </td>
   <td> <p>HLS视频流</p> </td>
  </tr>
  <tr> 
   <td> <p>移动设备</p> </td>
   <td> <p>Android（默认浏览器）</p> </td>
   <td> <p>渐进式下载</p> </td>
  </tr>
  <tr> 
   <td> <p>移动设备</p> </td>
   <td> <p>Safari(iOS)</p> </td>
   <td> <p>HLS视频流</p> </td>
  </tr>
  <tr> 
   <td> <p>移动设备</p> </td>
   <td> <p>Chrome(iOS)</p> </td>
   <td> <p>HLS视频流</p> </td>
  </tr>
  <tr> 
   <td> <p>移动设备</p> </td>
   <td> <p>黑莓</p> </td>
   <td> <p>HLS视频流</p> </td>
  </tr>
 </tbody>
</table>
