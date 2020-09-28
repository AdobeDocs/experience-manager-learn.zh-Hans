---
title: 在AEM Dynamic Media中使用视频播放器
seo-title: 在AEM Dynamic Media中使用视频播放器
description: AEM Dynamic Media视频播放器过去依赖Flash运行时支持桌面客户端和浏览器上的自适应视频流，现在它们对基于Flash的内容流更加积极。 随着HLS(Apple的HTTP实时流视频投放协议)的推出，现在无需依赖闪存即可对内容进行流处理。
seo-description: AEM Dynamic Media视频播放器过去依赖Flash运行时支持桌面客户端和浏览器上的自适应视频流，现在它们对基于Flash的内容流更加积极。 随着HLS(Apple的HTTP实时流视频投放协议)的推出，现在无需依赖闪存即可对内容进行流处理。
uuid: aac6f471-4bed-4773-890f-0dd2ceee381d
discoiquuid: b01cc46b-ef64-4db9-b3b4-52d3f27bddf5
sub-product: 动态媒体
feature: media-player, video-profiles
topics: videos, renditions, authoring, best-practices
doc-type: feature video
audience: all
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '271'
ht-degree: 5%

---


# 在AEM Dynamic Media中使用视频播放器{#using-the-video-player-in-aem-dynamic-media}

AEM Dynamic Media视频播放器过去依赖Flash运行时支持桌面客户端和浏览器上的自适应视频流，现在它们对基于Flash的内容流更加积极。 随着HLS(Apple的HTTP实时流视频投放协议)的推出，现在无需依赖闪存即可对内容进行流处理。

>[!VIDEO](https://video.tv.adobe.com/v/16791/?quality=9&learn=on)

## 快速了解非Flash视频播放器 {#quick-look-into-non-flash-video-player}

>[!VIDEO](https://video.tv.adobe.com/v/17429/?quality=9&learn=on)

HLS浏览器支持如下，对于不支持的浏览器，我们回退到渐进式视频投放

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
   <td> <p>HLS视频流</p> </td>
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