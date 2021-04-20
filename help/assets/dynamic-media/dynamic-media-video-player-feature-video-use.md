---
title: 在AEM Dynamic Media中使用视频播放器
description: AEM Dynamic Media视频播放器过去依赖Flash运行时支持桌面客户端和浏览器上的自适应视频流，现在在基于flash的内容流上变得更加积极。 随着HLS(苹果的HTTP实时流视频投放协议)的推出，现在无需依赖闪存即可对内容进行流处理。
sub-product: dynamic-media
feature: Video Profiles
version: 6.3, 6.4, 6.5
topic: Content Management
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 8%

---


# 在AEM Dynamic Media中使用视频播放器{#using-the-video-player-in-aem-dynamic-media}

AEM Dynamic Media视频播放器过去依赖Flash运行时支持桌面客户端和浏览器上的自适应视频流，现在在基于flash的内容流上变得更加积极。 随着HLS(苹果的HTTP实时流视频投放协议)的推出，现在无需依赖闪存即可对内容进行流处理。

>[!VIDEO](https://video.tv.adobe.com/v/16791/?quality=9&learn=on)

## 快速查看非Flash视频播放器{#quick-look-into-non-flash-video-player}

>[!VIDEO](https://video.tv.adobe.com/v/17429/?quality=9&learn=on)

HLS浏览器支持如下所示，对于不支持的浏览器，我们回退到渐进式视频投放

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